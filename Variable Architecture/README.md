# Variable Architecture

> Idea from - [Ryan Hipple - Unite 2017](https://www.youtube.com/watch?v=raQ3iHhE_Kk&t=2713s)

***Contact info in the end***

---

#### Concept

​	Variables as SO to create a layer of communication between objects that need info from some shared state

#### Simple scenario

​	When a local variable needs to be used by another behavior component, instead of creating a reference between current objects, create a variable in asset and make the two objects to have a reference to it

#### How to use

1. Create a new SO of the wishing kind
2. Inside script write the kind: e.g., `public IntReference sharedState;` 
3. In inspector select "*use variable*" (see image below)
4. Inject variable

![Imgur](https://i.imgur.com/bBUb3VH.png)

#### How it works (basically)

​	The idea is divided into two parts

1. A generic base class

```c#
public abstract class BaseVariable<T> : ScriptableObject
{
    public bool haveDefaultValue = false;
    
    [SerializeField]
    private T value;
    private T runTimeValue;
    
    public T Value
    {
        get => haveDefaultValue ? runTimeValue : value;
        set
        {
            if (haveDefaultValue)
                runTimeValue = value;
            else
                this.value = value;
        }
    }

    private void OnEnable()
    {
        if (haveDefaultValue)
            runTimeValue = value;
    }
    
    private void OnDisable()
    {
        if (haveDefaultValue)
            runTimeValue = value;
    }
}
```

And an SO to each necessary kind

```c#
[CreateAssetMenu(fileName = "IntVariable", menuName = "Framework/Variables/Int")]
public class IntVariable : BaseVariable<int>
{ }
```

2. A "reference" to be created inside components that will share some state. This approach also is important to create a more designer-friendly system, turning possible to change variables without touch in code using inspector as an injector

```c#
using System;

[Serializable]
public class IntReference
{
    public bool UseConstant = true;
    public int ConstantValue;
    public IntVariable Variable;

    public IntReference()
    { }

    public IntReference(int value)
    {
        UseConstant = true;
        ConstantValue = value;
    }

    public int Value
    {
        get => UseConstant ? ConstantValue : Variable.Value; 
        set
        {
            if (UseConstant)
                ConstantValue = value;
            else
                Variable.Value = value;
        }
    }

    public static implicit operator int(IntReference reference)
        => reference.Value;
}
```

---

## Contact

> If you have doubts, suggestions, criticizes or only want to talk

Nefisto – joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
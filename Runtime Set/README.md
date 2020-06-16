# Runtime sets

> Idea from - [[Ryan Hipple - Unite 2017](https://www.youtube.com/watch?v=raQ3iHhE_Kk)]

***Contact info in the end\***

---

#### Concept

​	Sometimes is necessary to keep track of some group of objects that exist in scene at an arbitrary moment. To solve this kind of problem what is suggested is that we use an SO to store this kind of info.

#### How to use

1. Create an SO that will represent our group
2. Insert *RuntimeItem* component in objects that will belong to group created above
3. When you need to know about this group, only make an reference to the *RuntimeSet* SO that will be injected by inspector

```c#
...
public RuntimeSet trackableGroup;
...
trackableGroup.Items.Find(...);
```

> OBS: The script that have reference will need to get necessary components

#### How it work (basically)

1. One abstract class that represents a group of *things*

```c#
[CreateAssetMenu(fileName= "RuntimeSet", menuName= "RuntimeSet")]
public class RuntimeSet : ScriptableObject
{
    public List<RuntimeItem> Items = new List<RuntimeItem>();

    public void Add(RuntimeItem thing)
    {
        if (!Items.Contains(thing))
            Items.Add(thing);
    }

    public void Remove(RuntimeItem thing)
    {
        if (Items.Contains(thing))
            Items.Remove(thing);
    }
}
```

2. An component to set game object as *trackable*

```c#
public class RuntimeItem : MonoBehaviour
{
    public RuntimeSet runtimeSet;

    private void OnEnable()
        => runtimeSet.Add(this);

    private void OnDisable()
        => runtimeSet.Remove(this);
}
```

---

## Contact

​	If you have doubts, suggestions, criticizes or only want to talk, the best way is mailing me in joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
# Singletons as SO n MONO

> SO_singletons -> Idea from - [Richar Fine - Unite 2016](https://www.youtube.com/watch?v=6vmRwLYWNRo) 

***Contact info in the end\***

---

#### Concept

​	To avoid some kind of chaos scenarios from overusing more than the necessary amount of singletons, aka manager, we're providing a base class that uses a singleton as an asset, in this case, we can assume that the singleton will be available at any time, so skipping the use of a preload scene.

#### How to use

1. Create a script the inherit from *ScriptableSingleton<T>*

```c#
[CreateAssetMenu(fileName = "SceneController", menuName = "Managers/Scene")]
public class SceneController : ScriptableSingleton<SceneController>
{   
    public void ChangeScene(int sceneIndex)
       => SceneManager.LoadScene(sceneIndex);
}
```

2. **In this approach the singleton need to exist in a folder called \*Resources\*** that is an special folder in unity where we can find assets via `Resources.Load()`
3. From now on you use the singleton as you are used to `SceneController.Instance.ChangeScene(0)`

#### How it works

1. An generic class that'll find singleton inside resource folder.

```c#
using System.Linq;
using UnityEngine;

public abstract class ScriptableSingleton<T> : ScriptableObject 
    where T : ScriptableObject
{
    protected static T _instance;
    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                var type = typeof(T);
                var instances = Resources.LoadAll<T>(string.Empty);
                _instance = instances.FirstOrDefault();
                if (_instance == null)
                {
                    Debug.LogErrorFormat("[ScriptableSingleton] No instance of {0} found!", type.ToString());
                }
                else if (instances.Count() > 1)
                {
                    Debug.LogErrorFormat("[ScriptableSingleton] Multiple instances of {0} found!", type.ToString());
                }
            }
            return _instance;
        }
    }
}
```

---

## Contact

​	If you have doubts, suggestions, criticizes or only want to talk, the best way is mailing me in joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
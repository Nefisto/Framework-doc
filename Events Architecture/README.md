# Events architecture

> Idea from - [Ryan Hipple - Unite 2017](https://www.youtube.com/watch?v=raQ3iHhE_Kk&t=2713s)

***Contact info in the end***

---

#### Concept

​	Highlight events that happen during execution (e.g. pause, movement-based in a grid) and create a layer that detaches objects that are responsible to trigger that event and those who are listeners. It's a simple event idea, but using unity inspector to control who triggers and who listener, in this way we try to avoid "update chaos" and turn the game flow easily to read because most of the behavior that is happening you can see in the inspector

#### How to use

1. Create a new SO of the wishing event type
2. Inside our object that will be a trigger, create the reference and call it using *invoke* wherever you want

```c#
...
public GameEvent onDeath;
...
public void Death()
    => onDeath.Raise()
```

3. In object that will act as a listener, add the component (Monobehavior) *GameEventListener*, be careful to add listener from the same type of event
   ![Imgur](https://i.imgur.com/YSVD9sk.png)

4. Set methods that will be called when the event's called

> OBS: You cannot control the order that the things will happen

#### How it works (basically)

1. A class that represents the event and expose ways to register and unregister from the event

   > OBS: This class is recreated for each event type

```c#
[CreateAssetMenu(fileName = "GameEvent", menuName = "Events/GameEvent(void)")]
public class GameEvent : ScriptableObject
{
    private readonly List<GameEventListener> eventListeners = new List<GameEventListenerInt>();

    public void Raise()
    {
        for (int i = eventListeners.Count - 1; i >= 0; i--)
            eventListeners[i].OnEventRaised();
    }

    public void RegisterListener(GameEventListener listener)
    {
        if (!eventListeners.Contains(listener))
            eventListeners.Add(listener);
    }

    public void UnregisterListener(GameEventListener listener)
    {
        if (eventListeners.Contains(listener))
            eventListeners.Remove(listener);
    }
}
```

2. A listener as a component to register in some event by injecting event in the inspector

   > OBS: To serialize typed events, Unity requires that we create a parameterized version of UnityEvent
   >
   > ```c#
   > [Serializable]
   > public class MyIntEvent : UnityEvent<int> {}
   > ```

```c#
public class GameEventListener : MonoBehaviour
{
    [Tooltip("Event to register with.")]
    public GameEvent Event;

    [Tooltip("Response to invoke when Event is raised.")]
    public UnityEvent Response;

    private void OnEnable()
    {
        Event.RegisterListener(this);
    }

    private void OnDisable()
    {
        Event.UnregisterListener(this);
    }

    public void OnEventRaised()
    {
        Response.Invoke();
    }
}
```



#### Features

* To simplify debug and test work, was added a button to invoke the event

  > For obvious reasons the event can be called only in runtime

![Imgur](https://i.imgur.com/KgJvQRY.png)

---

## Contact

​	If you have doubts, suggestions, criticizes or only want to talk, the best way is mailing me in joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
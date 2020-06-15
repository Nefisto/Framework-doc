# Events architecture

> Idea from - [Ryan Hipple - Unite 2017](https://www.youtube.com/watch?v=raQ3iHhE_Kk&t=2713s)

---

## Overview

​	Destacar eventos que acontecem durante a execução, como por exemplo um pause, e criar uma camada que separa os objetos que são responsáveis por disparar o evento (*triggers*) e aqueles que respondem a esse evento (*listeners*)

#### Quando usar

​	Sempre que possível

#### Como usar

1. Crie um novo SO do tipo de evento desejado 

2. Dentro do objeto que ira atuar como *trigger*, crie a variável e faça seu *invoke* no lugar desejado

   ```c#
   ...
   public GameEvent onDeath;
   ...
   public void Death()
       => onDeath.Raise()
   ```

3. Nos objetos que atuarão como *listeners*, adicione o componente *GameEventListener* (do mesmo tipo do evento)
   ![Imgur](https://i.imgur.com/YSVD9sk.png)

4. Arraste o evento ao qual o objeto ira responder e insira os comportamentos a serem executados na lista de *response*

> OBS: Não há garantia de ordem de execução para a resposta (até onde eu sei)

#### Como funciona

1. Uma classe que representa o evento e expõe maneiras de se registrar e se remover como *listener* do evento.

   > OBS: Essa classe é recriada para cada evento de tipo distinto

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

​	Para facilitar o trabalho de depuração é escrito um editor que permite que o evento seja invocado direto do *inspector*

![Imgur](https://i.imgur.com/KgJvQRY.png)

> OBS: por razões obvias o evento só pode ser chamado em *runtime*

2. Há um componente de *monobehaviour* que atua como listener para um evento qualquer, do mesmo tipo, e executa os eventos quando o *trigger* é ativado

   > OBS: Para eventos tipados é necessário criar uma classes serializavel que herde de *UnityEvent* 
   >
   > ex:  Para um evento de *int*
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



[to up](

## Release History

* 0.0.1
  * Base version

---

## Contact

​	If you have doubts, suggestions, criticizes or only want to talk, the best way is mailing me in joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
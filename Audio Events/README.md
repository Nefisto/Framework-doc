# Audio events

> Idea from - [Richar Fine - Unite 2016](https://www.youtube.com/watch?v=6vmRwLYWNRo) 

***Contact info in the end\***

---

#### Concept

​	To control 

​	Para controlar a adição de áudio no projeto e melhorar a experiência de controle, é utilizado um padrão de *delegate* para criar comportamentos distintos entre arquivos que controlam os áudios

#### Quando usar

​	É possível criar comportamentos distintos para grupos distintos de áudio, como por exemplo, um comportamento que randomiza um áudio dentro de uma *array* de *AudioClips* com ou sem peso, que alterna entre áudio, etc... e todos por ser injetados da mesma maneira pelo *inspector* e/ou chamados por código. Enfim, **use sempre que possível**

#### Como usar

1. Crie e configure um novo SO do evento de áudio desejado 
   (botão direito -> Áudio events)

2. Na classe responsável por executar o áudio crie uma referencia para a classe base e chame seu método Play() pelo código OU **pelo *inspector* por meio de eventos**

   ```c#
   ...
   public AudioEvent burp;
   ...
   ```

3. Arraste o áudio criado no passo 1

#### Como funciona

1. Uma classe base que garante a existência do método *play* (note que é usado uma classe abstrata pois o *inspector* do *unity* não permite que interfaces sejam injetadas por ele, não de maneira nativa pelo menos)
2. Agora é criado diferentes tipos de áudio que herdem da classe acima, implementa comportamentos distintos para cada um e permitindo que eles sejam criados por meio de SO
3. É implementado um *custom editor* para permitir que o som seja testado a qualquer momento 

![Imgur](https://i.imgur.com/IedhXx7.png)

> OBS 1: A variável  de volume e *pitch* são customizados para possuir um mínimo e máximo
>
> OBS 2: A imagem é apenas um exemplo, cada classe vai ter seu próprio meio de aparecer no *inspector*

[to up](

---

## Contact

​	If you have doubts, suggestions, criticizes or only want to talk, the best way is mailing me in joao.gavron@gmail.com

Distributed in asset store under the [Unity license](https://unity3d.com/legal/as_terms?_ga=2.91212574.56628704.1591012418-1089589826.1583496471).
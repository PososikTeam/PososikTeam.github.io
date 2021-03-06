---
layout: post
categories: blog2
---
{% highlight java %}
{% endhighlight %}

<h1>About Map-Reduce</h1>

<div>
{% highlight java %}
Map-Reduce - моель(парадигма) распределённых вычислений для обработки больших объёмов данных. 
Обычно вычисления производятся на кластере из машин, когда встречаются большые данные.
Также можно использовать и на одной машине.
Задача: есть текстовый файл в нем нужно подсчитать количество вхождений определённого слоя.
Когда файл маленький все хорошо, но естли файл большой: log файл в котором 10 Gb памяти то тут 
возникают проблемы.
{% endhighlight %}
</div>
<h4>Два шага Map - в котором обрабатываются данные и Reduce в котором данные свёртываются.</h4>

<div>
<img src="/assets/images/MapReduce-02.jpg" class="center">
</div>


{% highlight java %}
1) Файлы делятся на независимые части (split-ы)
2) Далее идёт Map phase (Обработка данных занимаются так называемые процессы worker-ы)
    worker-ы на фазе map - mapper-ы
    worker-ы на фазе reduce - reducer-ы
3) Worker-ы имеют ограниченное число оперативной памяти и ядер
4) Каждый split может обрабатыватся своим worker-ом, причем разные worker-ы
 не могут общатся между собой т.к они работают параллельно
5) После отработки mapper-ов каждый mapper записывает результат себе на диск
6) Далее передача reducer-у происходит по KEY-VALUE 
Причем разные mapper-ы могут перейти в один reducer, если у них одинаковый KEY
7) Результат пишется в output file

{% endhighlight %}

<img src="/assets/images/MapReduce-03.jpg" class="center">

{% highlight java %}
Так как воркеры не могут передавать информацию друг другу то сплиты должны
быть независимыми, обычно делятся по блокам в hdfs. Так как архивы нельзя 
разделить независимо то каждый архив обрабатывается одним воркером.
{% endhighlight %}

<h3>Data locality - в hdfs данные лежат на локальном диске и чтобы обработка была 
максимально быстрой, то делается так чтобы воркер запускался на той машине где лежит сплит
если это возможно.
</h3>

<img src="/assets/images/MapReduce-04.jpg" class="center">
<h3>Промежуточные данные не хранятся в HDFS так как при этом нужно производить
репликацию данных, что очень затратно. Если произошёл сбой и данные потерялись,
то просто перезапускается воркер</h3>
<h3>Shuffle - процесс копирования с мапперов в редюсеры</h3>

<img src="/assets/images/MapReduce-05.jpg" class="center">
<h3> 
Количество выходных данных равно количеству редюсеров. Редюсер может писать только в один файл.
Данные не теряются т.к хранятся уже в HDFS.
</h3>
<h3>
Чем больше редюсеров тем меньше данных будет обрабатывать конкретный воркер, 
но мы можем получить случай когда основное время уходит на запуск редюсера 
по сравнению с подсчётом задачи и редюсеры будут просто ждать своей очереди
что не есть эффективно. На практике принимается баланс обычно<h2>500Mb - 1.5Gb</h2>
</h3>
<img src="/assets/images/MapReduce-06.jpg" class="center">
<h3>Данная парадигма поможет если мапперы работает более суток, тогда 
если какой-то маппер упал его надо перезапускать и задача может считаться неделю
хотя должна считаться сутки, для этого мы считаем отдельно фазу мап и фазу редюс.
А уже в фазе мап мв храним в HDFS резапускать ничего не нужно.</h3>

<h2>Полная схема Map-Reduce программ.</h2>
<img src="/assets/images/MapReduce-07.jpg" class="center">
<h3>User Program - включает в себя входную и выходную директорию, функцию мап и функцию редюс.
Все это передаётся Мастеру.
</h3>
<h3>Мастер считывает данные из входного пути и определяет: Количество сплитов, 
сколько воркеров будет и какие данные отправлять воркерам, воркеры отправляют данные мастеру о своей работе,
причем если маппер упал то мастер перезапускает его, далее копируются данные в локальные диски,
далее шафл и мастер запускает редюсеры и также слидит за ними.
</h3>
---
layout: post
categories: blog2
---

{% highlight java %}
{% endhighlight %}
Hadoop Streaming - позволяет использовать другие языки программирования 
для написание Map-Reduce программ. Подходят только те которые могут 
писать в Std.Output и писать в Std.Input 
<div class="my_text">
Используется для быстрого написания не сложных программ, для всего остального лучше юзать Java.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-34.jpg" class="center">


<img src="/assets/images/MapReduce_API/MapReduce_API-35.jpg" class="center">

<div class="my_text">
Пример Mapper в Python (print выводит в std.out)
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-36.jpg" class="center">

<div class="my_text">
Пример Reducer в Python
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-37.jpg" class="center">

<div class="my_text">
Используется на малых файлах чтобы посмотреть функциональность.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-38.jpg" class="center">

<div class="my_text">
Используется на большых файлах. hadoop-streaming.jar - стандартный жарвик.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-39.jpg" class="center">
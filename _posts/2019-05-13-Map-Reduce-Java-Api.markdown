---
layout: post
categories: blog2
---

{% highlight java %}
{% endhighlight %}

<div class="my_text">
Hadoop и все его подсистемы написанны на java, поэтому все его java api богаты.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-02.jpg" class="center">

<div class="my_text">
Один из основных классов Job - создает объекты задачи.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-03.jpg" class="center">

<div class="my_text">
Наследовать наш маппер мы должны от класса Mapper, и реализовать там функцию void map(K, V, Context). 
Context используется для передачи статистики jobtracker, осуществлять запись выходных данных, также можно из context можно получить информацию о задаче.
<br>
В функции  setup можно что-то проинициализировать до мапперов(например открыть файл).

</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-04.jpg" class="center">

<div class="my_text">
Наследуем наш редюсер от класса Reducer и реализуем там функцию reduce.
<br>
Iterable не хранит как по себе все значения можно только пробегать по ним(например нельзя сразу узнать количество данных)
сделано это, чтобы при больших данных память не переполнялась.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-05.jpg" class="center">

<div class="my_text">
Наследуем наш партишн от Partitioner и наследуемся от Partitioner которая возвращает номер редюсера.
<br>
numPartitions - число редюсеров 
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-06.jpg" class="center">



<div class="my_text">
Псевдокод подсчёта слов в тексе. docid - номер строки, text - текст строки.
В редюсер передаём  в качестве ключа слово и в качестве значения итератор из единичек.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-07.jpg" class="center">

<div class="my_text">
Реализация на java. getConf() - получает объект конфигурации и WordCount - название задачи.
Далее нужно определить где определяется функциональность задачи (setJarBuClass).
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-08.jpg" class="center">

<div class="my_text">
Далее нужно определить входные пути. И определить формат с помщью которого нужно прочитать файлы!
<br>
Вместо TextInputFormat может быть любой класс наследуемый от InputFormat. TextInputFormat - готовый класс 
у которого ключ -номерстроки(LongWritable) а значение сама строка(Text).

</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-09.jpg" class="center">

<div class="my_text">
Темерь мы должны описать выходные данные. Определяем путь выходных данных, формат выходных данных,
ключь и значение выходных данных, а также типы оправки ключей и значений для мап если они отличаются отредюсера.

</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-10.jpg" class="center">


<div class="my_text">
Определение маппера и редюсера для задачи.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-11.jpg" class="center">

<img src="/assets/images/MapReduce_API/MapReduce_API-12.jpg" class="center">

<div class="my_text">
Рассмотрим полнй текс программы (нам нужно реализовывать класс) переобпределить класс run.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-13.jpg" class="center">

<div class="my_text">
main()
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-14.jpg" class="center">

<div class="my_text">
Класс Mapper. IntWritable - целое число, Text - текст, ImmutableBytesWritable - массив байтов.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-15.jpg" class="center">

<div class="my_text">
Реализация Mapper. tokenizer разбивает по словам, записываем пару ключь значение через 
write в context.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-16.jpg" class="center">

<div class="my_text">
Класс Reducer. Выходные значения маппера должны быть входными для релюсера!
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-17.jpg" class="center">

<div class="my_text">
Реализация Mapper. tokenizer разбивает по словам, записываем пару ключь значение через 
write в context.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-18.jpg" class="center">

Про Combiner.
<img src="/assets/images/MapReduce_API/MapReduce_API-19.jpg" class="center">

<div class="my_text">
Используются другие типы данных так как нужно их кодировать и раскодировать для хадупа.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-20.jpg" class="center">

<div class="my_text">
Что делать если нам нужен свой тип данных( например хотим передовать координаты х и y и передовать её в качестве ключа)
<br>
Можно написать так x@y но разделитель не доджне встречатся в других данных, при этом нужно будет парсить данные в дальнейшем.
<br>
ОБЫЧНО ТАКОЙ СПОСОБ НЕ ИСПОЛЬЗУЕТСЯ!
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-21.jpg" class="center">

<div class="my_text">
ПРАВИЛЬНЫЙ СПОСОБО НАСЛЕДОВАНИЕ ОТ Writable(Comprable)
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-22.jpg" class="center">

<div class="my_text">
Split данных!  Существуют  уже готовые реализации например FileSplit & TableSplit.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-23.jpg" class="center">

<div class="my_text">
Class InputFormat. Должны быть реализованы два метода getSplits - возвращает list объектов инпутсплит,
createRecordReader() - как прочитать inputsplit
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-24.jpg" class="center">

<div class="my_text">
NLineFormat - ограничевает количество строк, DBInputFormat - для баз данных, TableInputFormat - Hadoop базы данных.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-25.jpg" class="center">

<div class="my_text">
Class OutputFormat - должен проверять ошибки для задачи(например если файл занят для записи 
то он должен создавать новый и записывать их). 
<br>
RecordWriter - объект для записи(каким образом данные будут записаны)
<br>
OutputCommiter - для востановления связи после сбоя.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-26.jpg" class="center">

<div class="my_text">
NullOutputFormat -  когда данные не выводятся а пишутся в какой то файл например.
</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-27.jpg" class="center">

<div class="my_text">
Процесс передачи данных от маппера в редюсер, этот процесс очень важен так как он может занимать большую часть времени работы программы.

</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-28.jpg" class="center">
<img src="/assets/images/MapReduce_API/MapReduce_API-29.jpg" class="center">
<img src="/assets/images/MapReduce_API/MapReduce_API-30.jpg" class="center">

Запуск программы на API.
<img src="/assets/images/MapReduce_API/MapReduce_API-31.jpg" class="center">
<div class="my_text">
Процесс отладки. Обычно это вывод сообщений(вывод в лог файл) или использование счётчиков т.к программы осуществляется
параллельно.

</div>
<img src="/assets/images/MapReduce_API/MapReduce_API-32.jpg" class="center">

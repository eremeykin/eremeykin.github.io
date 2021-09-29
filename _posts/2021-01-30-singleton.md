---
title: Singleton
tags:
- 'тип: производящий'
- 'уровень: объект'
- GoF
- singleton
- static
- enum
show_excerpt: true
---

Singleton обеспечивает наличие в системе только одного экземпляра заданного
класса, позволяя другим классам получать доступ к этому экземпляру.

<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }

    .spoiler >  input + .box {
    	display: none;
    }
    .spoiler >  input:checked + .box {
    	display: block;
    }
    .bootstrap.card {
        max-width: 100%;
    }
    h5.mb-0 {
        margin-top: 0;
    }
    .no-shadow.btn:focus {
     box-shadow: none !important;
    }
</style>

## Назначение
Singleton обеспечивает наличие в системе только одного экземпляра заданного
класса, позволяя другим классам получать доступ к этому экземпляру.

- нужно, имея лишь один экземпляр класса, обеспечить доступ к нему всем
элементам приложения и запретить создание новых экземпляров этого класса.

## Описание
Допустим, имеется некоторая центральная функциональность, которая должна быть
разделена на всё приложение. Такой функциональностью может быть, например, сбор
статистики (истории) или управление единственным ресурсом. Эта функциональность
должна быть реализована классом, единственный экземпляр которого используется
всеми клиентами внутри приложения. Для избежания несогласованного состояния
недопустимо позволять клиентам самостоятельно создавать экземпляры этого класса.
С другой стороны, каждому клиенту необходимо предоставить доступ к единственному
объекту.

## Реализация
Во-первых, необходимо запретить создавать объекты класса. Для этого надо определить в
классе только `private` конструктор(ы). Это также ведёт к потере возможности
наследования такого класса. Для получения единственного экземпляра класса
добавляется `public static` метод `getInstance()`, возвращающий единственный объект
singleton класса. Ссылка на этот объект хранится как `private static`
поле класса. Как правило, создание singleton объекта
происходит при инициализации класса, но возможно и "lazy" инстанциирование,
когда создание экземпляра происходит в момент первого вызова `getInstance()`.

*Опасности:*
- все конструкторы обязательно должны быть объявлены `private` и причем должен
быть как минимум один конструктор, иначе Java добавит конструктор
по-умолчанию;
- класс не должен допускать альтернативных возможностей создания своих
экземпляров: реализовывать интерфейсы `Cloneable` или `Serializable`;
- внимательно надо относиться к реализации Singleton в условиях, когда программа
ссылается на Singleton только через динамически загружаемые классы: апплеты,
сервлеты, мидлеты. Если программа не использует классы, то они могут быть
удалены сборщиком мусора. Когда Singleton будет загружен снова, то информация
в старом экземпляре будет потеряна;
- потокобезопасность.

<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>

<div id="accordion">
  <div class="bootstrap card">
    <div class="card-header" id="headingOne">
      <h5 class="mb-0">
        <button class="btn collapsed no-shadow" data-toggle="collapse" data-target="#collapseOne" aria-expanded="false" aria-controls="collapseOne">
          Потокобезопасность
        </button>
      </h5>
    </div>
    <div id="collapseOne" class="collapse" aria-labelledby="headingOne" data-parent="#accordion">
      <div class="card-body">
        <ul>
            <li>
                потокобезопасность является проблемой только для ленивой инициализации; для инициализации при загрузке
                класса и варианта реализации через <code>enum</code> потокобезопасность достигается из коробки;
            </li>
            <li>
                для ленивой инициализации при одновременном доступе нескольких потоков
                возможно создание нескольких экземпляров Singleton. Решения:
                <ul>
                    <li>метод <code>getInstance()</code> объявить как <code>synchronized</code>;</li>
                    <li>применить идиому <a href="https://habr.com/ru/post/129494/">Double Checked Locking & volatile</a>;</li>
                    <li>применить идиому <a href="https://habr.com/ru/post/129494/">On Demand Holder idiom</a>;</li>
                </ul>
            </li>
            <li>
                на потокобезопасность надо обращать внимание не только в контексте инициализации Singleton,
                но и в контексте вызова других методов, так как если существует единственный объект, то все потоки используют только этот объект.
            </li>
        </ul>
      </div>
    </div>
  </div>
</div>



<p align="center">
  <img src="/assets/images/singleton/singleton-class-diagram.png" />
</p>

<div class="grid grid--px-0">
  <div class="cell cell--lg-2 cell--3"><b>Singleton</b></div>
  <div class="cell cell--auto"><i>Runtime</i></div>
  <div class="cell cell--lg-12 wrap">Класс, объект которого существует в единственном экземпляре.</div>

  <div class="cell cell--lg-2 cell--3"><b><u>instance</u></b></div>
  <div class="cell cell--auto"><i>private static final Runtime currentRuntime</i></div>
  <div class="cell cell--lg-12 wrap"><code>private static final</code> поле типа Singleton, ссылка на тот самый единственный экземпляр, который создается при инициализации класса (как правило)</div>

  <div class="cell cell--lg-2 cell--3"><b>Singleton()</b></div>
  <div class="cell cell--auto"><i>private Runtime() {}</i></div>
  <div class="cell cell--lg-12 wrap"><code>private</code>конструктор (часто пустой) для защиты от создания новых объектов</div>

  <div class="cell cell--lg-2 cell--3"><b>getInstance()</b></div>
  <div class="cell cell--auto"><i>public static Runtime getRuntime()</i></div>
  <div class="cell cell--lg-12 wrap"><code>public static</code>метод для доступа к единственному экземпляру класса, т.е. возвращает ссылку на <code>instance</code></div>

</div>

## Примеры
* Класс `java.lang.Runtime` имеет единственный `private` конструктор с
комментарием `/** Don't let anyone else instantiate this class */` и статический
метод `getRuntime()`, который возвращает `private static final` экземпляр
`currentRuntime`.
* Может использоваться например для реализации Null Object. Так как Null Object
обычно всё равно не содержит никакой информации о состоянии, то для него
подойдет реализация в виде Singleton.

## Варианты
1. "Канонический", не ленивый: создание экземпляра при инициализации класса:
```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton(){}

    public static Singleton getInstance() {
        return instance;
    }
}
```
1. Ленивый (**потоконебезопасный**) Singleton: `instance` создается не при инициализации класса, а
лениво, во время вызова `getInstance()`:
```java
// ...
public static Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```
1. Ленивый (потокобезопасный) Singleton через `synchronized`. Добавлен
модификатор `synchronized` к методу `getInstance()`.
1. Ленивый (потокобезопасный) Singleton через идиому Double Checked Locking & volatile
```java
public class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        Singleton localInstance = instance;
        if (localInstance == null) {
            synchronized (Singleton.class) {
                localInstance = instance;
                if (localInstance == null) {
                    instance = localInstance = new Singleton();
                }
            }
        }
        return localInstance;
    }
}
```
1. Ленивый (потокобезопасный) Singleton через идиому On Demand Holder idiom
```java
public class Singleton {
    private static class SingletonHolder {
        public static final Singleton HOLDER_INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.HOLDER_INSTANCE;
    }
}
```
Идиома основана на том, что класс инициализируется только при первом **активном**
использовании (см. [справку](#справка)). То есть, инициализация класса `Singleton` выполняется тривиально
(в нем нет `static` переменных или `static` блоков инициализации) и не вызывает инициализацию `SingletonHolder`.
Инициализация `SingletonHolder` выполняется в момент использования статического поля `HOLDER_INSTANCE`
внутри метода `getInstance()`. JLS гарантирует последовательную, не многопоточную инициализацию класса.
1. Реализация через `enum`, не ленивый (предпочтительна по Effective Java by Joshua Bloch):
```java
public enum Singleton {
    INSTANCE;
    public void execute(String arg){
        // perform operation here
    }
}
```
1. Допустимо несколько экземпляров Singleton. Метод `getInstance()` управляет
распределением экземпляров и иногда может создавать новые. Обобщается до
шаблона ObjectPool.
1. Метод `getInstance()` может возвращать подклассы Singleton.

## Справка
Следует различать инициализацию объектов класса, инициализацию классов и
загрузку классов.

**Загрузка класса** - чтение его бинарного представления в JVM. Момент загрузки
класса явно не специфицирован, известно лишь что она происходит до
инициализации класса. Загрузка может быть ленивой или нет в зависимости от JVM.

**Инициализация класса** - установка начальных значений `static` переменных и выполнение
кода в `static` блоках. Момент инициализации класса - первое активное
использование класса:
 - создается экземпляр класса (в том числе через reflection, клонирование, десериализацию)
 - вызывается статический метод класса
 - присваивается значение статической переменной, объявленной в классе
 - используется статическое поле, если это не константа
 - выполняется выражение `assert`, вложенное лексически в класс верхнего уровня

<!--Когда инициализируется класс, с ним также инициализируется и его суперклассы
(если они ещё не были инициализированы) и его суперинтерфейсы, если они
объявляют какие либо `default` методы. Инициализация интерфейса сама по себе
не вызывает инициализации суперинтерфейсов. Ссылка на `static` поле вызывает
инициализацию только того класса/интерфейса, который на самом деле объявляет её,
даже если ссылка указана через имя подкласса/подинтерфейса или класса, который
реализует интерфейс.-->





## Ссылки
[https://java-design-patterns.com/patterns/singleton/](https://java-design-patterns.com/patterns/singleton/)

[https://github.com/iluwatar/java-design-patterns/tree/master/singleton](https://github.com/iluwatar/java-design-patterns/tree/master/singleton)

[StackOverflow: Examples of singleton classes in the Java APIs](https://stackoverflow.com/questions/3061328/examples-of-singleton-classes-in-the-java-apis)

[Хабр: Правильный Singleton в Java](https://habr.com/ru/post/129494/)

[Wikipedia: Initialization-on-demand holder idiom](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom)

[https://refactoring.guru/design-patterns/singleton](https://refactoring.guru/design-patterns/singleton)

[Baeldung - Singletons in Java](https://www.baeldung.com/java-singleton)

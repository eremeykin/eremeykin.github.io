---
title: Decorator
tags:
- 'тип: структурный'
- 'уровень: компонент'
- GoF
- decorator
- wrapper
- delegation
- recursion
- interface
- 'abstract class'
- 'dynamic inheritance'
- 'inheritance vs delegation'
- 'class explosion'
- 'avoid conditionals'
- 'wrapper family pattern'
- 'wrapper family pattern *'
- 'recursion like'
show_excerpt: true
---

Decorator незаметно для клиента расширяет (модифицирует) функциональность
объекта путем реализации интерфейса оригинального объекта и делегирования ему
вызовов.

<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }
</style>

## Назначение

Decorator незаметно для клиента расширяет (модифицирует) функциональность
объекта путем реализации интерфейса оригинального объекта и делегируя ему
вызовы. Благодаря Decorator:
* класс оригинального объекта остается неизмененным
* достигается гибкость в изменении поведения во время выполнения
* можно набирать требуемые свойства в определенной последовательности заворачивая исходный объект в декораторы

## Описание
Имеется некоторый оригинальный объект с полезным поведением, которое определено
интерфейсом. Допустим, возникла необходимость немного изменить поведение этого
объекта без изменения кода оригинального класса. Требуемое изменение можно
выразить как некоторая операция, совершаемая перед вызовом метода(ов) объекта или
после его вызова (возможно, модифицирующая результат вызова). Decorator
одновременно и является объектом целевого интерфейса и имеет этот объект.

В этом случае применяется шаблон Decorator. Оригинальный объект замещается
обращающимся к нему другим объектом, реализующим тот же интерфейс. Замещающий
объект заключает в себе необходимые изменения, применяемые к операциям
оригинального объекта.

Decorator можно применять несколько раз последовательно по принципу матрешки:
первый раз к оригинальному объекту, затем к первому декоратору и т.д., набирая
таким образом нужное поведение.

Decorator -- гибкая альтернатива наследованию. Поведение можно менять во время
выполнения, добавляя или снимая обертки из декораторов.

Часто Decorator принимает оригинальный объект в виде аргумента конструктора, а
не создает его самостоятельно. Т.е. Decorator не управляет его жизненным циклом.

Decorator, как и chain of responsibility, чем-то напоминает рекурсию. Следует
помнить, что в рекурсивный метод мы попадаем "два раза". Первый раз
на нисходящем пути, второй -- на восходящем.

<p align="center">
  <img src="/assets/images/decorator/decorator-recursion.png" />
</p>

## Реализация


<p align="center">
  <img src="/assets/images/decorator/decorator-uml-class-diagram.png" />
</p>


<div class="grid grid--px-0">
  <div class="cell cell--lg-3 cell--3"><b>Client</b></div>
  <div class="cell cell--auto">Client</div>
  <div class="cell cell--lg-12 wrap">Клиентский код, использует сервис через его интерфейс, таким образом подмена сервиса на декоратор может быть проведена незаметно</div>

  <div class="cell cell--lg-3 cell--3"><b>Service</b></div>
  <div class="cell cell--auto"><i>InputStream</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейс, предоставляемый декорируемым компонентом (на самом деле может быть абстрактным / конкретным классом)</div>

  <div class="cell cell--lg-3 cell--3"><b>ConcreteService</b></div>
  <div class="cell cell--auto">RandomInputStream</div>
  <div class="cell cell--lg-12 wrap">Некоторая полезная реализация сервиса, которую требуется несколько изменить/расширить</div>

  <div class="cell cell--lg-3 cell--3"><b><i>AbstractServiceDecorator</i></b></div>
  <div class="cell cell--auto">FilterInputStream</div>
  <div class="cell cell--lg-12 wrap">Базовая реализация декоратора, как правило делегирует все вызовы без изменений и предполагает что дочерний класс переопределит это поведение при необходимости</div>

  <div class="cell cell--lg-3 cell--3"><b>ServiceDecoratorA</b></div>
  <div class="cell cell--auto"><i>DigestInputStream</i></div>
  <div class="cell cell--lg-12 wrap">Декоратор, который привносит полезную модификацию в существующую реализацию</div>

</div>

## Примеры
Допустим, для теста требуется поток случайных байтов в виде реализации `InputStream`.
Но после того как этот поток будет прочтен, требуется получить md5 сумму этого потока.
При этом требуется не расходовать память на хранение всего массива прочтенных байтов.
Для решения такой задачи удобно применить шаблон Decorator. Исходный случайный поток
декорируется с помощью `DigestInputStream` который при вызове метода `read()`
подсчитывает md5, а текущий байт отдает без какого-либо изменения.

<p align="center">
  <img src="/assets/images/decorator/decorator-uml-class-diagram-example.png" />
</p>

## Варианты

* Decorator всегда сохраняет объект того класса, который он декорирует. Он может
это делать в конструкторе, приняв объект как аргумент, либо через setter.

* Decorator может расширять декорируемый интерфейс, привнося в него какие-либо
полезные методы.

* Может декорироваться не только интерфейс, но и абстрактный класс (`InputStream`, например)
или даже конкретный класс.

* Decorator может не быть унаследован от класса, который напрямую перенаправляет все вызовы,
а реализовывать декорируемый интерфейс напрямую.

## Чем отличается

**[Adapter](/2021/01/24/adapter.html)** -- Decorator не изменяет интерфейс объекта,
а назначение Adapter состоит в том, чтобы привести имеющийся интерфейс к желаемому.
Adapter не предназначен для каких-либо умных преобразований функционала
(в отличие от Decorator), он просто сводит один интерфейс к другому.

**[Proxy](/2021/04/26/proxy.html)** -- И Proxy и Decorator имеют с замещаемым
объектом один и тот же тип, но Proxy знает точный тип объекта, в то время как
Decorator оперирует интерфейсом более высокого уровня. По GOF отличие Decorator
от Proxy в том, что Decorator добавляет какой-либо новый функционал или
ответственность объекту, а Proxy только управляет доступом к нему, накладывает
ограничения на клиента. При использовании Decorator клиент, как правило, знает,
что ему требуется расширенная или модифицированная функциональность, а использование
Proxy должно быть совершенно прозрачно для клиента.

**[Bridge](/2021/03/21/bridge.html)**, как и Decorator может решить
проблему "class explosion". Но есть принципиальный момент: Decorator работает
на пространстве одной иерархии классов, а Bridge на пространстве двух иерархий.
Т.е. Decorator позволяет комбинировать между собой классы из множества, допустим
`{1, 2, 3}` получая `(1, 2), (1, 2, 3), (1), (2, 3)...` и т.д. а Bridge позволяет
сочетать классы их одной иерархии с классами из другой, например, `{A1, A1}` с
`{B1, B2}` получая `(A1, B1), (A2, B1), ...`. Несмотря на то, что Decorator
может декорировать различие реализации некоторого интерфейса, интерфейс декорируемого
всё объекта равно одни. Bridge разделяет две иерархии с различными интерфейсами.

**[Template method](/2021/01/26/template-method.html)**, подобно
Decorator, позволяет менять реализацию, но не перед/после вызова метода, а внутри
его вызова. Использует наследование, а не композицию.

**[Strategy](/2021/09/28/strategy.html)** Через композицию позволяет менять внутренности метода.

**[Chain of responsibility](/2021/05/24/chain-of-responsibility.html)** имеет структуру, очень похожую на структуру
Decorator. И Decorator и Chain of responsibility по сути набор последовательно
связанных объектов одного типа. Шаблоны различаются назначением: Decorator для добавления
некоторого поведения, Chain of responsibility для обработки. Существует [мнение](https://stackoverflow.com/a/3721318/5457525), что
основное их отличие в том, что Chain of responsibility может прервать цепочку в любом месте
и вернуть результат, в то время как для Decorator это не типично.

## Ссылки
[https://java-design-patterns.com/patterns/decorator/](https://java-design-patterns.com/patterns/decorator/)

[https://github.com/iluwatar/java-design-patterns/tree/master/decorator](https://github.com/iluwatar/java-design-patterns/tree/master/decorator)

[https://refactoring.guru/design-patterns/decorator](https://refactoring.guru/design-patterns/decorator)

[Decorator Pattern – Design Patterns (ep 3)](https://www.youtube.com/watch?v=GCraGHx6gso)

[Differences between Proxy and Decorator Pattern](https://stackoverflow.com/questions/18618779/differences-between-proxy-and-decorator-pattern/60478875)

[Structural Patterns (comparison) – Design Patterns (ep 12)](https://www.youtube.com/watch?v=lPsSL6_7NBg&list=PLrhzvIcii6GNjpARdnO4ueTUAVR9eMBpc&index=12)

[Baeldung - The Decorator Pattern in Java](https://www.baeldung.com/java-decorator-pattern)

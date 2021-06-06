---
title: Chain of responsibility
tags:
- 'тип: поведенческий'
- 'уровень: компонент'
- GoF
- chain of responsibility
- event
- listener
- 'recursion like'

show_excerpt: true
---

Chain of responsibility позволяет выстроить гибкую цепочку из обработчиков команды,
каждый из которых принимает независимое решение об обработке и/или передаче
следующему обработчику по цепочке.

<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }
</style>

## Назначение
Chain of responsibility позволяет выстроить гибкую цепочку из обработчиков команды,
каждый из которых принимает независимое решение об обработке и/или передаче
следующему обработчику по цепочке.

* цепочка обработчиков может меняться динамически по составу или структуре/последовательности
* цепочка обработчиков определяет порядок обработки
* следует либо не надеяться что все команды будут обработаны, либо определить
  терминальный обработчик, который в обязательном порядке обрабатывает все команды каким-либо дефолтным способом.


## Описание
В шаблоне принимают участие объект-отправитель команды и обработчики этой команды.
Все обработчики определены одним интерфейсом. Обработчики команды организованы в
виде цепочки: то есть текущий обработчик имеет ссылку только на следующий обработчик,
но не знает полного своего окружения. Отправитель команды вызывает метод первого
обработчика, который может обработать команду и вернуть ответ и/или делегировать
обработку следующему обработчику по цепочке. Важно, что обработчики ориентированы
на использование только общего интерфейса, это позволяет динамически менять состав
цепочки обработчиков. Для команды, передаваемой по цепочке обычно рекомендуется
заводить один объект, объединяющий в себе все параметры.

**Проблемы:**

* каким образом и кем будет создана последовательность обработчиков?
* как клиент получит ссылку на первый обработчик?


## Реализация


<p align="center">
  Схема<br/>
  <img src="/assets/images/chain-of-responsibility/chain-of-responsibility-scheme.png" />
</p>

<p align="center">
  Диаграмма классов<br/>
  <img src="/assets/images/chain-of-responsibility/chain-of-responsibility-class-diagram.png" />
</p>


<div class="grid grid--px-0">
  <div class="cell cell--lg-3 cell--3"><b>Client</b></div>
  <div class="cell cell--auto">Client</div>
  <div class="cell cell--lg-12 wrap">Первичный источник команды, отправляющий её первому обработчику в цепочке, к которому у источника имеется ссылка через общий интерфейс всех обработчиков</div>

  <div class="cell cell--lg-3 cell--3"><b>Handler</b></div>
  <div class="cell cell--auto">Filter</div>
  <div class="cell cell--lg-12 wrap">Общий интерфейс всех обработчиков команды в цепочке</div>

  <div class="cell cell--lg-3 cell--3"><b><i>AbstractHandler</i></b></div>
  <div class="cell cell--auto"></div>
  <div class="cell cell--lg-12 wrap">Абстрактный класс обработчиков, содержащий общее поведение для всех обработчиков, например,
    обработку команды в случае возможности её обработки и передачи дальше по цепочке в противном случае.
    Также часто содержит код, который обслуживает ссылку на следующий обработчик</div>

  <div class="cell cell--lg-3 cell--3"><b>ConcreteHandler</b></div>
  <div class="cell cell--auto">WelcomeFilter</div>
  <div class="cell cell--lg-12 wrap">Полезная реализация некоторого обработчика команды</div>
</div>

## Примеры
`javax.servlet.Filter` - пример обработчика, которые связаны по принципу Chain Of Responsibility.
Фильтр имеет метод `doFilter(request, response, filterChain)` в который кроме запроса и ответа
передается объект FilterChain. Этот объект отражает последовательность фильтров. Контейнер сервлетов,
в зависимости от текущей конфигурации (`web.xml`), на каждый запрос создает соответствующий FilterChain.
Таким образом, Filter явно не хранит ссылку на следующий фильтр, ему передается оставшаяся часть цепочки
в качестве параметра. Последовательность филтров хранится в FilterChain.

<p align="center">
  <img src="/assets/images/chain-of-responsibility/chain-of-responsibility-example.png" />
</p>

<p align="center">
  <img src="/assets/images/chain-of-responsibility/chain-fo-responsibility-servlet-filter.png" /><br/>
  Source:  <a href="https://stackoverflow.com/questions/25196867/how-filter-chain-invocation-works">https://stackoverflow.com/questions/25196867/how-filter-chain-invocation-works</a>
</p>

## Варианты
* есть подход, при котором обработчику в любом случае передается команда для
  обработки, а в процессе обработки обработчик решает, передать ли команду дальше.
* есть подход, при котором обработчик сначала решает, сможет ли он обработать
  команду. Если сможет, то он обрабатывает её, дальше запрос не передается. Если
  не сможет, запрос передается по цепочке. Подход характерен тем, что у команды
  не больше одного обработчика. В таком подходе есть отдельный `boolean`
  метод для проверки и метод для обработки.
* обработчики необязательно должны быть выстроены в виде цепочки. Обработчики
  могу составлять например дерево, но маршрут команды в итоге образует цепочку.

<p align="center">
  <img src="/assets/images/chain-of-responsibility/chain-of-responsibility-as-tree.png" />
</p>

## Чем отличается

**[Decorator](/2021/05/05/decorator.html)** имеет структуру, очень похожую на структуру
Chain of responsibility. И Decorator и Chain of responsibility по сути набор последовательно
связанных объектов одного типа. Шаблоны различаются назначением: Decorator для добавления
некоторого поведения, Chain of responsibility для обработки. Существует [мнение](https://stackoverflow.com/a/3721318/5457525), что
основное их отличие в том, что Chain of responsibility может прервать цепочку в любом месте
и вернуть результат, в то время как для Decorator это не типично.


## Ссылки
[https://java-design-patterns.com/patterns/chain-of-responsibility/](https://java-design-patterns.com/patterns/chain-of-responsibility/)

[https://github.com/iluwatar/java-design-patterns/tree/master/chain-of-responsibility](https://github.com/iluwatar/java-design-patterns/tree/master/chain-of-responsibility)

[https://refactoring.guru/design-patterns/chain-of-responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)

[StackOverflow: How filter chain invocation works?](https://stackoverflow.com/a/25197006/5457525)

[StackOverflow: Design Patterns Chain of Resposibility Vs Decorator](https://stackoverflow.com/questions/3721256/design-patterns-chain-of-resposibility-vs-decorator)


---
title: Abstract Factory
tags:
- 'тип: производящий'
- 'уровень: компонент'
- GoF
- abstract
- factory
- interface
- toolkit
show_excerpt: true
---

Abstract Factory обеспечивает создание семейств взаимосвязанных или зависящих
друг от друга объектов без указания конкретных классов. Этим объектам
соответствует набор связанных интерфейсов, которые реализуют создаваемые
объекты.
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
Abstract Factory обеспечивает создание семейств взаимосвязанных или зависящих
друг от друга объектов без указания конкретных классов. Этим объектам
соответствует набор связанных интерфейсов, которые реализуют создаваемые
объекты.

- клиент не должен зависеть от способа получения продукта;
- порождаемые продукты создаются в виде определенного набора, то есть реализуют
интерфейсы, некоторым образом связанные между собой.

## Описание
Допустим, необходимо создавать продукты двух (или более) типов, которые
определяются интерфейсами `ProductA`, `ProductB`. Эти интерфейсы каким-либо
образом связаны друг с другом (например, это элементы одной оконной системы:
текстовое поле и кнопка для Windows, Linux, MacOS; или это юниты одной армии:
лучник и рыцарь японцев, немцев или римлян). Для создания этих продуктов
определен интерфейс `Factory` в котором имеется отдельный метод для создания
продукта каждого из типов: `createA()` и `createB()`. Для создания конкретных
продуктов есть конкретные реализации `1` и `2` интерфейса `Factory`:
`ConcreteFactory1`, `ConcreteFactory2`, которые производят соответствующие
конкретные продукты указанных интерфейсов:

| Производитель           |Метод              |Продукт              |
| ------------------------|:-------------------:|:-------------------:|
| `ConcreteFactory1`      | `createA()`<br>`createB()`| `ConcreteProductA1`<br> `ConcreteProductB1`|
| `ConcreteFactory2`      | `createA()`<br>`createB()`| `ConcreteProductA2`<br> `ConcreteProductB2`|


## Реализация

<p align="center">
  <img src="/assets/images/2021/01/02/abstract-factory/abstract-factory.png" />
</p>

<div class="grid grid--px-0">
  <div class="cell cell--lg-3 cell--3"><b><i>Factory</i></b></div>
  <div class="cell cell--auto"><i>Connection</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейс фабрики, определяет набор взаимосвязанных абстрактных продуктов</div>

  <div class="cell cell--lg-3 cell--3"><b><i>ProductA, ProductB</i></b></div>
  <div class="cell cell--auto"><i>Statement, PreparedStatement, CallableStatement</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейсы, которые описывают общие функции каждого из продуктов, они взаимосвязаны между собой и пораждаются одной фабрикой</div>

  <div class="cell cell--lg-3 cell--3"><b><i>createA(), createB()</i></b></div>
  <div class="cell cell--auto"><i>createStatement(), prepareStatement(), prepareCall()</i></div>
  <div class="cell cell--lg-12 wrap">Методы абстрактной фабрики для получения соответствующего вида продукта</div>

  <div class="cell cell--lg-3 cell--3"><b><i>ConcreteFactory1, ConcreteFactory2</i></b></div>
  <div class="cell cell--auto"><i>PgConnection, OracleConnection</i></div>
  <div class="cell cell--lg-12 wrap">Классы, реализующие функционал абстрактной фабрики для набора конкретных продуктов одного вида</div>

  <div class="cell cell--lg-3 cell--3"><b><i>ConcreteProductA1, ConcreteProductB1 и ConcreteProductA2, ConcreteProductB2</i></b></div>
  <div class="cell cell--auto"><i>PgStatement, PgPreparedStatement, PgCallableStatement</i></div>
  <div class="cell cell--lg-12 wrap">Связанные между собой конкретные продукты определенного вида</div>


</div>


## Примеры
- `java.sql.Connection` - пример абстрактной фабрики, которую можно получить `Connection connection = DriverManager.getConnection("jdbc:postgresql://localhost:5432/database");`

<p align="center">
  <img src="/assets/images/2021/01/02/abstract-factory/abstract-factory-example.png" />
</p>

## Ссылки
[https://java-design-patterns.com/patterns/abstract-factory/](https://java-design-patterns.com/patterns/abstract-factory/)

[https://github.com/iluwatar/java-design-patterns/tree/master/abstract-factory](https://github.com/iluwatar/java-design-patterns/tree/master/abstract-factory)

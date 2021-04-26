---
title: Factory Method (Mark Grand)
tags:
- 'тип: производящий'
- 'уровень: компонент'
- factory method
- interface
- factory
- 'Mark Grand'
show_excerpt: true
---

Factory Method в интерпретации Mark Grand позволяет создавать различные конкретные
типы продукта, определенного интерфейсом, при этом оставить независимыми клиента
продукта и его производителя.


<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }
</style>

## Назначение
Factory Method в интерпретации Mark Grand позволяет создавать различные конкретные
типы продукта, определенного интерфейсом, при этом оставить независимыми клиента
продукта и его производителя.

- требуется позволить создавать объекты без привязки к классу, который их создает
- требуется создавать различные продукты в зависимости от аргумента,
переданного в создающий метод (data-driven class determination)
- требуется создавать различные продукты в зависимости от конфигурации, определяемой при запуске (determination by configuration)

## Описание
Основа шаблона в том, что создание объекта происходит не прямым вызовом
конструктора через `new`, а путем вызова фабричного метода, определенного в интерфейсе.
Класс, инициирующий создание объекта, создает его не сам, а делегирует создание
объекту-фабрике, при этом обращаясь к ней через заданный интерфейс. Использование
интерфейса позволяет развязать создателя продукта и его клиента. Поэтому,
фабрика и её клиент могут быть реализованы в разных модулях приложения или
загружаться динамически.
Варианты определения фабричного метода:

- Data-driven class determination - фабричный метод, определенный в интерфейсе
фабрики имеет параметр `descriminator` по которому фабрика определяет конкретный
типа продукта.

```java
Image createImage(String ext) {
    if ("gif".equals(ext))
        return new GIFImage();
    if ("jpeg".equals(ext))
        return new JPEGImage();
    // ...
}
```

<p align="center">
  <img src="/assets/images/factory-method/factory-method-discriminator.png" />
</p>

- Determination by configuration - фабричный метод не имеет параметра,
конкретный тип продукта определяется на этапе конфигурирования приложения,
исходя из текущей конфигурации на старте создается та или иная реализация
фабрики, производящий метод которой не имеет параметров. Такая фабрика
производит один тип продуктов, тип продуктов выбирается один раз из конфигурации.


<p align="center">
  <img src="/assets/images/factory-method/factory-method-by-configuration.png" />
</p>

## Реализация

<p align="center">
  <img src="/assets/images/factory-method/factory-method-uml.png" />
</p>

<div class="grid grid--px-0">
  <div class="cell cell--lg-3 cell--3"><b><i>CreationRequester</i></b></div>
  <div class="cell cell--auto"><i></i></div>
  <div class="cell cell--lg-12 wrap">Клиент, которые запрашивает создание продукта для его использования через интерфейс.</div>

  <div class="cell cell--lg-3 cell--3"><b><i>Factory</i></b></div>
  <div class="cell cell--auto"><i>JournalRecordFactory</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейс фабрики, определяющий фабричный метод, который и производит конкретный продукт, являющийся реализацией интерфейса продукта.</div>

  <div class="cell cell--lg-3 cell--3"><b><i>Product</i></b></div>
  <div class="cell cell--auto"><i>JournalRecord, (JournalRecordFactory)</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейс продукта, единственный способ использовать функционал произведенного фабрикой продукта.</div>

  <div class="cell cell--lg-3 cell--3"><b><i>ConcreteProduct</i></b></div>
  <div class="cell cell--auto"><i>StartOfSale, SaleLineItem, ..., (ABCJournalRecordFactory, XYZJournalRecordFactory)</i></div>
  <div class="cell cell--lg-12 wrap">Интерфейс продукта, единственный способ использовать функционал произведенного фабрикой продукта.</div>
</div>

**Проблема:**

Как и для [Abstract Factory](/2021/02/23/abstract-factory.html), актуальна проблема создания конкретной реализации
интерфейса фабрики.

## Примеры
Допустим, надо считывать лог файлы, записанные системой торгового терминала.
Есть два производителя систем торговых терминалов: ABC и XYZ. Каждому
производителю соответствует своя форма различных типов записей: в лог файле
может встретиться, например запись "Начало продаж" (`StartOfSale`) или
"Продажа товара" (`SaleLineItem`).

<p align="center">
  <img src="/assets/images/factory-method/factory-method-example.png" />
</p>
Фабрика `RecordFactoryFactory` создает определенную реализацию
`JournalRecordFactory` в зависимости от текущей конфигурации, то есть выбранного
производителя системы торговых терминалов (`posType`). Созданная реализация
`JournalRecordFactory` инкапсулирует правила чтения и формат записей каждого
типа (`StartOfSale`, `SaleLineItem`, ...) Например, если записи "Начало продаж"
в лог файле торгового терминала от производителя ABC соответствует код 17, в
коде `ABCJournalRecordFactory` можно встретить константы
```java
private static final String START_OF_SALE = "17";
private static final String SALE_LINE_ITEM = "4";
```
К этим константам обращается метод `newRecord()` который сам по коду определяет
ту реализацию `JournalRecord` которую ему надо вернуть для следующей строки.

## Чем отличается
**[Abstract Factory](/2021/02/23/abstract-factory.html)**
Структурно Factory Method (Mark Grand) похож на Abstract Factory.
Первое поверхностное отличие заключается в том, что Abstract Factory предназначен для
создания *совокупности связанных объектов*, а Factory Method, как правило, создает один продукт.
* Data-driven class determination - есть одна реализация интерфейса фабрики,
метод принимает аргумент, по которому определяет конкретный тип продукта.
В этом случае задача фабрики состоит исключительно в инкапсуляции логики
решения какой продукт производить. Через интерфейс развязаны клиент продуктов и
их создатель. В случае Abstract Factory реализаций несколько, методы не имеют параметров,
соответственно тип продукта определяется реализацией фабрики, а не переданным аргументом.
* Determination by configuration - как и в Abstract Factory есть несколько
реализаций фабрики, метод без параметров, тип продукта зависит от класса фабрики.
В такой интерпретации Factory Method является частным случаем Abstract Factory.
Возможно, есть мотивационные различия, но структурных по сути нет.

## Варианты
- Фабричный метод с параметрами или без: Data-driven / Determination by configuration,
см. [описание](/2021/02/28/factory-method-mark-grand.html#описание)
- В интерпретации MarkGrand / [GoF](/2021/03/01/factory-method-gof.html).
- Продукты могут не реализовывать свой интерфейс напрямую, а путем расширения
  абстрактного класса, который реализует этот интерфейс.

## Ссылки
[https://java-design-patterns.com/patterns/factory-method/](https://java-design-patterns.com/patterns/factory-method/)

[https://github.com/iluwatar/java-design-patterns/tree/master/factory-method](https://github.com/iluwatar/java-design-patterns/tree/master/factory-method)

[Factory method GoF](/2021/03/01/factory-method-gof.html)

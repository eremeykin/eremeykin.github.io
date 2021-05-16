---
title: Factory Method (GoF)
tags:
- 'тип: поведенческий'
- 'уровень: объект'
- GoF
- template method
- abstract
- abstract method
- abstract class
- protected
show_excerpt: true
---

Factory Method в интерпретации GoF предоставляет метод, который служит для
получения продукта, определяемого интерфейсом. Этот метод позволяет подклассам
переопределять конкретный тип этого продукта.


<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }
</style>

## Назначение

Factory Method в интерпретации GoF предоставляет метод, который служит для
получения продукта, определяемого интерфейсом. Этот метод позволяет подклассам
переопределять конкретный тип этого продукта.

Требуется создать объект, для которого конкретный класс в момент создания
неизвестен, известен только интерфейс, через который с ним можно
взаимодейтсвовать. Из интерфейса или абстркатного класса нельзя напрямую создать
экземпляр вызовом `new`, поэтому создание объекта выносится в отдельный класс.

## Описание
Подобно шаблону [Template Method](/2021/01/26/template-method.html), абстрактный
суперкласс определяет основную логику работы и в том месте, где требуется
создание продукта, вызывает абстрактный метод, который возвращает некоторую его
реализацию. Какую именно реализацию продукта возвращает фабричный метод,
определяется в конкретном подклассе, реализующем этот метод.

## Реализация
<p align="center">
  <img src="/assets/images/factory-method-gof/factory-method-gof-uml.png" />
</p>

<div class="grid grid--px-0">
  <div class="cell cell--lg-2 cell--3"><b><i>Creator</i></b></div>
  <div class="cell cell--auto"><i>AbstractCollection</i></div>
  <div class="cell cell--lg-12 wrap">Абстрактный класс (или конкретный, см. варианты), содержащий метод, производящий продукт.</div>

  <div class="cell cell--lg-2 cell--3"><b>Product</b></div>
  <div class="cell cell--auto">Iterator</div>
  <div class="cell cell--lg-12 wrap">Интерфейс продукта, которой определяет весь функционал, предоставляемый продуктом его клиенту</div>

  <div class="cell cell--lg-2 cell--3"><b><i>factoryMethod()</i></b></div>
  <div class="cell cell--auto"><i>iterator()</i></div>
  <div class="cell cell--lg-12 wrap">Фабричный метод (обычно абстрактный, но бывает содержащий реализацию по умолчанию), который производит продукт, определяемый через интерфейс</div>


  <div class="cell cell--lg-2 cell--3"><b>ConcreteCreator</b></div>
  <div class="cell cell--auto">ArrayList</div>
  <div class="cell cell--lg-12 wrap">Реализация абстрактного класса, которая определяет конкретный тип возвращаемого фабричным методом продукта</div>

  <div class="cell cell--lg-2 cell--3"><b>ConcreteProduct</b></div>
  <div class="cell cell--auto">ArrayList.Itr</div>
  <div class="cell cell--lg-12 wrap">Реализация интерфейса продукта</div>

</div>

## Примеры

<p align="center">
  <img src="/assets/images/factory-method-gof/factory-method-gof-example.png" />
</p>

## Чем отличается
**[Template Method](/2021/01/26/template-method.html)** является общим случаем Factory Method,
по сути Factory Method это Template Method (GoF), который возвращает объект, реализующий интерфейс
и он применяется с целью создания этого объекта. И Factory Method и Template Method оба имеют очень похожую структуру, но главное
отличие в разной цели. Цель Factory Method - порождающая, а цель Template
Method - поведенческая.

**[Iterator](/2021/04/28/iterator.html)** является примером Factory Method.
Интерфейс `Iterable` является интерфейсом фабрики, а продукт фабрики -- сами
итераторы `Iterator`.

## Варианты

- В интерпретации [MarkGrand](/2021/02/28/factory-method-mark-grand.html) / GoF.
- Абстрактный класс может предоставлять некоторую реализацию продукта по умолчанию, которую, при необходимости, переопределяет наследник.
В этом случае нет прямой необходимости делать класс, содержащий фабричный метод абстрактным (см. GoF).
- Абстрактный класс создателя продукта может быть интерфейсом, а не классом (как в Применение шаблонов Java)
- Фабричный метод может иметь параметры. В этом случае реализация фабричного метода можеть предоставлять различные конкретные продукты
в зависимости от того аргумента, который был ему передан.


## Ссылки
[https://java-design-patterns.com/patterns/factory-method/](https://java-design-patterns.com/patterns/factory-method/)

[https://github.com/iluwatar/java-design-patterns/tree/master/factory-method](https://github.com/iluwatar/java-design-patterns/tree/master/factory-method)

[https://refactoring.guru/design-patterns/factory-method](https://refactoring.guru/design-patterns/factory-method)

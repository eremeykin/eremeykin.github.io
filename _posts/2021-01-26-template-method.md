---
title: Template Method
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

Template Method предоставляет метод, который позволяет подклассам переопределять
части метода, не прибегая к полному его переписыванию.

<!--more-->

<style>
    .wrap {
        padding-bottom: 25px;
    }
</style>

## Назначение
Template Method предоставляет метод, который позволяет подклассам переопределять
части метода, не прибегая к полному его переписыванию.

- требуется получить базовый метод, позволив переопределять его части конкретным
подклассам;
- можно упростить класс для реализации функционала через наследование,
предоставив шаблон класса, в котором оставлены "пробелы", которые необходимо
заполнить;
- необходимо централизовать функциональность метода, которая остается единой для
всех подклассов, но в каждом подклассе она может выполняться по-своему.

## Описание
Пусть некоторый код повторяется многократно в реализациях подклассов с
небольшими локальными изменениями. Проблема состоит в том, что эти изменения
касаются информации, не доступной на уровне суперкласса. Т.к. желательно
максимально переиспользовать имеющийся код, то следует общий код располагать в
суперклассе.

Для того чтобы позволить подклассам переопределять части логики,
применяется шаблон Template Method. В суперклассе определяется метод,
реализующий функциональность, общую для всех подклассов -- шаблонный метод. Этот
шаблонный метод в тех местах, где нельзя определить его поведение на уровне
суперкласса, вызывает абстрактный метод, функциональность которого описана в
конкретных подклассах. Подклассы "заполняют пробелы" в суперклассе через
реализацию абстрактных методов.



## Реализация
Абстрактный класс определяет конкретный шаблонный метод, который
вызывает одну или несколько абстрактных операций (как правило, `protected`).
Конкретный подкласс, реализующий абстрактный класс, определяет поведение только
абстрактных операций через реализацию методов, а поведение шаблонного метода
наследуется. Если шаблонный метод определен как `final`, то
возможности подкласса ограничены описанием работы отдельных частей.

*Примечание:* не следует в шаблонном методе вызывать слишком много абстрактных
методов, в этом случае суперкласс становится не удобно использовать. Шаблонный
метод должен вызывать лишь несколько абстрактных.

<p align="center">
  <img src="/assets/images/2021/01/template-method/template-method-class-diagram.png" />
</p>

<div class="grid grid--px-0">
  <div class="cell cell--lg-2 cell--3"><b><i>AbstractTemplate</i></b></div>
  <div class="cell cell--auto"><i>InputStream</i></div>
  <div class="cell cell--lg-12 wrap">Абстрактный класс (или конкретный, см. варианты), содержащий шаблонный метод.</div>

  <div class="cell cell--lg-2 cell--3"><b>templateMethod()</b></div>
  <div class="cell cell--auto"><i>public int read(byte b[], int off, int len)<br> // Reads up to len bytes of data from input stream into an array of bytes</i></div>
  <div class="cell cell--lg-12 wrap">Конкретный шаблонный метод (часто <code>final</code>), в своем теле вызывающий абстрактные методы.</div>

  <div class="cell cell--lg-2 cell--3"><i><b>operation1()<br>operation2()</b></i></div>
  <div class="cell cell--auto"><i>public abstract int read()<br> // Reads the next byte of data from the input stream</i></div>
  <div class="cell cell--lg-12 wrap">Методы (как правило, абстрактные, но не обязательно), вызываемые шаблонным и описывающие специфическую для подкласса часть логики.</div>

  <div class="cell cell--lg-2 cell--3"><b>ConcreteTemplate</b></div>
  <div class="cell cell--auto"><i>FileInputStream</i></div>
  <div class="cell cell--lg-12 wrap">Класс, унаследованный от шаблонного класса и реализующий необходимые его части.</div>

  <div class="cell cell--lg-2 cell--3"><b>operation1()<br>operation2()</b></div>
  <div class="cell cell--auto"><i>public native int read()</i></div>
  <div class="cell cell--lg-12 wrap">Конкретные методы, описывающие специфическую для подкласса часть логики.</div>

</div>

## Примеры
* Абстрактный класс `java.io.InputStream` имеет шаблонный абстрактный метод
`public int read(byte b[], int off, int len)`. Абстрактный метод реализован в
`FileInputStream`.

## Варианты
1. В суперклассе можно использовать не абстрактные, а конкретные методы с
поведением по-умолчанию. Такие конкретные методы называются hook-методами.
Действует соглашение об именах: их называют ли `do`-something или
something-`Hook`. По умолчанию используется определение этих методов из
суперкласса, но при необходимости его можно заменить в подклассах. В этом
случае суперкласс не обязан быть `abstract`. В документации обязательно (!)
прописывать такие методы, так как иначе их невозможно отделить от обычных.


## Ссылки
[https://java-design-patterns.com/patterns/template-method/](https://java-design-patterns.com/patterns/template-method/)

[https://github.com/iluwatar/java-design-patterns/tree/master/template-method](https://github.com/iluwatar/java-design-patterns/tree/master/template-method)

[StackOverflow: Template design pattern in JDK, could not find a method defining set of methods to be executed in order](https://stackoverflow.com/questions/35559360/template-design-pattern-in-jdk-could-not-find-a-method-defining-set-of-methods)
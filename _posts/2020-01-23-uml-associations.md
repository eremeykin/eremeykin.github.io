---
title: Основы UML
tags: UML диаграмма модификатор стериотип
---

Основные связи на диаграммах классов UML, модификаторы доступа и
стериотипы.

<!--more-->

<style>
.vertical
  {
    -ms-writing-mode: tb-rl;
    -webkit-writing-mode: vertical-rl;
    writing-mode: vertical-rl;
    transform: rotate(180deg);
    white-space: nowrap;
    text-align: center;
    vertical-align: bottom;
  }

.center-flex {
    display: flex;
    justify-content: center;
    align-items: center;
}
  .icon1 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -25px;
    }

  .icon2 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -175px;
    }

  .icon3 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -325px;
    }

  .icon4 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -475px;
    }

  .icon5 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -775px;
    }

  .icon6 {
      width: 200px;
      height: 100px;
      float: left;
      background: url(/assets/images/2020/01/uml-associations/uml-arrows.png) no-repeat;
      background-position: -80px -625px;
    }
</style>
## Виды связей
<div class="grid grid--py-3">
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Класс, который <br> вызывает метод</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon1"></div> </div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Класс, метод <br> которого вызывают</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Направленная ассоциация.</b> Ассоциация - самый слабый вид связи. Обычно ассоциация возникает, когда
  один класс вызывает метод другого или если при вызове метода в качестве аргумента передается объет другого класса.</div>

  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Подкласс</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon2"></div></div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Супер класс</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Обобщение</b> является отношением между более общим элементом (родителем) и более частным
  или специальным элементом (дочерним). Применительно к диаграмме классов данное отношение описывает иерархическое строение класов и
  наследование их свойств и поведения.</div>

  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Класс</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon3"></div></div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Интерфейс</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Реалзиация</b> возникает между классами в том случае, когда интерфейс задает требования
  к поведению системы, а класс реализует это поведение.</div>

  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Класс-клиент</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon4"></div></div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Независимый класс</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Зависимость</b> обозначает такое отношение между классами, что изменение спецификации
  класса-поставщика может повлиять на работу зависимого класса, но не наоборот.</div>

  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Часть</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon5"></div></div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Целое</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Агрегация.</b> Частный случай ассоциации, описывает отношение "является частью". Не всегда
  обозначает физическое вхождение, например, профессиональное сообщестов имеет членов.</div>

  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Часть</div>
  <div class="cell cell--lg-3 cell--md-8 center-flex"><div class="icon6"></div></div>
  <div class="cell cell--lg-1 cell--md-2 vertical center-flex">Целое</div>
  <div class="cell cell--lg-7 cell--md-12"><b>Композиция.</b> Частный случай агрегации. Делает акцент на процесс конструирования и уничтожения
  частей агрегата. Создание/уничтожение частей происходи в результате создания/уничтожения агрегата.</div>
</div>

## Модификаторы доступа
<div class="grid-container">
    <div class="grid">
        <div class="cell cell--3">UML обозначение</div>
        <div class="cell cell--9">Модификатор доступа Java</div>
        <div class="cell cell--3"><code class="language-plaintext highlighter-rouge">+</code></div>
        <div class="cell cell--9"><code class="language-java highlighter-rouge">public</code></div>
        <div class="cell cell--3"><code class="language-plaintext highlighter-rouge">#</code></div>
        <div class="cell cell--9"><code class="language-java highlighter-rouge">protected</code></div>
        <div class="cell cell--3"><code class="language-plaintext highlighter-rouge">~</code></div>
        <div class="cell cell--9"><code class="language-java highlighter-rouge">package-private</code></div>
        <div class="cell cell--3"><code class="language-plaintext highlighter-rouge">-</code></div>
        <div class="cell cell--9"><code class="language-java highlighter-rouge">private</code></div>
    </div>
</div>

## Стериотипы
`<<constructor>>` стериотип перед конструктором

`<<misc>>` стериотип перед остальными методами

`<<interface>>` стериотип для интерфейса (перед названием в шапке)

`abstract` обозначается курсивом в названии, а не стериотипом (классы, метды)

`static` обозначается подчеркиванием названия (методы, поля)

---



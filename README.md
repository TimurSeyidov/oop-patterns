# Паттерны проектирования
Автор: Тимур Сейидов (вводная с [этого сайта](https://refactoring.guru))

Что такое паттерн?
-------------
Паттерн проектирования — это часто встречаемое решение
определённой проблемы при проектировании архитектуры
программ.

В отличие от готовых функций или библиотек, паттерн
нельзя просто взять и скопировать в программу. Паттерн
представляет собой не какой-то конкретный код, а общую
концепцию или пример решения той или иной проблемы,
которое нужно будет подстроить под нужды вашей
программы.

Паттерны часто путают с алгоритмами, ведь оба понятия
описывают типовые решения каких-то известных проблем.
И если алгоритм — это чёткий набор действий, то паттерн
— это высокоуровневое описание решения, реализация
которого может отличаться в двух разных программах.

Если привести аналогии, то алгоритм — это кулинарный
рецепт с чёткими шагами, а паттерн — инженерный чертёж,
на котором нарисовано решение, но не конкретные шаги
его получения.

Из чего состоит паттерн?
-------------

Описания паттернов обычно очень формальны и чаще
всего состоят из таких пунктов:

* проблемы, которую решает паттерн;
* мотивации к решению проблемы способом, который
предлагает паттерн;
* структуры классов, составляющих решение;
* примера на одном из языков программирования;
* особенностей реализации в различных контекстах;
* связей с другими паттернами.

Такой формализм в описании позволяет собрать обширный
каталог паттернов, проверяя все новые паттерны на
состоятельность.

Классификация паттернов
-------------

Паттерны отличаются по уровню сложности, детализации и
охвата проектируемой системы. Проводя аналогию со
строительством, вы можете повысить безопасность
перекрёстка, поставив светофор, а можете заменить
перекрёсток целой автомобильной развязкой с
подземными переходами.

Самые низкоуровневые и простые паттерны — *идиомы*. Они
не очень универсальные, так как применимы только в
рамках одного языка программирования.

Самые универсальные — *архитектурные паттерны*,
которые можно реализовать практически на любом языке.

Они нужны для проектирования всей программы, а не
отдельных её элементов.
Кроме этого, паттерны отличаются и предназначением. В
этой книге будут рассмотрены три основные группы
паттернов:
* Порождающие паттерны беспокоятся о гибком создании
объектов без внесения в программу лишних зависимостей.
* Структурные паттерны показывают различные способы
построения связей между объектами.
* Поведенческие паттерны заботятся об эффективной
коммуникации между объектами

## Содержание

### Структурные паттерны
* [Мост](Bridge.md)
* [Компоновщик](Composite.md)
* [Декоратор](Decorator.md)
* [Легковес](Flyweight.md)

### Поведенческие паттерны
* [Итератор](Iterator.md)
* [Посредник](Mediator.md)
* [Посетитель](Visitor.md)
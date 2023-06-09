# ПАТТЕРН Стратегия(Strategy)
***
## Автор:

Шамаев Евгений Александрович
***
**Стратегия** — это шаблон проектирования, который позволяет нашему приложению выбирать алгоритмы во время выполнения, что делает наше приложение гибким. В оригинальной книге о шаблонах проектирования, написанной GoF, говорится, что «Стратегический шаблон предназначен для определения семейства алгоритмов, инкапсулирует каждый из них и сделать их взаимозаменяемыми». В частности, он позволяет определить набор алгоритмов, которые взаимозаменяемы в соответствии с некоторыми факторами во время выполнения. Шаблон стратегии подпадает под категорию шаблонов поведенческого проектирования, поскольку он позволяет выбирать поведение алгоритма во время выполнения.   
![](https://images.ctfassets.net/23aumh6u8s0i/4tPzU9nvm5jUwrXAeeHH3e/1badc171178aa953f7df05f08960f228/01_design-types.jpg)
***
# Употребление
При разработке программных приложений у вас может быть несколько альтернатив для достижения чего-либо в вашем коде. В зависимости от выбора клиента, источников данных или других факторов вы хотите сделать что-то другое, не изменяя код. Вы часто склонны определять алгоритмы, используя условные операторы для различных ситуаций в основном классе кода. Но это не элегантный способ написания лучшего кода. Это делает основной класс вашего кода довольно длинным, и становится слишком сложно поддерживать приложение.
В подобных ситуациях стратегический паттерн является идеальным решением. Шаблон стратегии предлагает вам определить классы, называемые стратегиями, для ваших алгоритмов различных ситуаций. На стратегию ссылаются внутри основного класса, называемого контекстом, и код работает в соответствии с этой ситуацией. Контекст не выбирает подходящую стратегию для конкретного случая. Вместо этого клиент передает нужную стратегию в контекст.
Например, если у вас есть шахматное приложение, вы можете выбрать уровень сложности между легким, средним или сложным. Компьютер выбирает алгоритм в соответствии с выбранным вами уровнем. Это один из лучших примеров использования паттерна стратегии.
Паттерн стратегии следует принципу открытия/закрытия; Программное приложение открыто для расширения, но закрыто для модификации. Это означает, что вы можете добавить любое количество дополнительных стратегий, не изменяя основной класс. Это делает ваш код более гибким и простым в обслуживании.
 ![](https://images.ctfassets.net/23aumh6u8s0i/21fvYspoS3Voqv7pKyDE4E/e89c3e5f81d51b3d15a436cd81e795f7/02_UML-Strategy.jpg)
***
  # Диаграммы UML
  Ниже приведена UML-диаграмма паттерна «Стратегия».
  Контекст — это основной класс нашего приложения. В нем сохраняется ссылка на одну из конкретных стратегий.
Стратегия — интерфейс стратегии является общим для всех поддерживаемых стратегий. Контекст может взаимодействовать с другими стратегиями только через интерфейс стратегии.
ConcreteStrategies — это классы, которые реализуют алгоритм с помощью интерфейса Strategy.
  ***
  # Реализация:
  Давайте посмотрим на пошаговый процесс реализации паттерна стратегии.

Сначала вы должны определить алгоритмы, которые вы хотите выполнить в качестве конкретных стратегий в первичном классе.
Определите контекст (основной класс) и добавьте ссылку на стратегию, метод для задания стратегии и еще один метод для выполнения стратегии. Вы также можете определить стратегию по умолчанию, чтобы переключаться между стратегиями только в том случае, если им не нравится стратегия по умолчанию.
Во-первых, мы определяем поле для хранения ссылки на объект стратегии, и два метода, и . Задает стратегию, выбранную, если пользователь выбирает вариант или же тот.strategysetStrategyexecuteStrategysetStrategydefault
Определите интерфейс стратегии, который является общим для всех конкретных стратегий. Интерфейс имеет абстрактный метод, который вы можете изменить в конкретных стратегиях.Strategy
Определите конкретные стратегии, которые должны реализовать интерфейс. Эти конкретные стратегии должны иметь общий метод, который переопределяет метод интерфейса.StrategyexecuteStrategy
Теперь пользователи могут выбрать нужную стратегию во время выполнения. Создайте объект контекста и передайте конкретную стратегию.
Вывод приведенного выше кода будет следующим: 
ConcreteStrategy A
ConcreteStrategy B
Default
Если вы хотите использовать другую стратегию, замените экземпляр ConcreteStrategy нужной стратегией. Вы можете добавить новую конкретную стратегию, ничего не меняя в контексте.
  ***
  # Пример: 
Давайте разработаем игру «камень, ножницы, бумага», используя стратегический шаблон. Вы можете выбрать любую стратегию из камня, ножниц, бумаги и случайного варианта, чтобы играть против компьютера. В приведенном ниже примере кода шаблон стратегии используется для реализации различных стратегий.
  ```
## Changing the strategy among Rock, Paper, Scissors, and Random
import random
from abc import ABC, abstractmethod
## Strategy interface 
class Strategy(ABC):
    @abstractmethod
    def selection(self) -> None:
        pass
## Concrete strategies
class Rock(Strategy):
    ## actual application will have the algorithm instead this method
    def selection(self) -> str:
        return "Rock"
class Paper(Strategy):
    def selection(self) -> str:
        return "Paper"
class Scissors(Strategy):
    def selection(self) -> str:
        return "Scissors"
class Random(Strategy):
    def selection(self) -> str:
        options = ["Rock", "Paper", "Scissors"]
        return random.choice(options)
## Context class
class Game:
    strategy: Strategy
    def __init__(self, strategy: Strategy = None) -> None:
        if strategy is not None:
            self.strategy = strategy
        else:
            self.strategy = Random()
    def play(self, sec) -> None:
        s1 = self.strategy.selection()
        s2 = sec.strategy.selection()
        if s1 == s2:
            print("It's a tie")
        elif s1 == "Rock":
            if s2 == "Scissors":
                print("Player 1 wins!")
            else:
                print("Player 2 wins!")
        elif s1 == "Scissors":
            if s2 == "Paper":
                print("Player 1 wins!")
            else:
                print("Player 2 wins!")
        elif s1 == "Paper":
            if s2 == "Rock":
                print("Player 1 wins!")
            else:
                print("Player 2 wins!")
## Example application
## PLayer 1 can select his strategy
player1 = Game(Paper())
# Player 2 gets to select
player2 = Game(Rock())
# After the second player choice, we call the play method
player1.play(player2)
```
В соответствии со стратегиями, выбранными двумя участниками, ожидаемый результат будет следующим:

Player 1 wins!
Протестируйте все остальные случаи игры, используя все остальные стратегии. Чтобы добавить больше удовольствия в игру, попробуйте создать еще две стратегии к приведенному выше примеру в соответствии с расширением Lizard-Spock.
 # Заключение

В этой статье вы рассмотрели, где и как использовать шаблон стратегии в своем коде. Вы можете создавать гибкие и удобные в обслуживании программные приложения, используя шаблон стратегии. Вы можете переключаться между алгоритмами во время выполнения в соответствии с решением пользователя, не изменяя код. Но если в вашем коде всего пара алгоритмов, нет необходимости использовать стратегию. Это просто делает ваш код сложным с многочисленными классами и объектами. Паттерн Strategy может работать в качестве альтернативы условным операторам для выбора поведения приложения. Но потенциальный недостаток шаблона стратегии заключается в том, что пользователи должны знать, чем стратегии отличаются друг от друга, чтобы выбрать то, что им нужно. Поэтому было бы лучше, если бы вы использовали шаблон стратегии только тогда, когда изменение поведения приложения актуально для пользователей. Поэтому постарайтесь сделать свои программные приложения гибкими, используя шаблон стратегии.

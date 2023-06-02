 ПАТТЕРН Наблюдатель(Observer)
***
## Авторы:

Ижбулдин Илья Евгеньевич 
Иксанов Алмаз Рифович
***
***Наблюдатель*** — представляет поведенческий шаблон проектирования, который использует отношение "один ко многим". В этом отношении есть один наблюдаемый объект и множество наблюдателей. И при изменении наблюдаемого объекта автоматически происходит оповещение всех наблюдателей.

Данный паттерн еще называют Publisher-Subscriber (издатель-подписчик), поскольку отношения издателя и подписчиков характеризуют действие данного паттерна: подписчики подписываются email-рассылку определенного сайта. Сайт-издатель с помощью email-рассылки уведомляет всех подписчиков о изменениях. А подписчики получают изменения и производят определенные действия: могут зайти на сайт, могут проигнорировать уведомления и т.д.
![](https://radioprog.ru/uploads/media/articles/0001/06/fa410a58a5b06165b1e9510b455cf43c495b6324.png)
***
# Проблема
Представьте, что вы имеете два объекта: Покупатель и Магазин. В магазин вот-вот должны завезти новый товар, который интересен покупателю.

Покупатель может каждый день ходить в магазин, чтобы проверить наличие товара. Но при этом он будет злиться, без толку тратя своё драгоценное время.
![](https://radioprog.ru/uploads/media/articles/0001/06/6aea5488bd6c4b6fdf80ab14544963391b328497.png)  
С другой стороны, магазин может разослать спам каждому своему покупателю. Многих это расстроит, так как товар специфический, и не всем он нужен.

Получается конфликт: либо покупатель тратит время на периодические проверки, либо магазин тратит ресурсы на бесполезные оповещения.
 
***
  # Решение
 Давайте называть Издателями те объекты, которые содержат важное или интересное для других состояние. Остальные объекты, которые хотят отслеживать изменения этого состояния, назовём Подписчиками.

Паттерн Наблюдатель предлагает хранить внутри объекта издателя список ссылок на объекты подписчиков, причём издатель не должен вести список подписки самостоятельно. Он предоставит методы, с помощью которых подписчики могли бы добавлять или убирать себя из списка.
![](https://radioprog.ru/uploads/media/articles/0001/06/3bd13129cebf1bca55947d023270254cdc2db4a8.png)   
Теперь самое интересное. Когда в издателе будет происходить важное событие, он будет проходиться по списку подписчиков и оповещать их об этом, вызывая определённый метод объектов-подписчиков.

Издателю безразлично, какой класс будет иметь тот или иной подписчик, так как все они должны следовать общему интерфейсу и иметь единый метод оповещения.  
![](https://radioprog.ru/uploads/media/articles/0001/06/b9de96441d0d5c807a1f804233a7d5738499aab8.png) 
Увидев, как складно всё работает, вы можете выделить общий интерфейс, описывающий методы подписки и отписки, и для всех издателей. После этого подписчики смогут работать с разными типами издателей, а также получать оповещения от них через один и тот же метод.

  ***
  # Пример: 
  ```
  from abc import ABCMeta, abstractmethod

class IObservable(metaclass=ABCMeta):
    "The Subject Interface"

    @staticmethod
    @abstractmethod
    def subscribe(observer):
        "The subscribe method"

    @staticmethod
    @abstractmethod
    def unsubscribe(observer):
        "The unsubscribe method"

    @staticmethod
    @abstractmethod
    def notify(observer):
        "The notify method"

class Subject(IObservable):
    "The Subject (Observable)"

    def __init__(self):
        self._observers = set()

    def subscribe(self, observer):
        self._observers.add(observer)

    def unsubscribe(self, observer):
        self._observers.remove(observer)

    def notify(self, *args):
        for observer in self._observers:
            observer.notify(*args)

class IObserver(metaclass=ABCMeta):
    "A method for the Observer to implement"

    @staticmethod
    @abstractmethod
    def notify(observable, *args):
        "Receive notifications"

class Observer(IObserver):
    "The concrete observer"

    def __init__(self, observable):
        observable.subscribe(self)

    def notify(self, *args):
        print(f"Observer id:{id(self)} received {args}")

# The Client
SUBJECT = Subject()
OBSERVER_A = Observer(SUBJECT)
OBSERVER_B = Observer(SUBJECT)

SUBJECT.notify("First Notification", [1, 2, 3])

SUBJECT.unsubscribe(OBSERVER_B)
SUBJECT.notify("Second Notification", {"A": 1, "B": 2, "C": 3})Ы
```
 # Преимущества и недостатки

### Преимущества

* Издатели не зависят от конкретных классов подписчиков и наоборот.
* Вы можете подписывать и отписывать получателей на лету.
* Реализует принцип открытости/закрытости.

### Недостатки

* Подписчики оповещаются в случайном порядке.

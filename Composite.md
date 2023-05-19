Паттерны проектирования
=======================

Автор: Гарафиева Венера

Паттерн Composite / "Компоновщик"
--------------
**Компоновщик** – это структурный паттерн проектирования, который позволяет сгруппировать множество объектов в древовидную структуру, а затем работать с ней так, как будто это единый объект.
![](https://radioprog.ru/uploads/media/articles/0001/06/fd643b6ac0ff7b8bffc9e8aec8b2cffef87e0f7b.png)

Паттерн определяет иерархию классов, которые одновременно могут состоять из примитивных и сложных объектов, упрощает архитектуру клиента, делает процесс добавления новых видов объекта более простым.o

Паттерн Компоновщик имеет смысл только тогда, когда основная модель вашей программы может быть структурирована в виде дерева.Паттерн Компоновщик имеет смысл только тогда, когда основная модель вашей программы может быть структурирована в виде дерева.

Пример кода:

```python
from abc import ABC, abstractmethod


class IProduct(ABC):
    """Интерфейс продуктов
    входящих в пиццу"""
    @abstractmethod
    def cost(self) -> float:
        pass

    @abstractmethod
    def name(self) -> str:
        pass


class Product(IProduct):
    """Класс продукта"""
    def __init__(self, name: str, cost: float):
        self.__cost = cost
        self.__name = name

    def cost(self) -> float:
        return self.__cost

    def name(self) -> str:
        return self.__name


class CompoundProduct(IProduct):
    """Класс компонуемых продуктов"""
    def __init__(self, name: str):
        self.__name = name
        self.products = []

    def cost(self):
        cost = 0
        for it in self.products:
            cost += it.cost()
        return cost

    def name(self) -> str:
        return self.__name

    def add_product(self, product: IProduct):
        self.products.append(product)

    def remove_product(self, product: IProduct):
        self.products.remove(product)

    def clear(self):
        self.products = []


class Pizza(CompoundProduct):
    """Класс пиццы"""
    def __init__(self, name: str):
        super(Pizza, self).__init__(name)

    def cost(self):
        cost = 0
        for it in self.products:
            cost_it = it.cost()
            print(f"Стоимость '{it.name()}' = {cost_it} тугриков")
            cost += cost_it
        print(f"Стоимость пиццы '{self.name()}' = {cost} тугриков")
        return cost


if __name__ == "__main__":
    dough = CompoundProduct("тесто")
    dough.add_product(Product("мука", 3))
    dough.add_product(Product("яйцо", 2.3))
    dough.add_product(Product("соль", 1))
    dough.add_product(Product("сахар", 2.1))
    sauce = Product("Барбекю", 12.1)
    topping = CompoundProduct("топпинг")
    topping.add_product(Product("Дор блю", 14))
    topping.add_product(Product("Пармезан", 12.3))
    topping.add_product(Product("Моцарелла", 9.54))
    topping.add_product(Product("Маасдам", 7.27))
    pizza = Pizza("4 сыра")
    pizza.add_product(dough)
    pizza.add_product(sauce)
    pizza.add_product(topping)
    print(pizza.cost())
```
Output:

_Стоимость 'тесто' = 8.4 тугриков_ 

_Стоимость 'Барбекю' = 12.1 тугриков_

_Стоимость 'топпинг' = 43.11 тугриков_

_Стоимость пиццы '4 сыра' = 63.61 тугриков_

_63.61_

Применимость
--------------

1. Когда вам нужно представить древовидную структуру объектов.

_Паттерн Компоновщик предлагает хранить в составных объектах ссылки на другие простые или составные объекты. Те, в свою очередь, тоже могут хранить свои вложенные объекты и так далее._

_В итоге вы можете строить сложную древовидную структуру данных, используя всего две основные разновидности объектов._

2. Когда клиенты должны единообразно трактовать простые и составные объекты.

_Благодаря тому, что простые и составные объекты реализуют общий интерфейс, клиенту безразлично, с каким именно объектом ему предстоит работать._

Преимущества
--------------

1. Упрощает архитектуру клиента при работе со сложным деревом компонентов.

2. Облегчает добавление новых видов компонентов.

Недостатки
--------------

1. Создаёт слишком общий дизайн классов.
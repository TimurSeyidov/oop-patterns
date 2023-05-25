## Паттерн Bridge / "Мост"
Авторы: Гарафиева Венера, Егоров Вадим

---
**Мост** — это **структурный** паттерн проектирования, который разделяет один или несколько классов на две отдельные иерархии — **абстракцию** и **реализацию**, позволяя изменять их независимо друг от друга.

![](https://avatars.mds.yandex.net/get-images-cbir/1016912/3jHYX1P0OQoEviShkGfQag8140/ocr)

Проблема
--------

Отделить абстракцию от реализации так, чтобы и то и другое можно было изменять независимо. При использовании наследования реализация жестко привязывается к абстракции, что затрудняет независимую модификацию.

![](https://avatars.mds.yandex.net/get-images-cbir/4498648/bLqBe4s0pBOeotclPGQnJw8203/ocr)

Неудачный пример кода:
```python
class Shape:
    def __init__(self, name):
        self.length = name


class RedTriangle(Shape):
    def __init__(self, name):
        super().__init__(name)


class RedSquare(Shape):
    def __init__(self, name):
        super().__init__(name)


class BlueTriangle(Shape):
    def __init__(self, name):
        super().__init__(name)


class BlueSquare(Shape):
    def __init__(self, name):
        super().__init__(name)
```
Решение
--------
![](https://avatars.mds.yandex.net/get-images-cbir/4488919/ZwLAdYy4L5YTkI1hci9O1w8337/ocr)
Правильный код:
```python
class Color:
    def __init__(self):
        self.name = None


class Red(Color):
    def __init__(self):
        self.name = 'red'


class Blue(Color):
    def __init__(self):
        self.name = 'blue'

class Shape:
    def __init__(self, color):
        self.color = color
        self.name = None

    def say_hello(self):
        print(
            "Hello, I'm " + self.name +
            " and my color is " +
            self.color.name
        )

class Circle(Shape):
    def __init__(self, color):
        self.color = color
        self.name = 'Circle'
        
if __name__ == '__main__':
    red_circle = Circle(Red())
    red_circle.say_hello()

    blue_circle = Circle(Blue())
    blue_circle.say_hello()
```
Вывод:
 _Hello, I'm Circle and my color is red_ 
 _Hello, I'm Circle and my color is blue_

Преимущество
------------

1.  Позволяет строить платформо-независимые программы.
2.  Скрывает лишние или опасные детали реализации от клиентского кода.
3.  Реализует принцип открытости/закрытости.

Недостатки
----------

1.  Усложняет код программы из-за введения дополнительных классов.

 Применимость
------------

1.  Когда необходимо разделить монолитный класс, который содержит несколько различных реализаций какой-то функциональности
2.  Когда класс нужно расширять в двух независимых плоскостях.
3.  Когда изменения в реализации абстракции не должны сказываться на клиентах, то есть клиентский код не должен перекомпилироваться;
4.  Когда вы хотите, чтобы реализацию можно было бы изменять во время выполнения программы.

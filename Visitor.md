## Паттерн Visitor / "Посетитель"

Автор: Сибагатуллин Алик

-------------

**Паттерн посетитель** - поведенческий паттерн проектирвоания, который описывает операцию, выполняемую с каждым объектом из некоторой структуры.

Паттерн посетитель позволяет определить новую операцию, не изменяя классы этих объектов.


Паттерн посетитель позволяет определить новую операцию, не изменяя классы этих объектов.Поведенческий паттерн проектирвоания, который описывает операцию, выполняемую с каждым объектом из некоторой структуры.
Посетитель (англ. visitor) — поведенческий шаблон проектирования, описывающий операцию, которая выполняется над объектами других классов. При изменении visitor нет необходимости изменять обслуживаемые классы.
Шаблон демонстрирует классический приём восстановления информации о потерянных типах, не прибегая к понижающему приведению типов при помощи двойной диспетчеризации.
Шаблон следует использовать, если:

1)имеются различные объекты разных классов с разными интерфейсами, но над ними нужно совершать операции, зависящие от конкретных классов;

2)необходимо над структурой выполнить различные, усложняющие структуру операции;

3)часто добавляются новые операции над структурой.

![image](https://github.com/TimurSeyidov/oop-patterns/assets/98407097/ab0e3a4b-5d87-4b09-a460-5dd83970f482)

Преимущества:

* Упрощается добавление новых операций;

* Объединение родственных операции в классе Visitor;

* Класс Visitor может запоминать в себе какое-то состояние по мере обхода контейнера.

* Принцип Open/Closed: легко ввести новое поведение в классе, которое может работать с объектами разных классов без внесения изменений в эти классы.

* Принцип единой ответственности: несколько версий одного и того же поведения могут работать в одном и том же классе.

* Добавление сущностей: добавление сущности в метод посетителя очень просто, поскольку нам нужно вносить изменения только в класс посетителя, и это не повлияет на существующий элемент.

* Обновление логики: если логика работы обновляется, нам нужно внести изменения только в реализацию посетителя, а не во все классы элементов.


Недостатки:
* Много обновлений: мы должны обновлять каждого посетителя всякий раз, когда класс добавляется или удаляется из основной иерархии.

* Трудно расширить: если классов посетителей слишком много, становится очень сложно расширить весь интерфейс класса.

* Отсутствие доступа: иногда посетители могут не иметь доступа к закрытым полям определенных классов, с которыми они должны работать.

* Затруднено добавление новых классов, поскольку нужно обновлять иерархию посетителя и его сыновей

![VisitorDiagram svg](https://github.com/TimurSeyidov/oop-patterns/assets/98407097/b2acff2f-309d-494f-832b-ab507c017d5a)

**Input**

``` python

class Courses_At_GFG:
 
    def accept(self, visitor):
        visitor.visit(self)
 
    def teaching(self, visitor):
        print(self, "Taught by ", visitor)
 
    def studying(self, visitor):
        print(self, "studied by ", visitor)
 
 
    def __str__(self):
        return self.__class__.__name__
 
 

class SDE(Courses_At_GFG): pass
 
class STL(Courses_At_GFG): pass
 
class DSA(Courses_At_GFG): pass
 
 
""" Abstract Visitor class for Concrete Visitor classes:
 method defined in this class will be inherited by all
 Concrete Visitor classes."""
class Visitor:
 
    def __str__(self):
        return self.__class__.__name__
 
 
""" Concrete Visitors: Classes visiting Concrete Course objects.
 These classes have a visit() method which is called by the
 accept() method of the Concrete Course_At_GFG classes."""
class Instructor(Visitor):
    def visit(self, crop):
        crop.teaching(self)
 
 
class Student(Visitor):
    def visit(self, crop):
        crop.studying(self)
 
 
"""creating objects for concrete classes"""
sde = SDE()
stl = STL()
dsa = DSA()
 
"""Creating Visitors"""
instructor = Instructor()
student = Student()
 
"""Visitors visiting courses"""
sde.accept(instructor)
sde.accept(student)
 
stl.accept(instructor)
stl.accept(student)
 
dsa.accept(instructor)
dsa.accept(student)
```

**Output**
```
SDE Taught by  Instructor

SDE studied by  Student

STL Taught by  Instructor

STL studied by  Student

DSA Taught by  Instructor

DSA studied by  Student
```

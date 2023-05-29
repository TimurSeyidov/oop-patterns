Паттерны проектирования
=======================

Автор: Гильмиярова Зиля

**Шаблонный метод**
---------------

**Шаблонный метод** — это поведенческий паттерн проектирования, который определяет скелет алгоритма, перекладывая ответственность за некоторые его шаги на подклассы. Паттерн позволяет подклассам переопределять шаги алгоритма, не меняя его общей структуры. Шаблонный метод (Template Method) определяет общий алгоритм поведения подклассов, позволяя им переопределить отдельные шаги этого алгоритма без изменения его структуры.

**Когда использовать шаблонный метод?**  Когда планируется, что в будущем подклассы должны будут переопределять различные этапы алгоритма без изменения его структуры Когда в классах, реализующим схожий алгоритм, происходит дублирование кода. Вынесение общего кода в шаблонный метод уменьшит его дублирование в подклассах. Схематично в UML алгоритм можно изобразить следующим образом:

![](https://metanit.com/sharp/patterns/pics/templatemethod.png)

Формальная реализация паттерна на C#:


```python
abstract class AbstractClass
{
    public void TemplateMethod()
    {
        Operation1();
        Operation2();
    }
    public abstract void Operation1();
    public abstract void Operation2();
}
 
class ConcreteClass : AbstractClass
{
    public override void Operation1()
    {
    }
 
    public override void Operation2()
    {
    }
}
```

**Участники AbstractClass:** определяет шаблонный метод TemplateMethod(), который реализует алгоритм. Алгоритм может состоять из последовательности вызовов других методов, часть из которых может быть абстрактными и должны быть переопределены в классах-наследниках. При этом сам метод TemplateMethod(), представляющий структуру алгоритма, переопределяться не должен. ConcreteClass: подкласс, который может переопределять различные методы родительского класса.
Рассмотрим применение на конкретном примере. Допустим, в нашей программе используются объекты, представляющие учебу в школе и в вузе:
```python
abstract class Learning
class School
{
    // идем в первый класс
    public void Enter() { }
    // обучение
    public void Study() { }
    // сдаем выпускные экзамены
    public void PassExams() { }
    // получение аттестата об окончании
    public void GetAttestat() { }
}
 
class University
{
    // поступление в университет
    public void Enter() { }
    // обучение
    public void Study() { }
    // проходим практику
    public void Practice() { }
    // сдаем выпускные экзамены
    public void PassExams() { }
    // получение диплома
     public void GetDiploma() { }
}
```
Как видно, эти классы очень похожи, и самое главное, реализуют примерно общий алгоритм. Да, где-то будет отличаться реализация методов, где-то чуть больше методов, но в целом мы имеем общий алгоритм, а функциональность обоих классов по большому счету дублируется. Поэтому для улучшения структуры классов мы могли бы применить шаблонный метод:
```python
class Program
{
    static void Main(string[] args)
    {
        School school = new School();
        University university = new University();
 
        school.Learn();
        university.Learn();
 
        Console.Read();
    }
}
abstract class Education
{
    public void Learn()
    {
        Enter();
        Study();
        PassExams();
        GetDocument();
    }
    public abstract void Enter();
    public abstract void Study();
    public virtual void PassExams()
    {
        Console.WriteLine("Сдаем выпускные экзамены");
    }
    public abstract void GetDocument();
}
     
class School : Education
{
    public override void Enter()
    {
        Console.WriteLine("Идем в первый класс");
    }
 
    public override void Study()
    {
        Console.WriteLine("Посещаем уроки, делаем домашние задания");
    }
 
    public override void GetDocument()
    {
        Console.WriteLine("Получаем аттестат о среднем образовании");
    }
}
 
class University : Education
{
    public override void Enter()
    {
        Console.WriteLine("Сдаем вступительные экзамены и поступаем в ВУЗ");
    }
 
    public override void Study()
    {
        Console.WriteLine("Посещаем лекции");
        Console.WriteLine("Проходим практику");
    }
 
    public override void PassExams()
    {
        Console.WriteLine("Сдаем экзамен по специальности");
    }
 
    public override void GetDocument()
    {
        Console.WriteLine("Получаем диплом о высшем образовании");
    }
}
```
При этом в базовом классе необязательно определять все методы алгоритма как абстрактные. Можно определить реализацию методов по умолчанию, как в случае с методом PassExams().
И в результате работы программы консоль выведет:
```python
Идем в первый класс
Посещаем уроки, делаем домашние задания
Сдаем выпускные экзамены
Получаем аттестат о среднем образовании
Сдаем вступительные экзамены и поступаем в ВУЗ
Посещаем лекции
Проходим практику
Сдаем экзамен по специальности
Получаем диплом о высшем образовании
```
В данном случае надо отметить, что несмотря на то, что мы не можем в производном классе переопределить шаблонный метод Learn(), тем не менее мы можем скрыть реализацию этого метода в классе-наследнике:
```python
class School : Education
{
    public new void Learn()
    {
    Console.WriteLine("Не хочу учиться");
    }
    //..........................................
}
```
В итоге реализация шаблонного метода не будет иметь смысла.

Также надо отметить ситуацию с наследованием базового класса. Например, у нас может быть ситуация, когда базовый класс Education сам наследует метод Learn от другого класса:
```python
abstract class Learning
{
    public abstract void Learn();
}
abstract class Education :Learning
{
    public sealed override void Learn()
    {
        Enter();
        Study();
        PassExams();
        GetDocument();
    }
    //  остальные методы класса
}
```
Чтобы сокрыть алгоритм от изменения в классах наследниках, метод Learn объявляется с ключевым словом **sealed**.

Паттерны проектирования
=======================

Автор: Гильмиярова Зиля

**Шаблонный метод**
---------------

**Шаблонный метод в Pyton** —  это поведенческий шаблон проектирования, который определяет скелет операции и оставляет детали для реализации дочерним классом. Его подклассы могут переопределять реализации метода в соответствии с потребностями, но вызов должен выполняться таким же образом, как определено абстрактным классом. Это один из самых простых для понимания и реализации поведенческих шаблонов проектирования. Такие методы широко используются при разработке фреймворков, поскольку они позволяют нам повторно использовать один фрагмент кода в разных местах, внося определенные изменения. Это также позволяет избежать дублирования кода.

Решение с использованием шаблонного метода:


```python
"" method to get the text of file"""
def get_text():
     
    return "plain_text"
 
""" method to get the xml version of file"""
def get_xml():
     
    return "xml"
 
""" method to get the pdf version of file"""
def get_pdf():
     
    return "pdf"
 
"""method to get the csv version of file"""
def get_csv():
     
    return "csv"
 
"""method used to convert the data into text format"""
def convert_to_text(data):
     
    print("[CONVERT]")
    return "{} as text".format(data)
 
"""method used to save the data"""
def saver():
     
    print("[SAVE]")
 
"""helper function named as template_function"""
def template_function(getter, converter = False, to_save = False):
 
    """input data from getter"""
    data = getter()
    print("Got `{}`".format(data))
 
    if len(data) <= 3 and converter:
        data = converter(data)
    else:
        print("Skip conversion")
     
    """saves the data only if user want to save it"""
    if to_save:
        saver()
 
    print("`{}` was processed".format(data))
 
 
"""main method"""
if __name__ == "__main__":
 
    template_function(get_text, to_save = True)
 
    template_function(get_pdf, converter = convert_to_text)
 
    template_function(get_csv, to_save = True)
 
    template_function(get_xml, to_save = True)
```
**Вывод**
```python
Got `plain_text`
Skip conversion
[SAVE]
`plain_text` was processed
Got `pdf`
[CONVERT]
`pdf as text` was processed
Got `csv`
Skip conversion
[SAVE]
`csv` was processed
```
**Ниже приведена диаграмма классов для шаблонного метода**

![](https://media.geeksforgeeks.org/wp-content/uploads/20200218140413/Untitled-Diagram74.png)

**Преимущества**

**Эквивалентное содержимое**: Легко рассмотреть дублирующийся код в суперклассе, переместив его туда, где вы хотите его использовать.
**Гибкость**: он обеспечивает огромную гибкость, так что подклассы могут решать, как реализовать этапы алгоритмов.
**Возможность наследования**: мы можем повторно использовать наш код, поскольку шаблонный метод использует наследование, которое обеспечивает возможность повторного использования кода.

**Недостатки**

**Сложный код**: иногда при использовании шаблонного метода код может стать настолько сложным, что даже разработчикам, которые его пишут, становится слишком сложно понять код.
**Ограниченность**: клиенты могут запросить расширенную версию, потому что иногда они чувствуют нехватку алгоритмов в предоставленном скелете.
**Нарушение**: Вполне возможно, что при использовании шаблонного метода вы можете в конечном итоге нарушить принцип подстановки Лискова, которому определенно не стоит следовать.

**Участники AbstractClass:** определяет шаблонный метод TemplateMethod(), который реализует алгоритм. Алгоритм может состоять из последовательности вызовов других методов, часть из которых может быть абстрактными и должны быть переопределены в классах-наследниках. При этом сам метод TemplateMethod(), представляющий структуру алгоритма, переопределяться не должен. ConcreteClass: подкласс, который может переопределять различные методы родительского класса.

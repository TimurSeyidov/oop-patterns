ПАТТЕРНЫ ПРОЕКТИРОВАНИЯ
=======================

* * *

Авторы:
-------

Шамсутдинова Оксана  
Шипаева Лариса

Паттерн Декоратор (Decorator)
=============================

* * *

**Декоратор** — это структурный паттерн. Цель которого – предоставление новых функциональных возможностей классам и объектам во время выполнения кода. Чаще всего декоратор представляет собой абстрактный класс, принимающий в конструкторе объект, функциональность которого мы хотим расширить  

Decorator(Декоратор) ![](https://pythonru.com/wp-content/uploads/2020/09/rukovodstvo-po-dekoratoram-python.png)

Проблема
--------

Возложить дополнительные обязанности (прозрачные для клиентов) на отдельный объект, а не на класс в целом.

Представьте, что мы работаем с инструментом форматирования, который предоставляет такие функции, как выделение текста жирным шрифтом и подчеркивание текста. Но через некоторое время наши инструменты форматирования получили известность среди целевой аудитории, и, выслушав отзывы, мы получили, что наша аудитория хочет больше функций в приложении, таких как выделение текста курсивом и многие другие функции.
Выглядит просто? Реализовать это или расширить наши классы, чтобы добавить больше функциональных возможностей, не нарушая существующий клиентский код, непростая задача, потому что мы должны поддерживать принцип единой ответственности.

Решение
--------

Теперь давайте посмотрим на решение, которое у нас есть, чтобы избежать таких условий. Изначально у нас есть только writentext, но мы должны применить такие фильтры, как полужирный шрифт, курсив, ПОДЧЕРКИВАНИЕ. Итак, мы создадим отдельные классы-оболочки для каждой функции, такие как BoldWrapperClass, ItalicWrapperClass и UnderlineWrapperclass.

Сначала мы вызовем BoldWrapperclass над написанным текстом, который в конечном итоге преобразует текст в буквы, выделенные жирным шрифтомю.

Затем мы применим классы ItalicWrapperClass и UnderlineWrapperClass поверх текста, выделенного жирным шрифтом, что даст нам желаемый результат.

Следующий код написан с использованием метода декоратора:
```python
class WrittenText:
 
    """Represents a Written text """
 
    def __init__(self, text):
        self._text = text
 
    def render(self):
        return self._text
 
class UnderlineWrapper(WrittenText):
 
    """Wraps a tag in <u>"""
 
    def __init__(self, wrapped):
        self._wrapped = wrapped
 
    def render(self):
        return "<u>{}</u>".format(self._wrapped.render())
 
class ItalicWrapper(WrittenText):
 
    """Wraps a tag in <i>"""
 
    def __init__(self, wrapped):
        self._wrapped = wrapped
 
    def render(self):
        return "<i>{}</i>".format(self._wrapped.render())
 
class BoldWrapper(WrittenText):
 
    """Wraps a tag in <b>"""
 
    def __init__(self, wrapped):
        self._wrapped = wrapped
 
    def render(self):
        return "<b>{}</b>".format(self._wrapped.render())
 
""" main method """
 
if __name__ == '__main__':
 
    before_gfg = WrittenText("GeeksforGeeks")
    after_gfg = ItalicWrapper(UnderlineWrapper(BoldWrapper(before_gfg)))
 
    print("before :", before_gfg.render())
    print("after :", after_gfg.render())
```


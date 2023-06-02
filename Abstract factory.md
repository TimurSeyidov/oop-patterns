# ПАТТЕРН Абстрактная фабрика(Abstract factory)
***
## Авторы:

Аскаров Алмаз Димович
***
**Абстрактная фабрика** — порождающий шаблон проектирования, предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не специфицируя их конкретных классов. Шаблон реализуется созданием абстрактного класса Factory, который представляет собой интерфейс для создания компонентов системы (например, для оконного интерфейса он может создавать окна и кнопки). Затем пишутся классы, реализующие этот интерфейс.   
![](https://stackabuse.s3.amazonaws.com/media/abstract-factory-design-pattern-in-python-01.jpeg)
***
# Реализация паттерна абстрактная фабрика с примером на python.

Допустим у нас есть 2 операционные системы Mac Os и Linux, нам необходимо реализовать произвольное окно с кнопкой или кнопками в каждой из этих операционных систем. Естественно окна и кнопки в MacOs и linux выглядят по разному, так сказать имеют разные стили оформления.
Наша задача состоит в том, что бы создать универсальный интерфейс который будет создавать окно с кнопкой в не зависимости от того на какой операционной системе запущенна программа.
Т.е. функции которая будет конструировать окно с кнопкой, будет работать вне зависимости от того какое окно мы захотим создать.
Ознакомившись с терминологией, давайте попробуем реализовать шаблон абстрактной фабрики в Python.
***
  # Пример: 
  ```from abc import ABC, abstractmethod

class Browser(ABC):
    """
    Creates "Abstract Product A"
    """

    # Interface - Create Search Toolbar
    @abstractmethod
    def create_search_toolbar(self):
        pass

    # Interface - Create Browser Window
    @abstractmethod
    def create_browser_window(self):
        pass

class Messenger(ABC):
    """
    Creates "Abstract Product B"
    """

    @abstractmethod
    # Interface - Create Messenger Window
    def create_messenger_window(self):
        pass
        class VanillaBrowser(Browser):
    """
    Type: Concrete Product
    Abstract methods of the Browser base class are implemented.
    """

    # Interface - Create Search Toolbar
    def create_search_toolbar(self):
        print("Search Toolbar Created")

    # Interface - Create Browser Window]
    def create_browser_window(self):
        print("Browser Window Created")


class VanillaMessenger(Messenger):
    """
    Type: Concrete Product
    Abstract methods of the Messenger base class are implemented.
    """

    # Interface - Create Messenger Window
    def create_messenger_window(self):
        print("Messenger Window Created")

class SecureBrowser(Browser):
    """
    Type: Concrete Product
    Abstract methods of the Browser base class are implemented.
    """

    # Abstract Method of the Browser base class
    def create_search_toolbar(self):
        print("Secure Browser - Search Toolbar Created")

    # Abstract Method of the Browser base class
    def create_browser_window(self):
        print("Secure Browser - Browser Window Created")

    def create_incognito_mode(self):
        print("Secure Browser - Incognito Mode Created")


class SecureMessenger(Messenger):
    """
    Type: Concrete Product
    Abstract methods of the Messenger base class are implemented.
    """

    # Abstract Method of the Messenger base class
    def create_messenger_window(self):
        print("Secure Messenger - Messenger Window Created")

    def create_privacy_filter(self):
        print("Secure Messenger - Privacy Filter Created")

    def disappearing_messages(self):
        print("Secure Messenger - Disappearing Messages Feature Enabled")
```
Вы можете заметить, что помимо абстрактных методов, к конкретным продуктам добавляются дополнительные функции, чтобы сделать их функциональными в их собственном контексте.
Мы почти у цели. Давайте теперь создадим саму абстрактную фабрику и соответствующие бетонные фабрики как:
 ```class AbstractFactory(ABC):
    """
    The Abstract Factory
    """

    @abstractmethod
    def create_browser(self):
        pass

    @abstractmethod
    def create_messenger(self):
        pass

class VanillaProductsFactory(AbstractFactory):
    """
    Type: Concrete Factory
    Implement the operations to create concrete product objects.
    """

    def create_browser(self):
        return VanillaBrowser()

    def create_messenger(self):
        return VanillaMessenger()

class SecureProductsFactory(AbstractFactory):
    """
    Type: Concrete Factory
    Implement the operations to create concrete product objects.
    """

    def create_browser(self):
        return SecureBrowser()

    def create_messenger(self):
        return SecureMessenger()
        
        def main():
    for factory in (VanillaProductsFactory(), SecureProductsFactory()):
        product_a = factory.create_browser()
        product_b = factory.create_messenger()
        product_a.create_browser_window()
        product_a.create_search_toolbar()
        product_b.create_messenger_window()

if __name__ == "__main__":
    main()

```
### Преимущества

*Основным преимуществом этого шаблона является гибкость - возможность добавлять новые возможности и функции к существующим продуктам или, возможно, даже добавлять новые бетонные изделия на бетонные заводы. Это можно сделать, не саботируя весь код.
*Прямое взаимодействие между клиентом и конкретными изделиями минимально. Существует также гибкость в организации и уплотнении кода.

### Недостатки

*Основным недостатком этого шаблона является удобочитаемость и удобство сопровождения кода. Несмотря на то, что он предоставляет гибкий способ добавления новых вариантов будущего, добавление нового компонента потребует добавления к конкретным классам, изменения интерфейсов и т. д. Каскадные эффекты модификации требуют времени на разработку.
### Заключение

Абстрактный фабричный шаблон может быть очень эффективно использован для тесно связанных семейств различных продуктов, в отличие от фабричного шаблона, который можно использовать только для одного типа продукта.Шаблон Abstract Factory Pattern решает серьезную проблему, связанную с необходимостью написания чистого кода. Мы рассмотрели основы этого паттерна, а также разобрались в реализации на примере.

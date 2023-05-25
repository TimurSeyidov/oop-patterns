# ПАТТЕРНЫ ПРОЕКТИРОВАНИЯ
***
## Авторы:

Ямалиева Карина Радисовна  
Шайхуллина Диля Марсельевна

### Паттерн прокси (Proxy) или по другому "Заместитель"
***
**Паттерн Proxy** является суррогатом или замеcтителем другого объекта и контролирует доступ к нему. Предоставляя дополнительный уровень косвенности при доступе к объекту, может применяться для поддержки распределенного, управляемого или интеллектуального доступа.
![](https://user-images.githubusercontent.com/68103697/143173227-6366f90d-1d29-45bb-b4ad-d3887e67e0ef.png)
***
# Проблема

Для чего вообще контролировать доступ к объектам? Рассмотрим такой пример: у вас есть внешний ресурсоёмкий объект, который нужен не всё время, а изредка.

Мы могли бы создавать этот объект не в самом начале программы, а только тогда, когда он кому-то реально понадобится. Каждый клиент объекта получил бы некий код отложенной инициализации. Но, вероятно, это привело бы к множественному дублированию кода.

В идеале, этот код хотелось бы поместить прямо в служебный класс, но это не всегда возможно. Например, код класса может находиться в закрытой сторонней библиотеке.  
  ![](https://radioprog.ru/uploads/media/articles/0001/06/176ab278d373df58b44b5c54c166ac6ad76337d6.png)
***
  # Решение
  Паттерн Заместитель предлагает создать новый класс-дублёр, имеющий тот же интерфейс, что и оригинальный служебный объект. При получении запроса от клиента объект-заместитель сам бы создавал экземпляр служебного объекта и переадресовывал бы ему всю реальную работу.  
  ![](https://radioprog.ru/uploads/media/articles/0001/06/95339012955208c8eee66251b641c3ac81e4a585.png)  
  
Но в чём же здесь польза? Вы могли бы поместить в класс заместителя какую-то промежуточную логику, которая выполнялась бы до (или после) вызовов этих же методов в настоящем объекте. А благодаря одинаковому интерфейсу, объект-заместитель можно передать в любой код, ожидающий сервисный объект.
  ***
  # Пример: 
  ```from functools import partial
 
 
class ImageBase(object):
    """Абстрактное изображение"""
    @classmethod
    def create(cls, width, height):
        """Создает изображение"""
        return cls(width, height)
 
    def draw(self, x, y, color):
        """Рисует точку заданным цветом"""
        raise NotImplementedError()
 
    def fill(self, color):
        """Заливка цветом"""
        raise NotImplementedError()
 
    def save(self, filename):
        """Сохраняет изображение в файл"""
        raise NotImplementedError()
 
 
class Image(ImageBase):
    """Изображение"""
    def __init__(self, width, height):
        self._width = int(width)
        self._height = int(height)
 
    def draw(self, x, y, color):
        print 'Рисуем точку; координаты: (%d, %d); цвет: %s' % (x, y, color)
 
    def fill(self, color):
        print 'Заливка цветом %s' % color
 
    def save(self, filename):
        print 'Сохраняем изображение в файл %s' % filename
 
 
class ImageProxy(ImageBase):
    """
    Заместитель изображения.
    Откладывает выполнение операций над изображением до момента его сохранения.
    """
    def __init__(self, *args, **kwargs):
        self._image = Image(*args, **kwargs)
        self.operations = []
 
    def draw(self, *args):
        func = partial(self._image.draw, *args)
        self.operations.append(func)
 
    def fill(self, *args):
        func = partial(self._image.fill, *args)
        self.operations.append(func)
 
    def save(self, filename):
        # выполняем все операции над изображением
        map(lambda f: f(), self.operations)
        # сохраняем изображение
        self._image.save(filename)
 
 
img = ImageProxy(200, 200)
img.fill('gray')
img.draw(0, 0, 'green')
img.draw(0, 1, 'green')
img.draw(1, 0, 'green')
img.draw(1, 1, 'green')
img.save('image.png')
 
# Заливка цветом gray
# Рисуем точку; координаты: (0, 0); цвет: green
# Рисуем точку; координаты: (0, 1); цвет: green
# Рисуем точку; координаты: (1, 0); цвет: green
# Рисуем точку; координаты: (1, 1); цвет: green
# Сохраняем изображение в файл image.png
```
 # Преимущества и недостатки

### Преимущества

   * Позволяет контролировать сервисный объект незаметно для клиента.
   * Может работать, даже если сервисный объект ещё не создан.
   *  Может контролировать жизненный цикл служебного объекта.

### Недостатки

   * Усложняет код программы из-за введения дополнительных классов.
   * Увеличивает время отклика от сервиса.
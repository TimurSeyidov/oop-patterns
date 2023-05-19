<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="__0"></a>ПАТТЕРНЫ ПРОЕКТИРОВАНИЯ</h1>
<hr>
<h2 class="code-line" data-line-start=2 data-line-end=3 ><a id="_2"></a>Авторы:</h2>
<p class="has-line-data" data-line-start="3" data-line-end="5">Ямалиева Карина Радисовна<br>
Шайхуллина Диля Марсельевна</p>
<h1 class="code-line" data-line-start=5 data-line-end=6 ><a id="__Flyweight_5"></a>Паттерн легковес (Flyweight)</h1>
<hr>
<p class="has-line-data" data-line-start="8" data-line-end="10"><strong>Легковес</strong> — это структурный паттерн, который экономит память, благодаря разделению общего состояния, вынесенного в один объект, между множеством объектов. Легковес позволяет экономить память, кешируя одинаковые данные, используемые в разных объектах.<br></p>
<p> Flyweight(Легковес)
<img src="https://camo.githubusercontent.com/0eaf0ee3e06687c3be25749ca5c140acf8185ae337d1e024f387cffa64d2c6a4/68747470733a2f2f7265666163746f72696e672e677572752f696d616765732f7061747465726e732f636f6e74656e742f666c797765696768742f666c797765696768742d32782e706e67"></p>
<hr>
<pre><code class="has-line-data" data-line-start="14" data-line-end="61">class ComplexCars(object):
    """Separate class for Complex Cars"""
 
    def __init__(self):
        pass
 
    def cars(self, car_name):
 
        return "ComplexPattern[% s]" % (car_name)
 
 
class CarFamilies(object):
    """dictionary to store ids of the car"""
 
    car_family = {}
    def __new__(cls, name, car_family_id):
        try:
            id = cls.car_family[car_family_id]
        except KeyError:
            id = object.__new__(cls)
            cls.car_family[car_family_id] = id
        return id
 
    def set_car_info(self, car_info):
        """set the car information"""
        cg = ComplexCars()
        self.car_info = cg.cars(car_info)
        
        
    def get_car_info(self):
        """return the car information"""
        return (self.car_info)
 
if __name__ == '__main__':
    car_data = (('a', 1, 'Audi'), ('a', 2, 'Ferrari'), ('b', 1, 'Audi'))
    car_family_objects = []
    for i in car_data:
        obj = CarFamilies(i[0], i[1])
        obj.set_car_info(i[2])
        car_family_objects.append(obj)
 
   """similar id's says that they are same objects """
 
    for i in car_family_objects:
        print("id = " + str(id(i)))
        print(i.get_car_info())
</code></pre>

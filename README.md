# Reto__4
By Juan Esteban Molina Rey (eljuanessoy)

### 1. Include the class exercise in the repo.

En la primera parte de este reto, se empleó el diagrama de clases proporcionado para identificar las diferentes medidas de ciertas figuras que comparten atributos y clases. El código inicial se centra en definir dos clases principales que sirven como base para todo lo demás: la clase Punto y la clase Línea, y ademas la clase Shape, la cual contiene todos los atributos y métodos necesarios para cada figura.

```python
import math

class Punto:
    def __init__(self, x, y):
        self._x = x
        self._y = y

    def set_x(self, value):
        self._x = value

    def set_y(self, value):
        self._y = value

    def get_x(self):
        return self._x

    def get_y(self):
        return self._y


class Line:
    def __init__(self, start, end):
        self._start = start
        self._end = end

    def set_start(self, value):
        self._start = value

    def set_end(self, value):
        self._end = value

    def get_start(self):
        return self._start

    def get_end(self):
        return self._end

    def compute_length(self):
        return math.sqrt((self._end.get_x() - self._start.get_x())**2 + (self._end.get_y() - self._start.get_y())**2)

class Shape:
    def __init__(self, is_regular):
        self._is_regular = is_regular
        self._vertices = []
        self._edges = []

    def set_is_regular(self, is_regular):
        self._is_regular = is_regular
    
    def get_is_regular(self):
        return self._is_regular
    
    def add_vertex(self, vertex):
        self._vertices.append(vertex)
    
    def get_vertices(self):
        return self._vertices.copy()
    
    def add_edge(self, edge):
        self._edges.append(edge)
    
    def get_edges(self):
        return self._edges.copy()

    def compute_area(self):
        pass

    def compute_perimeter(self):
        pass
```

En esta sección se desarrolla la clase Triangle como una derivación de la clase Shap.  

- Vértices: Se solicita al usuario ingresar las coordenadas de los vértices del triángulo. Dado que un triángulo tiene tres vértices, este proceso se realiza en un rango de 3. Para organizar las coordenadas, se utiliza la clase *Punto*, y cada vértice se agrega a la lista que almacena los vértices.  

- Aristas: Se definen las tres aristas del triángulo calculando las distancias entre los vértices mediante la clase Línea.  

- Área: Se utiliza el algoritmo de Herón, que permite calcular el área del triángulo basándose en las longitudes de sus tres aristas.  

- Perímetro: Se obtiene sumando las longitudes de todas las aristas incluidas en la lista.  

- Ángulos internos: Para determinar los ángulos internos del triángulo, se aplica la ley de los cosenos considerando las tres aristas. Primero, se calculan las longitudes de las aristas, luego se aplica la fórmula correspondiente, y finalmente, los resultados se convierten de radianes a grados. Todos los ángulos se almacenan en una lista.  

En la parte final, se crean subclases de la clase Triangle, representando los diferentes tipos de triángulos existentes. Estas subclases heredan de Triangle e incluyen información adicional sobre si el triángulo es un polígono regular o irregular.

```python
class Triangle(Shape):
    def __init__(self, is_regular):
        super().__init__(is_regular)
        
        for i in range(3):  
            x = float(input(f"Introduce la coordenada x del vértice {i+1}: "))
            y = float(input(f"Introduce la coordenada y del vértice {i+1}: "))
            vertex = Punto(x, y)
            self.add_vertex(vertex)
        
        for i in range(3):  
            start = self._vertices[i]
            end = self._vertices[(i+1)%3]
            edge = Line(start, end)
            self.add_edge(edge)
        
    def compute_area(self):
        a = self._edges[0].compute_length()
        b = self._edges[1].compute_length()
        c = self._edges[2].compute_length()
        s = (a + b + c) / 2
        area = math.sqrt(s * (s - a) * (s - b) * (s - c))
        return area
    
    def compute_perimeter(self):
        perimeter = sum(edge.compute_length() for edge in self._edges)
        return perimeter

    def compute_inner_angles(self):
        angles = []
        for i in range(3):
            a = self._edges[i].compute_length()
            b = self._edges[(i+1)%3].compute_length()
            c = self._edges[(i+2)%3].compute_length()
            angle = math.degrees(math.acos((b**2 + c**2 - a**2) / (2 * b * c)))
            angles.append(angle)
        return angles


class Isosceles(Triangle):
    def __init__(self):
        super().__init__(False)


class Equilateral(Triangle):
    def __init__(self):
        super().__init__(True)


class Scalene(Triangle):
    def __init__(self):
        super().__init__(False)


class TriRectangle(Triangle):
    def __init__(self):
        super().__init__(False)
```

A continuación, se desarrolló la clase Rectangle, siguiendo una lógica similar a la utilizada para la clase Triangle:

- Vértices y aristas: Se empleó el mismo enfoque para calcular estos elementos, con la diferencia de que el rango se extendió a 4, dado que un rectángulo tiene 4 vértices y 4 aristas.

- Área: El área se obtuvo multiplicando dos valores de la lista de aristas, correspondientes a la base y la altura del rectángulo.

- Perímetro: El perímetro se calculó sumando todos los valores de la lista de aristas.

- Ángulos internos: Dado que todos los rectángulos tienen 4 ángulos internos de 90 grados, simplemente se creó una lista que contiene estos valores.

A partir de la clase Rectangle, se creó la clase Square. El cuadrado hereda características de la clase Rectangle, compartiendo propiedades esenciales con esta.

Y finalmente se imprimen los resultados.

```python
class Rectangle(Shape):
    def __init__(self, is_regular):
        super().__init__(is_regular)
        
        for i in range(4):  
            x = float(input(f"Introduce la coordenada x del vértice {i+1}: "))
            y = float(input(f"Introduce la coordenada y del vértice {i+1}: "))
            vertex = Punto(x, y)
            self.add_vertex(vertex)
        
        for i in range(4):  
            start = self._vertices[i]
            end = self._vertices[(i+1)%4]
            edge = Line(start, end)
            self.add_edge(edge)
        
    def compute_area(self):
        a = self._edges[0].compute_length()
        b = self._edges[1].compute_length()
        area = a * b
        return area
    
    def compute_perimeter(self):
        perimeter = sum(edge.compute_length() for edge in self._edges)
        return perimeter

    def compute_inner_angles(self):
        angles = [90, 90, 90, 90]
        return angles


class Square(Rectangle):
    def __init__(self):
        super().__init__(True)

#Imprimir los resultados

triangle = Triangle(False)
area = triangle.compute_area()
print("El área del triángulo es:", area)

perimeter = triangle.compute_perimeter()
print("El perímetro del triángulo es:", perimeter)

if triangle.get_is_regular():
    print("El triángulo es regular")
else:
    print("El triángulo no es regular")

angles = triangle.compute_inner_angles()
print("Los ángulos internos del triángulo son:", angles)

lengths = [edge.compute_length() for edge in triangle.get_edges()]

if len(set(lengths)) == 1:
    print("Type: Equilateral")
elif len(set(lengths)) == 2:
    print("Type: Isosceles")
else:
    print("Type: Scalene")


rectangle = Rectangle(False)

area = rectangle.compute_area()
print("El área del rectángulo es:", area)

perimeter = rectangle.compute_perimeter()
print("El perímetro del rectángulo es:", perimeter)

angles = rectangle.compute_inner_angles()
print("Los ángulos internos del rectángulo son:", angles)
```

### 2. The restaurant revisted

- Add setters and getters to all subclasses for menu item
- Override calculate_total_price() according to the order composition (e.g if the order includes a main course apply some disccount on beverages)
- Add the class Payment() following the class example.

```python
class MenuItem:
    def __init__(self, nombre, precio, impuesto, propina):
        self.precio = precio
        self.impuesto = impuesto
        self.propina = propina
        self.nombre = nombre

    def calcularPrecio(self):
        impuesto = self.precio * self.impuesto
        propina = self.precio * self.propina
        total = self.precio + impuesto + propina
        return total
    
class Entrada(MenuItem):
    def __init__(self, nombre, precio, impuesto, propina, tipo):
        super().__init__(nombre, precio, impuesto, propina)
        self.tipo = tipo

    def get_tipo(self):
        return self.tipo

    def set_tipo(self, tipo):
        self.tipo = tipo

class Postre(MenuItem):
    def __init__(self, nombre, precio, impuesto, propina, tipo):
        super().__init__(nombre, precio, impuesto, propina)
        self.tipo = tipo

    def get_tipo(self):
        return self.tipo

    def set_tipo(self, tipo):
        self.tipo = tipo

class Bebida(MenuItem):
    def __init__(self, nombre, precio, impuesto, propina, tipo):
        super().__init__(nombre, precio, impuesto, propina)
        self.tipo = tipo

    def get_tipo(self):
        return self.tipo

    def set_tipo(self, tipo):
        self.tipo = tipo

class Ensalada(MenuItem):
    def __init__(self, nombre, precio, impuesto, propina, vinagreta):
        super().__init__(nombre, precio, impuesto, propina)
        self.vinagreta = vinagreta

    def get_vinagreta(self):
        return self.vinagreta

    def set_vinagreta(self, vinagreta):
        self.vinagreta = vinagreta

class Fuerte(MenuItem):
    def __init__(self, nombre, precio, impuesto, propina, tipo):
        super().__init__(nombre, precio, impuesto, propina)
        self.tipo_pasta = tipo

    def get_tipo(self):
        return self.tipo_pasta

    def set_tipo(self, tipo):
        self.tipo = tipo

class Order:
    def __init__(self):
        self.items = []
    
    def agregarItem(self, item):   #Esta parte nos permite agregar los items que se deseen
        self.items.append(item)       


    def calcularTotal(self):
        main_course_ordered = any(isinstance(item, Fuerte) for item in self.items)
        total = 0
        for item in self.items:
            if isinstance(item, Bebida) and main_course_ordered:
                total += item.calcularPrecio() * 0.9  # Apply a 10% discount
            else:
                total += item.calcularPrecio()
        return total

    def imprimirFactura(self):
        for item in self.items:
            print(item.nombre, "- $",item.calcularPrecio())  #Imprime nombre y precio del Item
        print("Total: $",self.calcularTotal())
    
    def aplicarDescuento(self):         #Aplica descuento del 10% con condición
        total = self.calcularTotal()
        if total > 450000:
            for item in self.items:
                item.precio = item.precio * 0.9
    
class Payment:
    def __init__(self, total, method):
        self.total = total
        self.method = method

    def get_total(self):
        return self.total

    def set_total(self, total):
        self.total = total

    def get_method(self):
        return self.method

    def set_method(self, method):
        self.method = method

    def pay(self):
        pass

class Cash(Payment):
    def __init__(self, total, method, cash_received):
        super().__init__(total, method)
        self.cash_received = cash_received

    def get_cash_received(self):
        return self.cash_received

    def set_cash_received(self, cash_received):
        self.cash_received = cash_received

    def pay(self):
        if self.cash_received >= self.total:
            return self.cash_received - self.total
        else:
            return None

class Card(Payment):
    def __init__(self, total, method, card_number, expiration_date, cvv):
        super().__init__(total, method)
        self.card_number = card_number
        self.expiration_date = expiration_date
        self.cvv = cvv

    def get_card_number(self):
        return self.card_number

    def set_card_number(self, card_number):
        self.card_number = card_number

    def get_expiration_date(self):
        return self.expiration_date

    def set_expiration_date(self, expiration_date):
        self.expiration_date = expiration_date

    def get_cvv(self):
        return self.cvv

    def set_cvv(self, cvv):
        self.cvv = cvv

    def pay(self):
        return True
  

#Creamos el menu con cada item y sus atributos

entrada1 = Entrada("Cacerola de Patatas Bravas", 56800, 0.7, 0.10, "Queso")
entrada2 = Entrada("Ración de Jamón Ibérico de Bellota", 223500, 0.7, 0.10, "Carne")
entrada3 = Entrada("Carpaccio de Salmón", 70000, 0.7, 0.10, "Pescado")

fuerte1 = Fuerte("Spaguetti a la Fiorentina", 44600, 0.7, 0.10, "Spaghetti")
fuerte2 = Fuerte("Pasta Pomodoro", 70000, 0.7, 0.10, "Pasta corta")
fuerte3 = Fuerte("Spaguetti al Guanciale", 75000, 0.7, 0.10, "Spaghetti")
fuerte4 = Fuerte("Pasta a la Rueda con Prosciutto y manzana confitada", 85000, 0.7, 0.10, "Fetuccini")
fuerte5 = Fuerte("Paella Mixta", 149800, 0.7, 0.10, "Paella")

postre1 = Postre('Torta Philadelphia', 29800, 0.7, 0.10, "Tarta")
postre2 = Postre("Copa Amaretto", 26000, 0.7, 0.10, "Copa")
postre3 = Postre("Gelato al Pistacchio", 40000, 0.7, 0.10, "Gelatina")
postre4 = Postre("Afogatto", 26000, 0.7, 0.10, "Helado")

bebida1 = Bebida("Bubble Fizz", 50000, 0.7, 0.10, "Licor")
bebida2 = Bebida("Capuccino", 10000, 0.7, 0.10, "Bebida caliente")
bebida3 = Bebida("Gassata de Frutos Verdes", 18900, 0.7, 0.10, 'Refresco')
bebida4 = Bebida("Limonada de Mango Biche", 16500, 0.7, 0.10, "Limonada")
bebida5 = Bebida("Jugo de Mora", 10000, 0.7, 0.10, "Jugo")

ensalada1 = Ensalada("Insalata Di garedino", 55000, 0.7, 0.10, "Vinagreta Oliva Limón")
ensalada2 = Ensalada("Vero Amore", 51000, 0.7, 0.10, "Vinagreta tradicional")



# Crea una nueva orden
orden1 = Order()
orden1.agregarItem(ensalada2)
orden1.agregarItem(fuerte3)
orden1.agregarItem(postre3)
orden1.agregarItem(bebida4)

orden2 = Order()
orden2.agregarItem(entrada2)
orden2.agregarItem(fuerte5)
orden2.agregarItem(postre1)
orden2.agregarItem(bebida3)

orden3 = Order()
orden3.agregarItem(entrada1)
orden3.agregarItem(fuerte1)
orden3.agregarItem(postre4)
orden3.agregarItem(bebida3)

# Imprime el menú con su descuento si existe
orden = int(input("Ingrese una orden, 1, 2, 3: "))

if orden == 1:
    print()
    print("La factura de la orden 1 es:")
    print()
    orden1.aplicarDescuento()
    orden1.imprimirFactura()
    print()

elif orden == 2:
    print()
    print("La factura de la orden 2 es:")
    print()
    orden2.aplicarDescuento()
    orden2.imprimirFactura()
    print()

elif orden == 3:
    print()
    print("La factura de la orden 3 es:")
    print()
    orden3.aplicarDescuento()
    orden3.imprimirFactura()
    print()

else:
    print("Ingrese un método válido")

metodo = int(input("Elige un método de pago (1 si es con efectivo, 2 si es con tarjeta): "))

if metodo == 1:
    total = orden1.calcularTotal()
    cash_received = float(input("Ingrese el monto: "))
    cash = Cash(total, "Efectivo", cash_received)
    cambio = cash.pay()
    if cambio:
        print("El cambio es de $", cambio)
    else:
        print("El monto recibido no es suficiente")

if metodo == 2:
    total = orden1.calcularTotal()
    card_number = input("Ingrese el número de la tarjeta: ")
    expiration_date = input("Ingrese la fecha de expiración: ")
    cvv = input("Ingrese el código de seguridad: ")
    card = Card(total, "Tarjeta", card_number, expiration_date, cvv)
    if card.pay():
        print("Pago exitoso")
    else:
        print("Pago rechazado")

elif metodo != 1 and metodo != 2:
    print("Método de pago no válido")
```

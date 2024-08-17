import  math

print('Задание "Они все так похожи"')

class Figure: # родительский
    # Каждый объект класса Figure должен обладать следующими атрибутами:
    # Атрибуты(инкапсулированные): __sides(список сторон (целые числа)), __color(список цветов в формате RGB)
    # Атрибуты(публичные): filled(закрашенный, bool)

    sides_count = 0
    __sides = []

    def __init__(self, color=(0, 0, 0), *sides ):
        self.__sides = list(sides) if self.__is_valid_sides(*sides) else [1] * self.sides_count # Нужен именно список
        self.sides_count = len(sides)
        self.__color = list(color) if self.__is_valid_color(*color) else [0, 0, 0]
        self.filled = self.__color != [0, 0, 0]



    def get_color (self):
        return self.__color

    def __is_valid_color(self, r, g, b ): # валидны цвета до 255 каждый компанент
        return all(isinstance(i, int) and 0 <= i <= 255 for i in (r, g, b))

    def set_color(self, r, g, b ):
        #print(self.__is_valid_color(r, g, b))
        if self.__is_valid_color(r, g, b) :
            self.__color = [r, g, b]
            #print(self.__color)
        else:
            # подумать над обработкой исключения недозволительных цветов
            print("Не прописан цвет ", r, g, b, "остался ", self.__color)
            pass


    def __is_valid_sides(self,  *sides  ): # передаём стороны в параметре *args
        return all(isinstance(s, int) and s > 0 for s in sides) and len(sides) == self.sides_count


    def get_sides (self):
       return list (self.__sides)

    def __len__ (self): # Перезагрузка метода __len__ возвращает периметр фигуры.
        return sum(self.__sides)

    def set_sides(self, *new_sides):
        if self.__is_valid_sides(*new_sides):
            self.__sides = list(new_sides)



class Circle(Figure):
    sides_count = 1
    #__radius = 0

    def __init__(self, color=(0, 0, 0), *sides):
        super().__init__(color, *sides)
        # Если количество сторон не соответствует ожиданиям, устанавливаем значение по умолчанию
        if len(self.get_sides()) != self.sides_count:
            self.set_sides(1)
        # Расчет радиуса: Радиус = Длина_окружности / (2 * 3,14)
        # Атрибут __radius, рассчитывается исходя из длины окружности (одной единственной стороны).
        self.__radius = self.get_sides()[0] / (2 * math.pi)


    #    self.__sides = list(sides)
    #    self.sides_count = 1
    #    self.__color = color
    #    self.filled = filled

    def __get_square (self):
        return self.__radius * self.__radius * math.pi


class Triangle(Figure):
#    __sides = []
    sides_count = 3
    #__radius = 0

    def __init__(self, sides = (),   color = () ): # Хищим конструктор базового класса Фигура
        super().__init__(sides,   color)

    def get_square (self):
        p = self.__len__() / 2 # полупериметр
        s = p
        for side in self.get_sides():
            s = s * (p - side)
            #print( side, p, s )
        return math.sqrt(s)


class Cube(Figure):
    sides_count = 12

    def __init__(self, color=(0, 0, 0), *sides):
        if len(sides) == 1:
            sides = sides * self.sides_count
        super().__init__(color, *sides)
        # Если количество сторон не соответствует ожиданиям, устанавливаем значение по умолчанию
        if len(self.get_sides()) != self.sides_count:
            self.set_sides(*([1] * self.sides_count))


    def get_volume(self):
        side_length = self.get_sides()[0]
        # Метод get_volume, возвращает объём куба.
        return side_length ** 3




# void main(void)

# f = Figure(  )
#tr= Triangle ((3,4,5),   (0, 255, 255) )

circle1 = Circle((200, 200, 100), 10) # (Цвет, стороны)
cube1 = Cube((222, 35, 130), 6)

# Проверка на изменение цветов:
circle1.set_color(55, 66, 77) # Изменится
print(circle1.get_color())
cube1.set_color(300, 70, 15) # Не изменится
print(cube1.get_color())

# Проверка на изменение сторон:
cube1.set_sides(5, 3, 12, 4, 5) # Не изменится
print(cube1.get_sides())
circle1.set_sides(15) # Изменится
print(circle1.get_sides())

# Проверка периметра (круга), это и есть длина:
print(len(circle1))

# Проверка объёма (куба):
print(cube1.get_volume())

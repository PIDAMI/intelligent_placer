# intelligent placer
## Постановка задачи
Требуется по заданной фотографии нескольких предметов на светлой горизонтальной поверхности и многоугольника, определить, можно ли каким-либо способом расположить одновременно все предметы в многоугольник. Все предметы, которые могут оказаться на фотографии, заранее известны.


## Формат входных и выходных данных
На вход программа получает изображение в формате jpg, на котором изображен многоугольник и проверяемые предметы.

В результате работы программа печатает в консоль **True**, если предметы можно уместить в многоугольник, и **False**, если способа расположить предметы нет или же изображение и его содержание не подходит под критерии, перечисленные далее.

## Требования к предметам
+ предметы рассматриваются только из заранее известного набора, в котором они расположены на белом листе бумаги А4
+ не перекрываются 
+ отделены от границ изображения
+ отделены от многоугольника
+ каждый предмет встречается на фотографии максимум 1 раз
+ поверхность, на которой изображены предметы и многоугольник, должна быть светлой и горизонтальной; предметы хорошо отличимы на ее фоне 
## Требования к многоугольнику
+ задан как фигура, нарисованная темным маркером на белом листе бумаги, сфотографированная вместе с предметами
+ края листа бумаги, на котором изображен многоугольник, отделен от границ изображения
+ выпуклый
+ не больше 5 вершин
+ расположен по одну сторону от всех предметов (например, предметы не могут располагаться над и под многоугольником одновременно)

## Требования к изображению
+ угол между направлением камеры и перпендикуляром к поверхности(направленный на камеру) должен быть не более 10 градусов
+ цветные
+ формата jpg

# Исходные предметы и тестовые данные
[Изображения исходных предметов и поверхности](data/items_images)

[Примеры входных данных](data/test_images)

## План решения
Общий ход алгоритма можно разбить на несколько отдельных шагов.

## Поиск многоугольника и объектов
Используя тот факт, что поверхность светлая, многоугольник нарисован черным маркером, а объекты отличимы на фоне поверхности, можно выделить границы многоугольника и объектов с помощью различных методов локальной и глобальной бинаризации, предварительно обработав изображение морфологическими операциями.


Так как объекты отделены как друг от друга, так и от многоугольника, то после проведения бинаризации изображение можно разбить на компоненты связности и далее работать с каждой компонентой отдельно.


Чтобы найти среди всех компонент связности ту, которая соответствует многоугольнику, можно воспользоваться тем фактом, что, по условию, многоугольник нарисован на белом листе бумаги А4: посчитать, например, среднее значение цвета пикселей в каждой компоненте. Можно также посчитать отношение ширины к высоте каждой компоненты - у листа формат А4 и мы заранее знаем примерное значение этого отношения для него.

## Проверка на соответствие к требованиям
Вершины многоугольника можно найти как особые точки. Далее по этим вершинам можно построить выпуклую оболочку и с помощью нее проверить, что исходный многоугольник выпуклый(например, посчитать площади - они должны быть примерно равны, если площадь выпуклой оболочки много больше, то многоугольник не выпуклый).

Далее требуется проверить, что объекты с изображения принадлежат множеству заранее известного набора объектов - это можно сделать с помощью поиска особых точек (SIFT/SURF) заранее известных объектов и объектов на изображении и попарном сравнении этих особых точек

# Сканирующее окно по массивам NumPy

![Under construction](../data/2019.09.25-under-construction-icon.png)
**Under construction...**

[Опубликовано на Хабре]()

[CoLab блокнот](https://colab.research.google.com/drive/1Zru_-zzbtylgitbwxbi0eDBNhwr8qYl6)

Возможно сделать [скользящее окно](https://wiki.loginom.ru/articles/windowing-method.html) (rolling window, [sliding window](https://stackoverflow.com/questions/8269916/what-is-sliding-window-algorithm-examples), moving window) по массивам NumPy на языке программирования Python **без явных циклов**. В данной статье рассматривается создание одно-, двух-, трех- и N-мерных скользящих окон по массивам NumPy. В результате скорость обработки данных увеличивается в несколько тысяч раз и сравнима по скорости с языком программирования **С**.

Cкользящее окно применяется в: обработке изображений, искусственных нейронных сетях, интернет протоколе TCP, обработке геномных данных, прогнозировании временных рядов и т.д.

**Отказ от ответственности**: *в исходном коде могут быть ошибки!* Если вы видите ошибку, пожалуйста, напишите мне.

  * [Введение](#introduction)
  * [Скользящее 1D окно по ND массиву в Numpy](#1d)
  * [Скользящее 2D окно по ND массиву в Numpy](#2d)
  * [Скользящее 3D окно по ND массиву в Numpy](#3d)
  * [Скользящее MD окно по ND массиву, где M ≤ N](#md)
  * [Скользящее MD окно по ND массиву для любых M и N](#md-extended)

---
## <a name="introduction">Введение</a>
Эта статья является продолжением [моего ответа](https://stackoverflow.com/a/46237736/7550928) на сайте StackOverflow. Мои первые эксперименты со скользящим окном [здесь](https://github.com/foobar167/junkyard/blob/master/rolling_window.py) и [здесь](https://github.com/foobar167/junkyard/blob/master/rolling_window_advanced.py).

**Практическая реализация** скользящего двумерного окна по двумерному массиву изображения находится в функции `roll` файла [`logic_tools.py`](https://github.com/foobar167/junkyard/blob/master/manual_image_annotation1/polygon/logic_tools.py) проекта [Ручная разметка изображений с помощью многоугольников](https://github.com/foobar167/junkyard/tree/master/manual_image_annotation1).

Алгоритмы для одномерного скользящего окна уже реализованы [здесь](https://stackoverflow.com/a/7100681/7550928), [здесь](https://rigtorp.se/2011/01/01/rolling-statistics-numpy.html) и [здесь](https://stackoverflow.com/questions/6811183/rolling-window-for-1d-arrays-in-numpy).

Для понимания темы, вам необходимо знать, что такое [страйды (strides, шаги)](https://stackoverflow.com/a/53099870/7550928).

Какое-то скользящее окно реализовано [в библиотеке Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rolling.html), однако автор плохо знаком с библиотекой Pandas, чтобы сказать, насколько универсальная данная там реализация. К тому же всегда интереснее реализовать функционал самому, если есть такая возможность. Как альтернативу, можно задействовать библиотеку [Cython](https://cython.org/), однако скорость обработки все-равно будет ниже.

---
## <a name="1d" />1. Скользящее 1D окно по ND массиву в Numpy
Простейшее одномерное скользящее окно по многомерному массиву выглядит так:
```python
# Rolling 1D window for ND array
def roll(a,      # ND array
         b,      # rolling 1D window array
         dx=1):  # step size (horizontal)
    shape = a.shape[:-1] + (int((a.shape[-1] - b.shape[-1]) / dx) + 1,) + b.shape
    strides = a.strides[:-1] + (a.strides[-1] * dx,) + a.strides[-1:]
    return np.lib.stride_tricks.as_strided(a, shape=shape, strides=strides)
```
Функция [`numpy.lib.stride_tricks.as_strided`](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.lib.stride_tricks.as_strided.html) создает новый вид (view) массива с заданной формой (shape) и шагами (strides).

Форма (shape) нового массива создается из формы входного многомерного массива, по которому осуществляется движение сканирующего окна, и формы одномерного сканирующего окна. Тогда как шаги (strides) создаются только из шагов входного многомерного массива без участия одномерного сканирующего окна.

Форма (shape) состоит из трех слагаемых:
  * `a.shape[:-1]` — это остаток от формы ND-массива, где `N > 1`. Если `N == 1`, то этот остаток будет равен пустому [кортежу](https://pythonworld.ru/tipy-dannyx-v-python/kortezhi-tuple.html) `t == ()`, таким образом это слагаемое не важно для `N == 1`.
  * `(int((a.shape[-1] - b.shape[-1]) / dx) + 1,)` — это количество шагов скользящего окна по последней размерности `[-1]` массива. Шаг сканирующего окна `dx` может быть равным: 1, 2, 3 и т.д.
  * `b.shape` — это форма скользящего окна.

Шаги (strides) также состоят из трех сладаемых:
  * `a.strides[:-1]` — это остаток от шагов ND-массива, где `N > 1`. Если `N == 1`, то этот остаток будет равен пустому кортежу `t == ()`, таким образом это слагаемое не важно для `N == 1`.
  * `(a.strides[-1] * dx,)` — это количество байт между шагами скользящего окна. Например, целочисленный `int` массив имеет 4 байта в одном шаге между соседними элементами, таким образом для шага `dx == 2` шаг в байтах составит `4 * 2 = 8` байт.
  * `a.strides[-1:]` — это шаг в байтах между соседними элементами. Например, целочисленный `int` массив имеет шаг 4 байта между соседними элементами, тогда кортеж равен `(4,)`.

---
## <a name="2d" />2. Скользящее 2D окно по ND массиву в Numpy

---
## <a name="3d" />3. Скользящее 3D окно по ND массиву в Numpy

---
## <a name="md" />4. Скользящее MD окно по ND массиву, где M ≤ N

---
## <a name="md-extended" />5. Скользящее MD окно по ND массиву для любых M и N

Спасибо за внимание!

**Теги:** Python, NumPy, метод скользящего окна, rolling window method, массивы, алгоритмы, оптимизация кода, программирование,

**Хабы:** Python, NumPy, алгоритмы, программирование,

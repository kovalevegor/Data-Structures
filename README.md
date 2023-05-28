# Лабораторная 7 | Дерево Отрезков

#### 1. Запишите комментарии, объясните работу обоих способов построения интервала для метода ```accumulate```

### Построение интервала рекурсивным методом
```cpp
T seg_tree::acumulate(int l, int r) {
    return rec_accum(1, 0, 1 << k, l, r);
}
```
+ ```acumulate``` - метод, который выполняет операцию накопления (accumulate) на диапазоне от ```l``` до ```r```.
+ ```rec_accum``` - вспомогательная рекурсивная функция, которая выполняет накопление на поддереве с корнем в вершине ```v``` и диапазоне от ```vl``` до ```vr```.
+ Возвращает результат накопления.
```cpp
T seg_tree::rec_accum(int v, int vl, int vr, int l, int r) {
    if (l == vl && r == vr) return data[v];
```
+ Если заданный диапазон ```[l, r)``` полностью соответствует текущему узлу ```[vl, vr)```, то возвращается значение ```data[v]``` (подсчитанное значение в текущем узле).
```cpp
int c = (vl + vr) / 2;
if (r <= c) return rec_accum(v * 2, vl, c, l, r);
if (c <= l) return rec_accum(v * 2 + 1, c, vr, l, r);
```
+ Вычисляется середина поддерева как ```c = (vl + vr) / 2```.
+ Если правая граница ```r``` находится до середины ```c```, рекурсивно вызывается ```rec_accum``` для левого поддерева ```v * 2``` с диапазоном ```[vl, c)``` и возвращается результат.
+ Если левая граница ```l``` находится после или на равной позиции с серединой c, рекурсивно вызывается ```rec_accum``` для правого поддерева ```v * 2 + 1``` с диапазоном ```[c, vr)``` и возвращается результат.
```cpp
return op(rec_accum(v * 2, vl, c, l, c), rec_accum(v * 2 + 1, c, vr, c, r));
```
+ Если диапазон ```[l, r)``` перекрывает середину ```c```, выполняется операция ```op``` (которая должна быть определена) над результатами рекурсивного вызова ```rec_accum``` для левого поддерева ```v * 2``` с диапазоном ```[vl, c)``` и правого поддерева ```v * 2 + 1``` с диапазоном ```[c, vr)```.
---
### Процедура построения интервала подъемом по дереву
```cpp
T seg_tree::acumulate(int l, int r) {
    T result = identity_element;
```
+ Инициализация результата значением нулевым нейтральным элементом
```cpp
    l += 1 << k; r += 1 << k;
```
+ Изменение границ интервала, добавляя ```2^k```, для того, чтобы от номеров вершин в исходной последовательности, которые с нуля начинались, перейти к номерам в дереве. После выполнения этиъ действий, ```l``` и ```r``` - это номера в дереве
```cpp
    for (; l < r; l /= 2, r /= 2) {
```
+ Построение интервала подъемом по дереву. Пока левая и правая границы интервала не совпадут
```cpp
        if (l & 1)
            result = op(result, data[l++]);
```
+ Если левая граница нечетная, добавляем значение в результат и увеличиваем ее на 1
```cpp
        if (r & 2)
            result = op(result, data[--r]);
    }
```
+ Если правая граница четная, добавляем значение в результат и уменьшаем ее на 1
```cpp
    return result;
}
```
+ Возвращение результата
---
#### 2. Приведите пример интервала, который можно собрать ровно из ```2r-4``` ранее подсчитанных интервалов, где ```r``` - количество слоев в дереве. 
```

                         136
                /                     \
           58                              78
       /         \                   /          \
   25              33            45               33
 /    \          /    \        /    \           /    \
 9     16     17      16    21      24       9       24
/ \    / \    / \    / \    / \     / \     / \     /  \
4  5  6  10  7  10  8   9  10  11  12  12  6   3   12  12

```

Таким интрвалом является интервал листьев без первого и последнего `[1;15)`

Он составлен из следующих шестри ранее подсчитанных интервалов: 

`33 | 45 | 16 | 9 | 5 | 12`

---

# Конспект

### Дерево отрезков

`Дерево отрезков (Segment Tree)` - это структура данных, представляющая пример полное бинарное дерево, которое позволяет эффективно решать множество задач на отрезках в массиве или последовательности. Она представляет собой бинарное дерево, где каждый узел хранит информацию о некотором отрезке данных.

В дереве отрезков каждый узел представляет отрезок массива или последовательности и содержит информацию о свойствах этого отрезка (например, сумму элементов, минимальный или максимальный элемент и т.д.). Корень дерева отрезков соответствует всему массиву или последовательности, а листья представляют отдельные элементы.

---

### Хранение дерева

При построении дерева отрезков размер массива, используемого для хранения, обычно выбирается таким образом, чтобы было достаточно места для хранения всех узлов дерева отрезков. Для полного бинарного дерева с высотой $h (где h - целое число)$ общее количество узлов равно $2^(h+1) - 1$. Таким образом, массив для хранения дерева отрезков должен иметь размер $2^(h+1) - 1$.

Каждый узел дерева отрезков хранит информацию о своем отрезке данных. Обычно это представляется в виде структуры или класса, где каждый узел содержит следующую информацию:

+ Начало и конец отрезка, которые узел представляет.
+ Значение или агрегированная информация для этого отрезка.
+ Указатели на левого и правого потомка.

`Узлы дерева отрезков заполняются значениями от листьев к корню`. Листья соответствуют элементам исходного массива или последовательности. Значения в листьях могут быть непосредственно взяты из исходного массива или вычислены с помощью заданной функции или операции. `Нумерация узлов ведется от корня к листьям, начиная с 1`.

Затем каждый внутренний узел получает свою `агрегированную информацию`, которая зависит от значений его потомков. Обычно это делается с помощью комбинирования значений потомков, используя операцию, такую как сумма, минимум, максимум и т.д.

---

### Бинарные операции для дерева отрезков

В дереве отрезков используются различные бинарные операции (далее будем обозначать некоторую операцию как $\bigodot$) для выполнения запросов на отрезках данных. Вот некоторые из наиболее распространенных бинарных операций, которые могут быть применены в дереве отрезков:

1. `Сумма (Sum)`: Бинарная операция, которая возвращает сумму элементов на заданном отрезке.
2. `Минимум (Min)`: Бинарная операция, которая возвращает минимальный элемент на заданном отрезке.
3. `Максимум (Max)`: Бинарная операция, которая возвращает максимальный элемент на заданном отрезке.
4. `Произведение (Product)`: Бинарная операция, которая возвращает произведение элементов на заданном отрезке.
5. `НОД (GCD)`: Бинарная операция, которая возвращает наибольший общий делитель элементов на заданном отрезке.
6. `НОК (LCM)`: Бинарная операция, которая возвращает наименьшее общее кратное элементов на заданном отрезке.
7. `Конкатенация (Concatenation)`: Бинарная операция, которая объединяет элементы на заданном отрезке в одну строку или последовательность.
8. `Битовые операции (Bitwise Operations)`: Бинарные операции, такие как побитовое И (AND), побитовое ИЛИ (OR), побитовое исключающее ИЛИ (XOR), которые применяются к битовым представлениям элементов на заданном отрезке.

+ Обязательным свойством $\bigodot$ должна быть ассоциативность, то есть она не должна зависеть от порядка применения скобочек.

$$
(a \bigodot b) \bigodot c = a \bigodot (b \bigodot c) = a \bigodot b \bigodot c
$$

+ Для упрощения реализации будем считать, что операция $\bigodot$ является также коммутативной, хотя это условие не является обязательным

$$
a \bigodot b = b \bigodot a
$$

+ Кроме того, будем считать, что существует нейтарльный (единичный) элемент $e$ относительно операции $\bigodot$ такой, что для любого значения $x$ выполняется равенстов $ x \bigodot e = e \bigodot x = x$ Это условие также не является обязательным, но упрощает реализацию.

+ Нейтральными элементами для бинарных операций будут: $0, 1, \infty, -\infty$

---

### Принципы хранения значений в дереве отрезков

Дерево отрезков хранит значения в специальной структуре данных, которая позволяет эффективно выполнять операции над отрезками. Основные принципы хранения значений в дереве отрезков включают:

1. `Деление отрезков`: Дерево отрезков разбивает исходный массив или отрезок на более мелкие подотрезки. Каждый узел дерева отрезков представляет отрезок массива, исходный отрезок разбивается на два равных подотрезка, которые становятся дочерними узлами текущего узла.

2. `Сохранение значений`: Каждый узел дерева отрезков содержит информацию о некотором применении операции $\bigodot$ на соответствующем отрезке. Обычно это сумма или результат другой бинарной операции над элементами на отрезке. `В листьях дерева хранятся исходные значения`. Если исходных значений меньше, чем листьев, то пустые листья заполняются `нейтральными элементами`.

3. Корень узла ъранит результат применения операции $\bigodot$ ко всем значениям.

4. В общем случае, дерево отрезков хранит результат применения операции $\bigodot$ ко всем отрезкам $[l;r) таким, что r-l=2^m и l mod 2^m = 0$
































# SRC 

[Хранение информации в дереве отрезков](https://youtu.be/afRI2x04Pjk)

[Алгоритмы дерева отрезков](https://youtu.be/EGbpBUIfU-k)

[Основные операции над деревом отрезков](https://youtu.be/O9UlDA17P98)

[Использование дерева отрезков](https://youtu.be/JP0e__v1dV0)

[Рекомендации по выполнению заданий](https://youtu.be/6sTL5Z086iU)

[Тренировка по теме "Дерево отрезков"](http://olymp.isu.ru/cgi-bin/new-client?contest_id=67)

# Лекция 8 | Дерево отрезков | Групповые операции | Конспект 

### Другие варианты реализации дерева отрезков

#### Дерево без нейтральных элементов

+ В дереве $n$ элементов в полуоткрытом диапазоне $[0; n)$
+ Корень дерева контролирует весь диапазон $[0; n)$
+ Если некоторый узел контролирует диапазон $[l; r)$, состоящий более чем из одного элемента, то левый наследник будет контролировать диапазон $[l; c)$, а правый - $[c; r)$, где $c = (l + r) / 2$

--- 
	
Особенности реализации:

+ Для реализации не требуется нейтральный элемент и коммутативность операции
+ Нет очевидной зависимости между номером элемента и номером узла в дереве
+ Некоторые узлы не используются, к ним нет обращений, но они занимают память
+ Все методы должны быть реализованы через спуск от корня 
+ Реализация методов спуска совпадает с ранее рассмотренной, за исключением вызова для корня дерева
+ Нельзя добавить элементы в конец последовательности со сложностью $O (log n)$

---

### Дерево с экономией памяти

+ В дереве n элементов в полуоткрытом диапазоне $[0; n)$
+ Элементы с номером $i$ будет храниться в узле дерева с номером $n+i$
+ Для реализации достаточно $2\times n$ узлов дерева
+ Если дерево содержит узлы с номерами $2\times k$ и $2k+1$, то оно содержит и узел с номером $k$, который контролирует объединение диапазонов дочерних узлов 
 

Особенности реализации:

+ Нейтральный элемент и коммутативность операции могут упростить реализацию
+ Нет очевидной зависимости между номером узла в дереве и контролируемым диапазоном
+ Некоторые узлы не используются, но они занимают память
+ У структуры может не быть корня, фактически она является не деревом, а наоборот деревьев
+ Все методы должны быть реализованы через подъем от листьев
+ Реализация методов подъема совпадает с ранее рассмотренной, за исключением формулы для поиска номера узла дерева по номеру элемента
+ Нельзя добавить элементы в конец последовательности со сложностью $O(log n)$



### Групповые отложенные операции

+ Пусть задана некоторая операция, связанная с изменением значения элемента последовательности, например, присваивание нового значения, инкремент и декремент, прибавление или вычитание константы и другие. 
+ Задан диапазон элементов, причём указанная операция должна быть применена ко всем элементам диапазона, например, увеличить все элементы в диапазоне $[11; 92)$ на $10$
+ Теперь дерево отрезков, помимо запросов на вычисление, рассмотренных ранее, должно уметь выполнять и запросы на изменение элементов диапазона со сложностью $O(log n)$, не зависящей от длины диапазона 

---

+ При поступлении запроса на групповое изменение, операция не выполняется немедленно, а откладывается на будущее 
+ При этом, информация о необходимости выполнения этой операции запоминается
+ Информация об отложенных операциях должна быть структурирована, поскольку в противном случае придётся тратить время на поиск того, какие операции требуется произвести
+ В дереве отрезков отложенные операции связаны с узлами дерева и контролируемыми ими диапазонами 
+ Каждая новая операция должна перекрывать предыдущую так, чтобы в каждом узле хранилась информация только об одной операции. Например, если для диапазона $[16; 32)$ требовалось выполнить увеличение всех значений на 10 и, далее поступил запрос на увеличение элементов этого же диапазона на 5, то будет запомнено, что элементы диапазона необходимо в дальнейшем увеличить на 15
 
 
### Подход к реализации. Метод compose

+ Узел дерева должен содержать информацию о том, содержит ли он отложенную операцию. Далее будем предполагать, что объекты, хранимые в дереве, содержит метод `T::pending()`, возвращающий истину, если узел дерева содержит операцию, которая ожидает обработки
+ Узел дерева типа `Т` должен содержать метод для выполнения композиции двух операций `T::compose(const T &other)`
+ Метод compose в случае необходимости должен пересчитывать значения других полей в узле дерева. Например, если узел контролирующий диапазон $[16, 32)$ в дереве отрезков для операции суммирования получит запрос на увеличение всех элементов диапазона на $10$, то значение суммы в узле должно увеличиться на $10*(32 – 16) = 160$
+ Кроме того, каждый узел дерева должен уметь выполнять метод `complete()` для очистки отложенной операции. Этот метод будет применяться после того, как операция выполнена



### Проталкивание групповой операции. Метод push(рекурсия)

+ Все функции в дереве отрезков с групповыми операциями должны быть реализованы методам спуска от корня дерева 
+ В случае необходимости дальнейшего спуска по дереву, перед рекурсивным вызовом требуется выполнить проталкивание групповой операции вниз. `Это необходимо делать при реализации всех функций дерево отрезков`
+ Проталкивание осуществляется в соответствии со следующим всегда кодом

```cpp
if (текущий узел содержит отложенную операцию) {
    выполнить композицию для
        левого дочернего узла с текущим 
    выполнить композицию для 
        правого дочернего узла с текущим
    очистить отложенную операцию в текущем узле
}

```

---

3.	Пример реализации узла дерева

•	Класс node
 

 
•	Реализация дерева

	Для реализации можно выбрать вариант без нейтральных элементов, так как нерекурсивные методы подъёма от листьев всё равно использовать нельзя 
	Единственным параметром шаблона будет класс узла дерева. Операции над диапазоном элементов в нём уже заложены. Он также будет использоваться как тип значений последовательности
 
	Ранее мы находили элемент а потом присваивали его. Теперь удобнее выполнять пересчёт вместе хранения

 

	Проталкивания можно реализовать как закрытый метод Push.

 
	Все методы спуска от корня (кроме построения дерева) должны содержать проталкивания групповой операции перед переходом к дочерним узлам 
	Реализация групповой операции будет похожа на реализацию accumulate, но без возвращаемого значения. Занесение групповой операции выполняется с помощью метода compose. Также потребуется проталкивание и пересчет узлов после изменения

4.	Операции поиска элемента в дереве 

•	Поиск по номеру

	Логично реализовать операцию в виде оператора operator[], который должен возвращать константную ссылку на node, т.к любое изменение сопровождается определенными вычислениями
	Функция может быть нерекурсивной, так как направление поиска от текущего узла всегда определено и однозначно

 



•	Поиск по предикату

	Пусть задан некоторый логический предикат p(k), который проверяет выполнение некоторого свойства для элемента последовательности с номером k
	Определим   , то есть предикат p будет истинным на диапазоне элементов [l; r) тогда и только тогда, когда он будет истинным хотя бы на одном элементе из диапазона 
	Будем считать, что существует возможность вычислить значение p(l, r) за O(1) независимо от длины диапазона 
	Тогда, для любого диапазона [l; r) возможно решить задачу нахождения такого k, что k [l; r) и p(k) истина со сложностью O(log n)
	для вычисления p (l, r) можно использовать предподсчитанные значения в узлах дерево отрезков

	Предикат без параметров

Если предикат p(k) не зависит от других параметров, то для каждого узла дерева можно выполнить предподсчет самого p(l, r)

Например, для предиката p(k)≡х(k) = 0 в каждом узле дерева можно хранить информацию о том, равен ли нулю хотя бы один элемент из контролируемого диапазона 

	Предикат “больше”

Построим дерево отрезков с операцией максимума. Обозначим за max ([l; r)) максимум для заданного диапазона элементов 

В качестве предиката возьмем p (k, a) ≡ х(k) > а. Таким образом, требуется находить элемент больший, чем некоторая константа k

Тогда, тот же предикат для диапазона можно вычислить как 
p (l, r, a) ≡ max ([l; r))  > a




•	Метод first_after

Метод first_after (k(позиция), p(предикат)) позволяет найти самый левый(наименьший) индекс i >= k, для которого p(i) – истина

 
Сначала проверяем есть ли вообще подходящий для нас элемент. Если нет, то возвращаем правую границу+1. Затем если предикат истинен, и мы сейчас находимся в листе, то возвращаем номер листа 
Потом проталкивание и середина. Сначала вызываем рекурсию для левого поддерева, так как надо найти самый маленький элемент, проверяя позицию. Иначе ищем справа
 
•	Линейные преобразования

	Функция вида f(x) = ax + b называется линейной 
	Композиция двух линейных функций также является линейной функцией 
	Линейные преобразования можно использовать в качестве отложенной операции в дереве отрезков 
	Для массового отбора данных можно использовать предикаты <, ≠, ≤, ≥, >, =
	Методы дерева отрезков можно обобщить на многомерные структуры, например, на матрицы, для быстрой работы



--- 

# SRC

[Варианты реализации дерева отрезков](https://youtu.be/x_rqRrmajXM)

[Групповые отложенные операции](https://youtu.be/lHNRe0gapGQ)

[Реализация класса узла дерева](https://youtu.be/II4y1zYn_18)

[Операции поиска в дереве отрезков](https://youtu.be/gThxaGtb_6o)

[Рекомендации по выполнению заданий](https://youtu.be/N7YTu8pJcdY)

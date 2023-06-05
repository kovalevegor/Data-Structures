# Семестровое задание

### Реализуйте методы `lower_bound(const T &x)` и `upper_bound(const T &x)` для АВЛ-дерева. 


Методы `lower_bound` и `upper_bound` используются для поиска элементов в `AVL-дереве` или любом отсортированном контейнере с операцией сравнения. Оба метода предоставляют возможность выполнить эффективный поиск элементов, используя информацию о структуре дерева.

`lower_bound`:
+ Метод `lower_bound` возвращает итератор на первый элемент, который не меньше `(greater or equal)` заданного значения или ключа.
+ Если элемент с таким значением существует в дереве, то `lower_bound` возвращает итератор на этот элемент.
+ Если элемент с таким значением отсутствует, то `lower_bound` возвращает итератор на следующий элемент после последнего элемента, который меньше заданного значения.
+ Этот метод полезен, когда нужно выполнить поиск наименьшего элемента, который больше или равен заданному значению.

`upper_bound`:
+ Метод `upper_bound` возвращает итератор на первый элемент, который больше заданного значения или ключа.
+ Если элемент с таким значением существует в дереве, то `upper_bound` возвращает итератор на следующий элемент после него.
+ Если элемент с таким значением отсутствует, то `upper_bound` возвращает итератор на первый элемент, который больше заданного значения.
+ Этот метод полезен, когда нужно выполнить поиск наименьшего элемента, который строго больше заданного значения.

Оба метода (`lower_bound` и `upper_bound`) имеют сложность $O(log n)$, где $n$ - количество элементов в дереве. Это делает их очень эффективными для выполнения поиска в отсортированных контейнерах.

```cpp

iterator avl_tree<T>::lower_bound(const T& x) const {
    basenode* node = header.left; // Начинаем с корня дерева
    basenode* ans = nullptr; // Переменная для хранения ответа

    while (node != nullptr) {
        if (static_cast<bstnode*>(node)->key >= x) { // Если ключ текущего узла больше или равен x
            ans = node; // Сохраняем текущий узел как потенциальный ответ
            node = node->left; // Переходим в левое поддерево для поиска более близкого элемента
        } else {
            node = node->right; // Иначе переходим в правое поддерево
        }
    }

    if (ans == nullptr) {
        // Если ответ не найден (дерево пустое или все элементы меньше x),
        // возвращаем итератор, указывающий на конец дерева
        return iterator(nullptr);
    }

    // Возвращаем итератор, указывающий на найденный узел
    return iterator(ans);
}


iterator avl_tree<T>::upper_bound(const T& x) const {
    basenode* node = header.left; // Начинаем с корня дерева
    basenode* ans = nullptr; // Переменная для хранения ответа

    while (node != nullptr) {
        if (static_cast<bstnode*>(node)->key > x) { // Если ключ текущего узла больше x
            ans = node; // Сохраняем текущий узел как потенциальный ответ
            node = node->left; // Переходим в левое поддерево для поиска более близкого элемента
        } else {
            node = node->right; // Иначе переходим в правое поддерево
        }
    }

    if (ans == nullptr) {
        // Если ответ не найден (дерево пустое или все элементы меньше или равны x),
        // возвращаем итератор, указывающий на конец дерева
        return iterator(nullptr);
    }

    // Возвращаем итератор, указывающий на найденный узел
    return iterator(ans);
}

```

+ Оба метода (`lower_bound` и `upper_bound`) являются методами класса `avl_tree<T>` и возвращают итераторы на элементы в дереве.

+ Переменная `node` инициализируется указателем на левый потомок корневого узла, чтобы начать поиск элемента с верхней части дерева.

+ Переменная `ans` инициализируется как `nullptr`, чтобы сохранить найденный элемент (если он будет найден).

+ Цикл `while` выполняется до тех пор, пока `node` не станет равным `nullptr`. В каждой итерации цикла происходит следующее:
    + Проверяется условие: если ключ текущего узла больше или равен `x` (в `lower_bound`) или строго больше `x` (в `upper_bound`).
    + Если условие выполняется, текущий узел сохраняется в `ans` и переход осуществляется в левое поддерево для поиска более близкого элемента.
    + Если условие не выполняется, переход осуществляется в правое поддерево.

+ После завершения цикла проверяется значение `ans`. Если `ans` остается равным `nullptr`, значит, не был найден ни один элемент, удовлетворяющий условию, и возвращается итератор, указывающий на конец дерева (`nullptr`). В противном случае возвращается итератор, указывающий на найденный узел (`ans`).

---

### Кроме этого, реализуйте операторы присваивания и копирующие конструкторы с семантикой копирования и с семантикой перемещения.

```cpp
template <typename T>
class avl_tree {
    // ...
public:
    // ...

    // Копирующий конструктор с семантикой копирования
    avl_tree(const avl_tree& other) : header(nullptr), _size(0) {
        // Создаем новое дерево и копируем элементы из другого дерева
        for (const T& key : other) {
            insert(key);
        }
    }

    // Оператор присваивания с семантикой копирования
    avl_tree& operator=(const avl_tree& other) {
        if (this != &other) {
            // Очищаем текущее дерево
            clear();

            // Создаем новое дерево и копируем элементы из другого дерева
            for (const T& key : other) {
                insert(key);
            }
        }
        return *this;
    }

    // Копирующий конструктор с семантикой перемещения
    avl_tree(avl_tree&& other) noexcept : header(nullptr), _size(0) {
        // Перемещаем ресурсы из другого дерева в текущее дерево
        header.left = other.header.left;
        _size = other._size;

        // Обнуляем ресурсы в другом дереве
        other.header.left = nullptr;
        other._size = 0;
    }

    // Оператор присваивания с семантикой перемещения
    avl_tree& operator=(avl_tree&& other) noexcept {
        if (this != &other) {
            // Очищаем текущее дерево
            clear();

            // Перемещаем ресурсы из другого дерева в текущее дерево
            header.left = other.header.left;
            _size = other._size;

            // Обнуляем ресурсы в другом дереве
            other.header.left = nullptr;
            other._size = 0;
        }
        return *this;
    }

    // ...
};
```

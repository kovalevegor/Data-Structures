# Лекция 11 | АВЛ-деревья

### 1. Посчитайте, какое минимальное и максимальное количество узлов может быть в АВЛ-дереве высоты 3, высоты 4, и высоты 5.

Для определения минимального и максимального количества узлов в АВЛ-дереве заданной высоты, мы можем использовать следующую формулу:

Минимальное количество узлов в АВЛ-дереве высоты $h$ равно $F(h)$, где $F(h) - h+2$ число Фибоначчи.

Максимальное количество узлов в АВЛ-дереве высоты $h$ равно $2^(h+1) - 1$.

Теперь рассмотрим каждую высоту по отдельности:

`Высота 3:`
Минимальное количество узлов: $F(3) = 5$.
Максимальное количество узлов: $2^(3+1) - 1 = 15$.

`Высота 4:`
Минимальное количество узлов: $F(4) = 8$.
Максимальное количество узлов: $2^(4+1) - 1 = 31$.

`Высота 5:`
Минимальное количество узлов: $F(5) = 13$.
Максимальное количество узлов: $2^(5+1) - 1 = 63$.

Таким образом, минимальное и максимальное количество узлов в АВЛ-дереве зависит от его высоты. Высота определяет ограничения на количество узлов, и они могут варьироваться в соответствии с этими ограничениями.

---

### 2. Докажите, что при выполнении балансировки после вставнки нового занчения в АВЛ-дерево операция поворота выполняется не более одного раза

Для доказательства того, что при выполнении балансировки после вставки нового значения в АВЛ-дерево операция поворота выполняется не более одного раза, нам понадобится рассмотреть две ситуации: вставку в левое поддерево и вставку в правое поддерево.

1. Вставка в левое поддерево:
Пусть мы вставляем новое значение в левое поддерево дерева. После вставки, высота левого поддерева может увеличиться на одну единицу. Если разность высот левого и правого поддеревьев становится больше 1, то мы получаем небалансированное состояние.

+ Если новое значение было вставлено в левое поддерево левого поддерева, выполняется один правый поворот для восстановления баланса. В результате, высота дерева не изменяется, и операция поворота выполняется один раз.

+ Если новое значение было вставлено в правое поддерево левого поддерева, выполняется один левый поворот, а затем один правый поворот для восстановления баланса. В результате, высота дерева не изменяется, и операция поворота выполняется два раза, но общее количество поворотов ограничено одним.

2. Вставка в правое поддерево:
Пусть мы вставляем новое значение в правое поддерево дерева. После вставки, высота правого поддерева может увеличиться на одну единицу. Если разность высот правого и левого поддеревьев становится больше 1, то мы получаем небалансированное состояние.

+ Если новое значение было вставлено в правое поддерево правого поддерева, выполняется один левый поворот для восстановления баланса. В результате, высота дерева не изменяется, и операция поворота выполняется один раз.

+ Если новое значение было вставлено в левое поддерево правого поддерева, выполняется один правый поворот, а затем один левый поворот для восстановления баланса. В результате, высота дерева не изменяется, и операция поворота выполняется два раза, но общее количество поворотов ограничено одним.

Таким образом, в любом случае вставки нового значения в АВЛ-дерево, операция поворота выполняется не более одного раза, что доказывает данное утверждение.

---

### 3. Приведите пример АВЛ-дерерва, в котором при выполнении балансировки после удаления некоторого узла операция поворота выполняется два раза. (Дерево лучше изобразить)

Вот пример АВЛ-дерева, в котором при выполнении балансировки после удаления некоторого узла операция поворота выполняется два раза:

```
        5
       / \
      3   8
     /   / \
    2   7   9
   /
  1

```
В данном примере, если мы удалим узел со значением 7, то произойдет двукратная операция поворота для восстановления баланса. После удаления узла 7, дерево станет выглядеть следующим образом:
```
        5
       / \
      3   8
     /     \
    2       9
   /
  1
```
Далее, будет выполнен правый поворот вокруг узла 8:
```
        5
       / \
      3   9
     / \
    2   8
   /
  1
```
И наконец, будет выполнен левый поворот вокруг узла 5:
```
        3
       / \
      2   5
     /     \
    1       9
           /
          8
```
Таким образом, в данном примере после удаления узла 7 операция поворота выполняется два раза для восстановления баланса.


---

### 4. Почему для хранения высоты в АВЛ-дереве достаточно однобайтового целого числа? Не может ли оно переполниться? 

В АВЛ-дереве для хранения высоты каждого узла действительно достаточно однобайтового целого числа. Почему? Потому что высота АВЛ-дерева ограничена сверху значением $O(log n)$, где $n$ - количество узлов в дереве.

Рассмотрим, почему высота АВЛ-дерева ограничена значением $O(log n)$. В АВЛ-дереве каждый узел содержит информацию о высоте своего поддерева, и эта информация используется для балансировки дерева. При вставке или удалении узла выполняются повороты и перебалансировка, чтобы сохранить баланс дерева. Эти операции гарантируют, что разница высоты левого и правого поддерева каждого узла не превышает $1$. Таким образом, высота АВЛ-дерева ограничена логарифмической функцией от количества узлов в дереве.

Так как высота дерева ограничена значением $O(log n)$, где $n$ - количество узлов, однобайтовое целое число достаточно для хранения этого значения. Диапазон значений однобайтового целого числа составляет от $-128$ до $127$ (для знакового типа) или от $0$ до $255$ (для беззнакового типа). Поскольку высота дерева будет значительно меньше этого диапазона, нет риска переполнения.

---

#### 5. Для каких узлов дерева следует пересчитывать высоту после поворота, если балансировка происходит после вставки вершины? 

После выполнения операции поворота при балансировке АВЛ-дерева, высота должна быть пересчитана для всех узлов, которые были затронуты этой операцией. Это включает вставленную вершину, ее предка, и все узлы по пути от предка до корня дерева.

При вставке новой вершины в АВЛ-дерево, может возникнуть несбалансированность, которая нарушает условие АВЛ-дерева. Для восстановления баланса может потребоваться выполнение одного или нескольких поворотов. После каждого поворота, высота изменяется для всех узлов, которые были переставлены или переподвешены в результате этого поворота. Это необходимо для обновления значения высоты каждого узла и корректного отражения структуры дерева.

Таким образом, после каждого поворота при балансировке после вставки новой вершины, необходимо пересчитать высоту для всех узлов, затронутых этим поворотом. Это гарантирует, что высоты всех узлов будут актуальными и отражающими фактическую структуру дерева.

---

#### 6. Для какиз узлов дерева следует пересчитывать высоту после поворота, если балансировка происходит после удаления вершины?  

При балансировке после удаления вершины из АВЛ-дерева, высота должна быть пересчитана для всех узлов, которые были затронуты этой операцией. Это включает удаленную вершину, ее предка, и все узлы по пути от предка до корня дерева.

При удалении вершины из АВЛ-дерева может возникнуть несбалансированность, которая нарушает условие АВЛ-дерева. Для восстановления баланса может потребоваться выполнение одного или нескольких поворотов. После каждого поворота, высота изменяется для всех узлов, которые были переставлены или переподвешены в результате этого поворота. Это необходимо для обновления значения высоты каждого узла и корректного отражения структуры дерева.

Таким образом, после каждого поворота при балансировке после удаления вершины, необходимо пересчитать высоту для всех узлов, затронутых этим поворотом. Это гарантирует, что высоты всех узлов будут актуальными и отражающими фактическую структуру дерева.

---

#### 7. Сделайте копию файла с классом `bstree`. Далее потребуется выполнить доработку класс так, чтобы он реализовывал АВЛ-дерево. Реализуйте два статических метода `void rotate_left(basenode*)` и `void rotate_right(basenode*). Для тестирования добавьте в класс методы
```
void rotate_left(iterator it) {
        rotate_left(it node);
}
```
#### и аналогичный для правого поворота

```cpp
template <typename T>
void avl_tree<T>::rotate_left(basenode* node) {
    basenode* pivot = node->right;  // Сохраняем указатель на правого потомка узла node в переменной pivot.
    node->right = pivot->left;      // Переназначаем правого потомка узла node на левого потомка узла pivot.

    if (pivot->left != nullptr) {
        pivot->left->parent = node; // Если у узла pivot есть левый потомок, то обновляем его родителя на node.
    }

    pivot->parent = node->parent;   // Обновляем родителя узла pivot на родителя узла node.

    if (node->parent == &header) {
        header.left = pivot;        // Если node был корневым узлом, то обновляем корень дерева на pivot.
    } else if (node == node->parent->left) {
        node->parent->left = pivot;  // Если node был левым потомком своего родителя, то обновляем левого потомка родителя на pivot.
    } else {
        node->parent->right = pivot; // Иначе обновляем правого потомка родителя на pivot.
    }

    pivot->left = node;              // Делаем узел node левым потомком узла pivot.
    node->parent = pivot;            // Обновляем родителя узла node на pivot.

    // После поворота пересчитываем высоты узлов node и pivot.
    node->height = std::max(node_height(node->left), node_height(node->right)) + 1;
    pivot->height = std::max(node_height(pivot->left), node_height(pivot->right)) + 1;
}
template <typename T>
void avl_tree<T>::rotate_right(basenode* node) {
    basenode* pivot = node->left;   // Сохраняем указатель на левого потомка узла node в переменной pivot.
    node->left = pivot->right;      // Переназначаем левого потомка узла node на правого потомка узла pivot.

    if (pivot->right != nullptr) {
        pivot->right->parent = node; // Если у узла pivot есть правый потомок, то обновляем его родителя на node.
    }

    pivot->parent = node->parent;   // Обновляем родителя узла pivot на родителя узла node.

    if (node->parent == &header) {
        header.left = pivot;        // Если node был корневым узлом, то обновляем корень дерева на pivot.
    } else if (node == node->parent->right) {
        node->parent->right = pivot; // Если node был правым потомком своего родителя, то обновляем правого потомка родителя на pivot.
    } else {
        node->parent->left = pivot;  // Иначе обновляем левого потомка родителя на pivot.
    }

    pivot->right = node;             // Делаем узел node правым потомком узла pivot.
    node->parent = pivot;            // Обновляем родителя узла node на pivot.

    // После поворота пересчитываем высоты узлов node и pivot.
    node->height = std::max(node_height(node->left), node_height(node->right)) + 1;
    pivot->height = std::max(node_height(pivot->left), node_height(pivot->right)) + 1;
}
```
---

#### 8. Добавьте в класс узла следующие составляющие:

+ Добавьте в класс узла поле `height` типа `char`. Добавьте в конструктор инициализацию этого поля единицей.

```cpp
template <typename T>
class avl_tree {
    struct basenode {
        basenode* left;
        basenode* right;
        basenode* parent;
        char height;
        basenode(basenode* p) : left(nullptr), right(nullptr), parent(p), height(1) {}
    };
```

+ Добавьте статический метод `char_height(basenode*)` которая будет возвращать высоту поддерева, с учетом того, что параметр может быть пустым указателем. 

```cpp
template <typename T>
class avl_tree {
    // ...

    static char char_height(basenode* node) {
        if (node == nullptr) {
            return 0;
        }
        return static_cast<bstnode*>(node)->height;
    }

    // ...
};
```

+ Добавьте в функицю `dfs` вывод высоты текущего поддерева

```cpp
void avl_tree<T>::dfs(int dep, basenode* node) {
    if (node == nullptr)
        return;
    dfs(dep + 1, node->left);
    std::cout.width(dep * 5);
    std::cout << *iterator(node) << ' ' << (node) << ' ' << node->parent << ' ' << int(char_height(node)) << std::endl;
    dfs(dep + 1, node->right);
}
```
+ Начините разрабатывать методы `void balancing_after_insert(basenode*)` и `void balancing_after_erase(basenode*)`, которые будут пересчитывать веса вершин (пока без самой балансировки). Вставьте вызовы этих методов во все требуемые места программы. Подсказка: Над этим вопросом следует внимательно подумать. Для каждого из случаев эти методы требуется применять к различным узлам. На этом этапе реализация методов не будет различаться. 

```cpp
template <typename T>
void avl_tree<T>::balancing_after_insert(basenode* node) {
    while (node != nullptr) {
        node->weight = std::max(char_height(node->left), char_height(node->right)) + 1;
        node = node->parent;
    }
}

template <typename T>
void avl_tree<T>::balancing_after_erase(basenode* node) {
    balancing_after_insert(node);
}
```
+ Проверьте работу на прилагаемых тестах

---

### Работающая программа 

```cpp
#include <iostream>

#ifndef SRTTREE_H
#define SRTTREE_H

#include <utility>
#include <iostream>
#include <initializer_list>
#include <map>

template <typename T>
class avl_tree {
    struct basenode {
        basenode* left;
        basenode* right;
        basenode* parent;
        char height;
        basenode(basenode* p) : left(nullptr), right(nullptr), parent(p), height(1) {}
    };

    struct bstnode : basenode {
        T key;
        int height;
        bstnode(T k, basenode* p) : basenode(p), key(k), height(1) {}
    };

    // Методы для поиска следующего и предыдущего узла для итератора
    static basenode* next(basenode* node);
    static basenode* prev(basenode* node);
    void balancing_after_insert(basenode* node);
    void balancing_after_erase(basenode* node);

    // Методы для поворотов
    //void rotate_left(basenode* node);
    //void rotate_right(basenode* node);

    // Итератор

    class iterator {
    private:
        basenode* node;

    public:
        friend class avl_tree<T>;

        using value_type = T;
        using difference_type = std::ptrdiff_t;
        using reference = T&;
        using pointer = T*;
        using iterator_category = std::bidirectional_iterator_tag;

        iterator() : node(nullptr) {}
        iterator(basenode* nd) : node(nd) {}


        reference operator*() {
            return static_cast<bstnode*>(node)->key;
        }

        operator basenode* () const {
            return node;
        }

        iterator& operator++() {
            node = next(node);
            return *this;
        }

        iterator operator++(int) {
            iterator temp = *this;
            ++(*this);
            return temp;
        }

        iterator& operator--() {
            node = prev(node);
            return *this;
        }

        iterator operator--(int) {
            iterator temp = *this;
            --(*this);
            return temp;
        }



        bool operator==(const iterator& other) const {
            return node == other.node;
        }

        bool operator!=(const iterator& other) const {
            return !(*this == other);
        }

        operator const T& () const {
            return static_cast<bstnode*>(node)->key;
        }
    };

private:
    int _size;
    basenode header;
    void rec_clear(basenode*);
    void dfs(int dep, basenode* node) {
        if (node == nullptr)
            return;
        dfs(dep + 1, node->left);
        std::cout.width(dep * 5);
        std::cout << *iterator(node) << ' ' << (node) << ' ' << node->parent << ' ' << int(char_height(node)) << std::endl;
        dfs(dep + 1, node->right);
    };
public:
    static int char_height(basenode* node) {
        if (node == nullptr) {
            return 0; // Пустое поддерево имеет высоту 0
        } else {
            return node->height; // Возвращаем значение поля height объекта node
        }
    }
    void rotate_left(avl_tree<T>::basenode* node) {
        basenode* right_child = node->right;
        node->right = right_child->left;
        if (right_child->left != nullptr) {
            right_child->left->parent = node;
        }
        right_child->parent = node->parent;
        if (node->parent->left == node) {
            node->parent->left = right_child;
        }
        else {
            node->parent->right = right_child;
        }
        right_child->left = node;
        node->parent = right_child;

        // Обновление высоты узлов
        int left_height = (node->left != nullptr) ? static_cast<bstnode*>(node->left)->height : 0;
        int right_height = (node->right != nullptr) ? static_cast<bstnode*>(node->right)->height : 0;
        node->height = (left_height > right_height) ? left_height + 1 : right_height + 1;

        left_height = (right_child->left != nullptr) ? static_cast<bstnode*>(right_child->left)->height : 0;
        right_height = (right_child->right != nullptr) ? static_cast<bstnode*>(right_child->right)->height : 0;
        right_child->height = (left_height > right_height) ? left_height + 1 : right_height + 1;
    }
    void rotate_right(avl_tree<T>::basenode* node) {
        basenode* left_child = node->left;
        node->left = left_child->right;
        if (left_child->right != nullptr) {
            left_child->right->parent = node;
        }
        left_child->parent = node->parent;
        if (node->parent->left == node) {
            node->parent->left = left_child;
        }
        else {
            node->parent->right = left_child;
        }
        left_child->right = node;
        node->parent = left_child;

        // Обновление высоты узлов
        int left_height = (node->left != nullptr) ? static_cast<bstnode*>(node->left)->height : 0;
        int right_height = (node->right != nullptr) ? static_cast<bstnode*>(node->right)->height : 0;
        node->height = (left_height > right_height) ? left_height + 1 : right_height + 1;

        left_height = (left_child->left != nullptr) ? static_cast<bstnode*>(left_child->left)->height : 0;
        right_height = (left_child->right != nullptr) ? static_cast<bstnode*>(left_child->right)->height : 0;
        left_child->height = (left_height > right_height) ? left_height + 1 : right_height + 1;
    }
    avl_tree() : header(nullptr), _size(0) {}
    avl_tree(const std::initializer_list<T>& lst) : header(nullptr), _size(0) {
        // Вписать построение дерева по списку инициализации
        for (const T& value : lst) {
            insert(value);
        }
    }

    ~avl_tree() {
        clear();
    }

    int size() {
        return _size;
    }

    void print() {
        std::cout << "size=" << _size << std::endl;
        dfs(0, header.left);
    }

    void dump() {
        std::map<basenode*, int> mn;
        int num = 1;
        dfs_dump(num, header.left, mn);
    }

    void dfs_dump(int& num, basenode* node, std::map<basenode*, int>& mn) {
        mn[node] = num++;
        if (node->left != nullptr)
            dfs_dump(num, node->left, mn);
        if (node->right != nullptr)
            dfs_dump(num, node->right, mn);
    }

    void clear() {
        if (header.left != nullptr) {
            rec_clear(header.left);
            header.left = nullptr;
        }
        _size = 0;
    }

    iterator find(const T& key) const {
        basenode* node = header.left;
        while (node != nullptr) {
            if (key < static_cast<bstnode*>(node)->key) {
                node = node->left;
            }
            else if (static_cast<bstnode*>(node)->key < key) {
                node = node->right;
            }
            else {
                return iterator(node);
            }
        }
        return iterator(nullptr);
    }

    iterator lower_bound(const T& key) const {
        basenode* node = header.left;
        basenode* ans = nullptr;
        while (node != nullptr) {
            if (static_cast<bstnode*>(node)->key >= key) {
                ans = node;
                node = node->left;
            }
            else {
                node = node->right;
            }
        }
        if (ans == nullptr) {
            return iterator(nullptr);
        }
        return iterator(ans);
    }

    iterator insert(const T& key) {
        if (header.left == nullptr) {
            header.left = new bstnode(key, &header);
            ++_size;
            return iterator(header.left);
        }

        basenode* parent = &header;
        basenode* node = header.left;
        while (node != nullptr) {
            if (key < static_cast<bstnode*>(node)->key) {
                parent = node;
                node = node->left;
            }
            else if (static_cast<bstnode*>(node)->key < key) {
                parent = node;
                node = node->right;
            }
            else {
                // Вставка дубликата не разрешена
                return iterator(nullptr);
            }
        }

        bstnode* new_node = new bstnode(key, parent);
        if (key < static_cast<bstnode*>(parent)->key) {
            parent->left = new_node;
        }
        else {
            parent->right = new_node;
        }

        ++_size;

        // Расчет и обновление высоты для всех предков нового узла
        basenode* cur_node = new_node->parent;
        while (cur_node != &header) {
            int left_height = (cur_node->left != nullptr) ? static_cast<bstnode*>(cur_node->left)->height : 0;
            int right_height = (cur_node->right != nullptr) ? static_cast<bstnode*>(cur_node->right)->height : 0;
            cur_node->height = (left_height > right_height) ? left_height + 1 : right_height + 1;
            cur_node = cur_node->parent;
        }

        // Проверка и восстановление баланса
        cur_node = new_node->parent;
        while (cur_node != &header) {
            int left_height = (cur_node->left != nullptr) ? static_cast<bstnode*>(cur_node->left)->height : 0;
            int right_height = (cur_node->right != nullptr) ? static_cast<bstnode*>(cur_node->right)->height : 0;
            if (std::abs(left_height - right_height) > 1) {
                // Необходимо выполнить вращение
                if (right_height > left_height) {
                    if (key < static_cast<bstnode*>(cur_node->right)->key) {
                        // Двойное вращение вправо-влево
                        rotate_right(cur_node->right);
                    }
                    rotate_left(cur_node);
                }
                else {
                    if (static_cast<bstnode*>(cur_node->left)->key < key) {
                        // Двойное вращение влево-вправо
                        rotate_left(cur_node->left);
                    }
                    rotate_right(cur_node);
                }
                break;
            }
            cur_node = cur_node->parent;
        }
        balancing_after_insert(cur_node);
        return iterator(new_node);
    }

    void erase(const T& key) {
        basenode* node = header.left;
        while (node != nullptr) {
            if (key < static_cast<bstnode*>(node)->key) {
                node = node->left;
            }
            else if (static_cast<bstnode*>(node)->key < key) {
                node = node->right;
            }
            else {
                // Найден узел для удаления
                basenode* parent = node->parent;
                basenode* left_child = node->left;
                basenode* right_child = node->right;
                delete node;

                // Обновление указателя на узел родителя
                if (parent->left == node) {
                    parent->left = (left_child != nullptr) ? left_child : right_child;
                }
                else {
                    parent->right = (left_child != nullptr) ? left_child : right_child;
                }

                // Обновление указателя родителя для потомков узла
                if (left_child != nullptr) {
                    left_child->parent = parent;
                }
                if (right_child != nullptr) {
                    right_child->parent = parent;
                }

                --_size;

                // Расчет и обновление высоты для всех предков удаленного узла
                basenode* cur_node = parent;
                while (cur_node != &header) {
                    int left_height = (cur_node->left != nullptr) ? static_cast<bstnode*>(cur_node->left)->height : 0;
                    int right_height = (cur_node->right != nullptr) ? static_cast<bstnode*>(cur_node->right)->height : 0;
                    cur_node->height = (left_height > right_height) ? left_height + 1 : right_height + 1;
                    cur_node = cur_node->parent;
                }

                // Проверка и восстановление баланса
                cur_node = parent;
                while (cur_node != &header) {
                    int left_height = (cur_node->left != nullptr) ? static_cast<bstnode*>(cur_node->left)->height : 0;
                    int right_height = (cur_node->right != nullptr) ? static_cast<bstnode*>(cur_node->right)->height : 0;
                    if (std::abs(left_height - right_height) > 1) {
                        // Необходимо выполнить вращение
                        if (right_height > left_height) {
                            int right_left_height = (cur_node->right->left != nullptr) ? static_cast<bstnode*>(cur_node->right->left)->height : 0;
                            int right_right_height = (cur_node->right->right != nullptr) ? static_cast<bstnode*>(cur_node->right->right)->height : 0;
                            if (right_left_height > right_right_height) {
                                // Двойное вращение вправо-влево
                                rotate_right(cur_node->right);
                            }
                            rotate_left(cur_node);
                        }
                        else {
                            int left_left_height = (cur_node->left->left != nullptr) ? static_cast<bstnode*>(cur_node->left->left)->height : 0;
                            int left_right_height = (cur_node->left->right != nullptr) ? static_cast<bstnode*>(cur_node->left->right)->height : 0;
                            if (left_right_height > left_left_height) {
                                // Двойное вращение влево-вправо
                                rotate_left(cur_node->left);
                            }
                            rotate_right(cur_node);
                        }
                        cur_node = cur_node->parent;  // Узел стал предком после вращения
                    }
                    else {
                        cur_node = cur_node->parent;
                    }
                }
                balancing_after_erase(cur_node);
                break;
            }
        }
    }

    iterator begin() const {
        basenode* node = header.left;
        while (node->left != nullptr) {
            node = node->left;
        }
        return iterator(node);
    }

    iterator end() const {
        return iterator(nullptr);
    }
};

template <typename T>
typename avl_tree<T>::basenode* avl_tree<T>::next(typename avl_tree<T>::basenode* node) {
    if (node->right != nullptr) {
        node = node->right;
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    while (node->parent != nullptr && node->parent->right == node) {
        node = node->parent;
    }

    return node->parent;
}

template <typename T>
typename avl_tree<T>::basenode* avl_tree<T>::prev(typename avl_tree<T>::basenode* node) {
    if (node == nullptr) {
        return nullptr;
    }

    if (node->left != nullptr) {
        node = node->left;
        while (node->right != nullptr) {
            node = node->right;
        }
        return node;
    }

    while (node->parent != nullptr && node->parent->left == node) {
        node = node->parent;
    }

    return node->parent;
}

template <typename T>
void avl_tree<T>::balancing_after_insert(basenode* node) {
    basenode* cur_node = node;
    while (cur_node != &header) {
        int left_height = (cur_node->left != nullptr) ? static_cast<bstnode*>(cur_node->left)->height : 0;
        int right_height = (cur_node->right != nullptr) ? static_cast<bstnode*>(cur_node->right)->height : 0;
        int cur_height = (left_height > right_height) ? left_height + 1 : right_height + 1;

        int balance_factor = right_height - left_height;

        if (balance_factor > 1) {
            // Необходимо выполнить вращение влево
            int right_left_height = (cur_node->right->left != nullptr) ? static_cast<bstnode*>(cur_node->right->left)->height : 0;
            int right_right_height = (cur_node->right->right != nullptr) ? static_cast<bstnode*>(cur_node->right->right)->height : 0;
            if (right_left_height > right_right_height) {
                // Двойное вращение вправо-влево
                rotate_right(cur_node->right);
            }
            rotate_left(cur_node);
        }
        else if (balance_factor < -1) {
            // Необходимо выполнить вращение вправо
            int left_left_height = (cur_node->left->left != nullptr) ? static_cast<bstnode*>(cur_node->left->left)->height : 0;
            int left_right_height = (cur_node->left->right != nullptr) ? static_cast<bstnode*>(cur_node->left->right)->height : 0;
            if (left_right_height > left_left_height) {
                // Двойное вращение влево-вправо
                rotate_left(cur_node->left);
            }
            rotate_right(cur_node);
        }

        cur_node->height = cur_height;
        cur_node = cur_node->parent;
    }
}

template <typename T>
void avl_tree<T>::balancing_after_erase(basenode* node) {
    balancing_after_insert(node);
}

template <typename T>
void avl_tree<T>::rec_clear(avl_tree<T>::basenode* node) {
    if (node->left != nullptr) {
        rec_clear(node->left);
    }
    if (node->right != nullptr) {
        rec_clear(node->right);
    }
    delete node;
}




#endif  // SRTTREE_H


using namespace std;
template <typename T>
void print(T& seq) {
    cout << "size=" << seq.size() << endl;
    for (auto& x : seq) cout << x << ' ';
    cout << endl;
    auto it = seq.begin();
    for (auto it = seq.end(); it != seq.begin();) {
        --it;
        cout << *it << ' ';
    }
    cout << endl;
}

using namespace std;

int main() {
    cout << "******************" << endl;
    cout << "*     task 7     *" << endl;
    cout << "******************" << endl;
    // Проверка задания 7.
    avl_tree<int> s1 = { 8,4,12,2,6,10,14,1,3,5,7,9,11,13,15,0,16 };
    s1.rotate_left(s1.find(8));
    s1.print();
    /* Правильный ответ
    size=17
                            0 0x2632520 0x1001be0
                       1 0x1001be0 0x1001b20
                  2 0x1001b20 0x1001ac0
                       3 0x1001c10 0x1001b20
             4 0x1001ac0 0x1001750
                       5 0x1001c40 0x1001b50
                  6 0x1001b50 0x1001ac0
                       7 0x1001c70 0x1001b50
        8 0x1001750 0x1001af0
                  9 0x1001ca0 0x1001b80
            10 0x1001b80 0x1001750
                 11 0x2632490 0x1001b80
    12 0x1001af0 0x62fd88
            13 0x26324c0 0x1001bb0
       14 0x1001bb0 0x1001af0
            15 0x26324f0 0x1001bb0
                 16 0x26325d0 0x26324f0
    */
    s1.rotate_left(s1.find(15));
    s1.print();
    print(s1);
    /* Правильный ответ
    size=17
                            0 0x642520 0xf71be0
                       1 0xf71be0 0xf71b20
                  2 0xf71b20 0xf71ac0
                       3 0xf71c10 0xf71b20
             4 0xf71ac0 0xf71750
                       5 0xf71c40 0xf71b50
                  6 0xf71b50 0xf71ac0
                       7 0xf71c70 0xf71b50
        8 0xf71750 0xf71af0
                  9 0xf71ca0 0xf71b80
            10 0xf71b80 0xf71750
                 11 0x642490 0xf71b80
    12 0xf71af0 0x62fd88
            13 0x6424c0 0xf71bb0
       14 0xf71bb0 0xf71af0
                 15 0x6424f0 0x6427e0
            16 0x6427e0 0xf71bb0
    size=17
    0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
    16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
    */
    s1.rotate_right(s1.find(12));
    s1.rotate_right(s1.find(16));
    s1.print();
    print(s1);
    /* Правильный ответ
    size=17
                       0 (1) 0x2552520 0xd91be0
                  1 (2) 0xd91be0 0xd91b20
             2 (3) 0xd91b20 0xd91ac0
                  3 (1) 0xd91c10 0xd91b20
        4 (4) 0xd91ac0 0xd91750
                  5 (1) 0xd91c40 0xd91b50
             6 (2) 0xd91b50 0xd91ac0
                  7 (1) 0xd91c70 0xd91b50
    8 (5) 0xd91750 0x62fd78
                  9 (1) 0xd91ca0 0xd91b80
            10 (2) 0xd91b80 0xd91af0
                 11 (1) 0x2552490 0xd91b80
       12 (4) 0xd91af0 0xd91750
                 13 (1) 0x25524c0 0xd91bb0
            14 (3) 0xd91bb0 0xd91af0
                 15 (2) 0x25524f0 0xd91bb0
                      16 (1) 0x2552660 0x25524f0
    size=17
    0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
    16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
    */

    cout << "******************" << endl;
    cout << "*     task 8     *" << endl;
    cout << "******************" << endl;
    // Проверка задания 8
    avl_tree<int> s2 = { 20,19,9,8,7 };
    s2.print();
    /* Правильный ответ
    size=5
                       7 (1) 0x642810 0x6426c0
                  8 (2) 0x6426c0 0x642750
             9 (3) 0x642750 0x6427e0
       19 (4) 0x6427e0 0x642840
    20 (5) 0x642840 0x62fd08
    */
    for (int i = 10; i < 19; i++)
        s2.insert(i);
    s2.print();
    /* Правильный ответ
    size=14
                       7 (1) 0x642810 0x6426c0
                  8 (2) 0x6426c0 0x642750
             9 (10) 0x642750 0x6427e0
                 10 (9) 0x6427b0 0x642750
                      11 (8) 0x642660 0x6427b0
                           12 (7) 0x642870 0x642660
                                13 (6) 0x6425a0 0x642870
                                     14 (5) 0x6425d0 0x6425a0
                                          15 (4) 0x6428a0 0x6425d0
                                               16 (3) 0x642600 0x6428a0
                                                    17 (2) 0x642690 0x642600
                                                         18 (1) 0x6426f0 0x642690
       19 (11) 0x6427e0 0x642840
    20 (12) 0x642840 0x62fd08
    */
    s2.erase(s2.find(18));
    s2.print();
    /* Правильный ответ
    size=13
                       7 (1) 0xee2720 0xee2870
                  8 (2) 0xee2870 0xee2780
             9 (9) 0xee2780 0xee25a0
                 10 (8) 0xee28d0 0xee2780
                      11 (7) 0xee2900 0xee28d0
                           12 (6) 0xee2750 0xee2900
                                13 (5) 0xee28a0 0xee2750
                                     14 (4) 0xee27b0 0xee28a0
                                          15 (3) 0xee2810 0xee27b0
                                               16 (2) 0xee2600 0xee2810
                                                    17 (1) 0xee27e0 0xee2600
       19 (10) 0xee25a0 0xee2690
    20 (11) 0xee2690 0x62fcf8
    */
    s2.erase(s2.find(14));
    s2.print();
    /* Правильный ответ
    size=12
                       7 (1) 0x26428d0 0x26427b0
                  8 (2) 0x26427b0 0x26425d0
             9 (8) 0x26425d0 0x2642810
                 10 (7) 0x2642630 0x26425d0
                      11 (6) 0x2642690 0x2642630
                           12 (5) 0x26427e0 0x2642690
                                13 (4) 0x2642840 0x26427e0
                                     15 (3) 0x26425a0 0x2642840
                                          16 (2) 0x26426c0 0x26425a0
                                               17 (1) 0x2642870 0x26426c0
       19 (9) 0x2642810 0x2642660
    20 (10) 0x2642660 0x62fcf8
    */
    s2.erase(s2.find(19));
    s2.print();
    /* Правильный ответ
    size=11
                  7 (1) 0x25026c0 0x2502750
             8 (2) 0x2502750 0x25027b0
        9 (8) 0x25027b0 0x25028d0
            10 (7) 0x2502900 0x25027b0
                 11 (6) 0x2502660 0x2502900
                      12 (5) 0x2502720 0x2502660
                           13 (4) 0x25025d0 0x2502720
                                15 (3) 0x25026f0 0x25025d0
                                     16 (2) 0x2502870 0x25026f0
                                          17 (1) 0x25027e0 0x2502870
    20 (9) 0x25028d0 0x62fcf8
    */

    s2.erase(s2.find(10));
    s2.erase(s2.find(11));
    s2.erase(s2.find(12));
    s2.erase(s2.find(15));
    s2.erase(s2.find(16));
    s2.erase(s2.find(17));
    s2.insert(12);
    s2.insert(10);
    s2.insert(11);
    s2.print();
    /*  Правильный ответ
    size=8
                  7 (1) 0x2682750 0x26825d0
             8 (2) 0x26825d0 0x2682690
        9 (5) 0x2682690 0x26827e0
                      10 (2) 0x2682870 0x26828d0
                           11 (1) 0x26826c0 0x2682870
                 12 (3) 0x26828d0 0x2682660
            13 (4) 0x2682660 0x2682690
    20 (6) 0x26827e0 0x62fca8
    */
    s2.erase(s2.find(9));
    s2.print();
    /*  Правильный ответ
    size=7
                  7 (1) 0x25927e0 0x25925d0
             8 (2) 0x25925d0 0x2592720
       10 (4) 0x2592720 0x2592660
                      11 (1) 0x25926c0 0x2592600
                 12 (2) 0x2592600 0x2592780
            13 (3) 0x2592780 0x2592720
    20 (5) 0x2592660 0x62fc98
    */
    cout << "******************" << endl;
    cout << "*     task 9     *" << endl;
    cout << "******************" << endl;

    avl_tree<int> s3;
    for (int i = 1; i <= 20; i++)
        s3.insert(i);
    s3.print();
    print(s3);
    /*  Правильный ответ
    size=20
                  1 (1) 0x642690 0x6427e0
             2 (2) 0x6427e0 0x642a20
                  3 (1) 0x6425d0 0x6427e0
        4 (3) 0x642a20 0x6426c0
                  5 (1) 0x642b40 0x642870
             6 (2) 0x642870 0x642a20
                  7 (1) 0x642840 0x642870
    8 (5) 0x6426c0 0x62fc78
                       9 (1) 0x642c00 0x642570
                 10 (2) 0x642570 0x642900
                      11 (1) 0x642810 0x642570
            12 (3) 0x642900 0x642b10
                      13 (1) 0x6426f0 0x642750
                 14 (2) 0x642750 0x642900
                      15 (1) 0x642990 0x642750
       16 (4) 0x642b10 0x6426c0
                 17 (1) 0x642a50 0x642bd0
            18 (3) 0x642bd0 0x642b10
                 19 (2) 0x6428d0 0x642bd0
                      20 (1) 0x6429c0 0x6428d0
    size=20
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    */
    avl_tree<int> s4;
    for (int i = 20; i >= 1; --i)
        s4.insert(i);
    s4.print();
    print(s4);
    /*  Правильный ответ
    size=20
                       1 (1) 0x642930 0x6425d0
                  2 (2) 0x6425d0 0x6427e0
             3 (3) 0x6427e0 0x642ab0
                  4 (1) 0x642ae0 0x6427e0
        5 (4) 0x642ab0 0x642540
                       6 (1) 0x642960 0x642630
                  7 (2) 0x642630 0x6427b0
                       8 (1) 0x642780 0x642630
             9 (3) 0x6427b0 0x642ab0
                      10 (1) 0x642a20 0x642a80
                 11 (2) 0x642a80 0x6427b0
                      12 (1) 0x642a50 0x642a80
    13 (5) 0x642540 0x62fc68
                 14 (1) 0x642840 0x642b10
            15 (2) 0x642b10 0x642bd0
                 16 (1) 0x6426c0 0x642b10
       17 (3) 0x642bd0 0x642540
                 18 (1) 0x642690 0x642600
            19 (2) 0x642600 0x642bd0
                 20 (1) 0x642660 0x642600
    size=20
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    */

    avl_tree<int> s5;
    for (int i = 1, j = 20; i < j; i++, j--) {
        s5.insert(i);
        s5.insert(j);
    }
    s5.print();
    print(s5);
    /*  Правильный ответ
    size=20
                       1 (1) 0x24b27e0 0x24b2810
                  2 (2) 0x24b2810 0x24b29c0
             3 (3) 0x24b29c0 0x24b2ab0
                  4 (1) 0x24b2c00 0x24b29c0
        5 (4) 0x24b2ab0 0x24b2990
                  6 (1) 0x24b2690 0x24b2540
             7 (2) 0x24b2540 0x24b2ab0
                  8 (1) 0x24b2960 0x24b2540
    9 (5) 0x24b2990 0x62fc48
                 10 (2) 0x24b2ae0 0x24b2b40
                      11 (1) 0x24b2600 0x24b2ae0
            12 (3) 0x24b2b40 0x24b2840
                 13 (1) 0x24b2870 0x24b2b40
       14 (4) 0x24b2840 0x24b2990
                 15 (2) 0x24b26c0 0x24b2930
                      16 (1) 0x24b2660 0x24b26c0
            17 (3) 0x24b2930 0x24b2840
                      18 (1) 0x24b2bd0 0x24b2630
                 19 (2) 0x24b2630 0x24b2930
                      20 (1) 0x24b2780 0x24b2630
    size=20
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    */
    for (int i = 1; i <= 10; i++)
        s3.erase(s3.find(i));
    s3.print();
    print(s3);
    /*  Правильный ответ
    size=10
            11 (1) 0x2642ba0 0x26429f0
       12 (3) 0x26429f0 0x2642a50
                 13 (1) 0x2642c00 0x2642a80
            14 (2) 0x2642a80 0x26429f0
                 15 (1) 0x2642a20 0x2642a80
    16 (4) 0x2642a50 0x62fc48
            17 (1) 0x2642ab0 0x26425d0
       18 (3) 0x26425d0 0x2642a50
            19 (2) 0x2642510 0x26425d0
                 20 (1) 0x26424e0 0x2642510
    size=10
    11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11
    */
    for (int i = 1; i <= 10; i++)
        s4.erase(s4.find(i));
    s4.print();
    print(s4);
    /*  Правильный ответ
    size=10
       11 (2) 0x642b10 0x642510
            12 (1) 0x6427e0 0x642b10
    13 (4) 0x642510 0x62fc28
                 14 (1) 0x642840 0x642c30
            15 (2) 0x642c30 0x642930
                 16 (1) 0x642ae0 0x642c30
       17 (3) 0x642930 0x642510
                 18 (1) 0x6427b0 0x642780
            19 (2) 0x642780 0x642930
                 20 (1) 0x642c00 0x642780
    size=10
    11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11
    */
    for (int i = 1; i <= 10; i++)
        s5.erase(s5.find(i));
    s5.print();
    print(s5);
    /*  Правильный ответ
    size=10
            11 (1) 0xe23110 0xe22de0
       12 (2) 0xe22de0 0xe23020
            13 (1) 0xe22d50 0xe22de0
    14 (4) 0xe23020 0x62fc08
            15 (2) 0xe23050 0xe233e0
                 16 (1) 0xe231d0 0xe23050
       17 (3) 0xe233e0 0xe23020
                 18 (1) 0xe230e0 0xe22cf0
            19 (2) 0xe22cf0 0xe233e0
                 20 (1) 0xe22e70 0xe22cf0
    size=10
    11 12 13 14 15 16 17 18 19 20
    20 19 18 17 16 15 14 13 12 11
    */
    return 0;
}
```

#### 9. Завершите написание методов `void balancing_after_insert(basenode*)` и `void balancing_after_erase(basenode*)`, добавив в них вызовы поворотов. Обратите внимание на пересчет весов. Реализации методов будут различаться только по пересчету весов и по выходу из цикла. 

[Сбалансированные деревья](https://youtu.be/w0Y3tWPcbyg)

[АВЛ-деревья](https://youtu.be/gJSg6DizT2U)

[Изменение высоты при повороте](https://youtu.be/P8MvUmrGFQY)

[Контест номер 90](http://olymp.isu.ru/cgi-bin/new-client?contest_id=90&locale_id=1)

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

### AVL-Tree class auto 

```cpp
#include <utility>
#include <iostream>
#include <initializer_list>
#include <map>

#ifndef SRTTREE_H
#define SRTTREE_H

template <typename T>
class avltree
{
    struct basenode
    {
        basenode* left;
        basenode* right;
        basenode* parent;
        basenode(basenode* p) : left(nullptr), right(nullptr), parent(p) {}
    };
    struct avlnode : basenode
    {
        T key;
        char height;
        avlnode(T k, basenode* p) : basenode(p), key(k), height(1) {}
    };
    static basenode* next(basenode* node);
    static basenode* prev(basenode* node);

    class iterator
    {
        basenode* node;

    public:
        friend class avltree<T>;
        typedef std::bidirectional_iterator_tag iterator_category;
        typedef T value_type;
        typedef ptrdiff_t difference_type;
        typedef T& reference;
        typedef T* pointer;

        iterator() : node(nullptr) {}
        iterator(basenode* nd) : node(nd) {}

        const T& operator*()
        {
            return static_cast<const avlnode*>(node)->key;
        }
        iterator& operator++()
        {
            node = next(node);
            return *this;
        }
        iterator& operator--()
        {
            node = prev(node);
            return *this;
        }
        iterator operator++(int)
        {
            iterator t = *this;
            node = next(node);
            return t;
        }
        iterator operator--(int)
        {
            iterator t = *this;
            node = prev(node);
            return t;
        }
        bool operator==(const iterator& other)
        {
            return node == other.node;
        }
        bool operator!=(const iterator& other)
        {
            return node != other.node;
        }
    };

private:
    int _size;
    basenode header;
    void rec_clear(basenode*);
    void dfs(int dep, basenode* node);

public:
    avltree() : header(nullptr), _size(0) {}
    avltree(const std::initializer_list<T>& lst) : header(nullptr), _size(0)
    {
        for (T q : lst)
            insert(q);
    }
    ~avltree()
    {
        clear();
    }
    int size()
    {
        return _size;
    }
    void print()
    {
        std::cout << "size=" << _size << std::endl;
        dfs(0, header.left);
    }
    void dump()
    {
        std::map<basenode*, int> mn;
        int num = 1;
        dfs_dump(num, header.left, mn);
    }
    void dfs_dump(int& num, basenode* node, std::map<basenode*, int>& mn)
    {
        if (node == nullptr)
            return;
        auto fnd = mn.find(node->parent);
        std::cout << static_cast<avlnode*>(node)->key << ' ';
        if (fnd == mn.end())
            std::cout << 0 << std::endl;
        else
            std::cout << fnd->second << std::endl;
        mn[node] = num++;
        dfs_dump(num, node->left, mn);
        dfs_dump(num, node->right, mn);
    }
    std::pair<iterator, bool> insert(T key);
    iterator find(const T& key);
    iterator begin()
    {
        if (_size == 0)
            return end();
        basenode* node = header.left;
        while (node->left != nullptr)
            node = node->left;
        return iterator(node);
    }
    iterator end()
    {
        return iterator(&header);
    }
    void clear()
    {
        rec_clear(header.left);
        header.left = nullptr;
        _size = 0;
    }
    void erase(iterator it);
    void rotate_right(iterator it);
    void rotate_left(iterator it);
    static char height(basenode* node);
    void balancing(basenode* node);
};

template <typename T>
void avltree<T>::dfs(int dep, basenode* node)
{
    if (node == nullptr)
        return;
    dfs(dep + 1, node->left);
    std::cout.width(dep * 5);
    std::cout << *iterator(node) << " (" << static_cast<int>(height(node))
        << ")" << ' ' << node << ' ' << node->parent << std::endl;
    dfs(dep + 1, node->right);
}

template <typename T>
std::pair<typename avltree<T>::iterator, bool> avltree<T>::insert(T key)
{
    if (_size == 0)
    {
        avlnode* node = new avlnode(key, &header);
        header.left = node;
        ++_size;
        return { node, true };
    }
    else
    {
        basenode* node = header.left;
        bool insert_left;
        while (true)
        {
            if (key < (static_cast<avlnode*>(node))->key)
            {
                insert_left = true;
                if (node->left == nullptr)
                    break;
                else
                    node = node->left;
            }
            else if (key > (static_cast<avlnode*>(node))->key)
            {
                insert_left = false;
                if (node->right == nullptr)
                    break;
                else
                    node = node->right;
            }
            else
                return { node, false };
        }
        ++_size;
        node = (insert_left ? node->left : node->right) = new avlnode(key, node);
        balancing(node->parent);
        return { node, true };
    }
}

template <typename T>
typename avltree<T>::iterator avltree<T>::find(const T& key)
{
    basenode* node = header.left;
    while (node != nullptr)
    {
        if (key < (static_cast<avlnode*>(node))->key)
            node = node->left; //
        else if (key > (static_cast<avlnode*>(node))->key)
            node = node->right; //
        else
            return iterator(node);
    }
    return end();
}

template <typename T>
void avltree<T>::rec_clear(basenode* node)
{
    if (node != nullptr)
    {
        if (node->left != nullptr)
            rec_clear(node->left);
        if (node->right != nullptr)
            rec_clear(node->right);
        delete node;
    }
}

template <typename T>
typename avltree<T>::basenode* avltree<T>::next(basenode* node)
{
    if (node->right != nullptr)
    {
        node = node->right;
        while (node->left != nullptr)
            node = node->left;
    }
    else
    {
        basenode* pn = node;
        node = node->parent;
        while (pn != node->left)
        {
            pn = node;
            node = node->parent;
        }
    }
    return node;
}

template <typename T>
typename avltree<T>::basenode* avltree<T>::prev(basenode* node)
{
    if (node->left != nullptr)
    {
        node = node->left;
        while (node->right != nullptr)
            node = node->right;
    }
    else
    {
        basenode* pn = node;
        node = node->parent;
        while (pn != node->right)
        {
            pn = node;
            node = node->parent;
        }
    }
    return node;
}

template <typename T>
void avltree<T>::erase(avltree<T>::iterator it)
{
    basenode* p = it.node->parent;
    if (it.node->left == nullptr)
    {
        if (it.node->right == nullptr)
        {
            (p->left == it.node ? p->left : p->right) = nullptr;
        }
        else
        {
            (p->left == it.node ? p->left : p->right) = it.node->right;
            it.node->right->parent = p;
        }
        balancing(p);
    }
    else if (it.node->right == nullptr)
    {
        (p->left == it.node ? p->left : p->right) = it.node->left;
        it.node->left->parent = p;
        balancing(p);
    }
    else
    {
        basenode* rr = it.node->right;
        if (rr->left == nullptr)
        {
            (it.node->parent->left == it.node ? it.node->parent->left : it.node->parent->right) = rr;
            rr->parent = it.node->parent;
            rr->left = it.node->left;
            it.node->left->parent = rr;
            balancing(rr);
        }
        else
        {
            while (rr->left != nullptr)
                rr = rr->left;
            rr->parent->left = rr->right;
            if (rr->right != nullptr)
                rr->right->parent = rr->parent;
            rr->left = it.node->left;
            rr->right = it.node->right;
            it.node->left->parent = rr;
            it.node->right->parent = rr;
            basenode* t = rr->parent;
            rr->parent = it.node->parent;
            (it.node->parent->left == it.node ? it.node->parent->left : it.node->parent->right) = rr;
            balancing(t);
        }
    }
    --_size;
    delete (it.node);
}

template <typename T>
void avltree<T>::rotate_right(avltree<T>::iterator it)
{
    basenode* ln = it.node->left;
    ln->parent = it.node->parent;
    if (it.node->parent->left == it.node)
        it.node->parent->left = ln;
    else
        it.node->parent->right = ln;
    it.node->left = ln->right;
    if (ln->right != nullptr)
        ln->right->parent = it.node;
    ln->right = it.node;
    it.node->parent = ln;
    static_cast<avlnode*>(it.node)->height = 1 + std::max(height(it.node->left), height(it.node->right));
    static_cast<avlnode*>(ln)->height = 1 + std::max(height(ln->left), height(ln->right));
}

template <typename T>
void avltree<T>::rotate_left(avltree<T>::iterator it)
{
    basenode* rn = it.node->right;
    rn->parent = it.node->parent;
    if (it.node->parent->left == it.node)
        it.node->parent->left = rn;
    else
        it.node->parent->right = rn;
    it.node->right = rn->left;
    if (rn->left != nullptr)
        rn->left->parent = it.node;
    rn->left = it.node;
    it.node->parent = rn;
    static_cast<avlnode*>(it.node)->height = 1 + std::max(height(it.node->left), height(it.node->right));
    static_cast<avlnode*>(rn)->height = 1 + std::max(height(rn->left), height(rn->right));
}

template <typename T>
char avltree<T>::height(basenode* node)
{
    if (node == nullptr)
        return 0;
    return static_cast<avlnode*>(node)->height;
}

template <typename T>
void avltree<T>::balancing(basenode* node)
{
    basenode* t = node;
    while (t != &header)
    {
        char lheight = height(t->left);
        char rheight = height(t->right);
        if (lheight == rheight + 2)
        {
            if (height(t->left->right) > height(t->left->left))
                rotate_left(t->left);
            rotate_right(t);
            t = t->parent->parent;
        }
        else if (lheight + 2 == rheight)
        {
            if (height(t->right->left) > height(t->right->right))
                rotate_right(t->right);
            rotate_left(t);
            t = t->parent->parent;
        }
        else
        {
            char cheight = 1 + std::max(lheight, rheight);
            if (cheight != height(t))
            {
                static_cast<avlnode*>(t)->height = cheight;
                t = t->parent;
            }
            else
                break;
        }
    }
}
#endif

using namespace std;

int main() {
    int n;
    avltree<int> tree;
    for (cin >> n; n > 0; --n) {
        int cm, a;
        cin >> cm;
        if (cm == 1) {
            cin >> a;
            auto it = tree.insert(a);
            cout << static_cast<int>(it.second) << std::endl;
        }
        else if (cm == 2) {
            cin >> a;
            cout << static_cast<int>(tree.find(a) != tree.end()) << endl;
        }
        else if (cm == 3) {
            cin >> a;
            tree.erase(tree.find(a));
        }
        else if (cm == 4) {
            tree.clear();
        }
        else if (cm == 5) {
            cout << tree.size();
            for (auto x : tree)
                cout << ' ' << x;
            cout << endl;
        }
        else if (cm == 6) {
            cout << tree.size();
            for (auto it = tree.end(); it != tree.begin();)
                cout << ' ' << *(--it);
            cout << endl;
        }
        else if (cm == 7) {
            tree.dump();
            tree.print();
        }
    }
}
```




























--- 

# SRC

[Сбалансированные деревья](https://youtu.be/w0Y3tWPcbyg)

[АВЛ-деревья](https://youtu.be/gJSg6DizT2U)

[Изменение высоты при повороте](https://youtu.be/P8MvUmrGFQY)

[Контест номер 90](http://olymp.isu.ru/cgi-bin/new-client?contest_id=90&locale_id=1)


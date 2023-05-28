# Лекция 10 | Деревья сортировки

#### 1. Ответьте на следующие вопросы: 

+ **Запишите перестановку чисел от 1 до 15 при внескении которой в дерево сортировки будет получено дерево из одной ветви, причем направление ветвей будет чередоваться. То есть от корня идет только одна ветвб влево, потом только одна ветвь вправо, потом снова одна ветвь влево и так далее. Предполагается, что элементы будут вставляться в заданном порядке с использованием элементарного алгоритма вставки.**

`1 3 2 5 4 7 6 9 8 11 10 13 12 15 14`

```
        1
         \
          3
         /
        2
         \
          5
         /
        4
         \
          7
         /
        6
         \
          9
         /
        8
         \
          11
         /
        10
         \
          13
         /
        12
         \
          15
         /
        14
```
+ **Запишите любую перестаноку чисел от 1 до 15 при внесении которой в дерево сортировки будет полулучено идеально сбалансированное дерево. Предполагается, что элементы будут вставляться в заданном порядке с использованием элементарного алгоритма вставки.**

`8 4 12 2 6 10 14 1 3 5 7 9 11 13 15`

```
            8
       /        \
      4          12
     / \        /   \
    2   6      10    14
   / \ / \     / \   / \
  1  3 5  7   9  11 13 15

```

+ **Запишите любую перестаноку чисел от 1 до 15 при внесении которой в дерево сортировки будет полулучено дерево из двух ветвей одинаковой длины. При этом одна ветвь должна идти только влево, другая только вправо от корня. Предполагается, что элементы будут вставляться в заданном порядке с использованием элементарного алгоритма вставки.**

`8 7 9 6 10 5 11 4 12 3 13 2 14 1 15`

```
              8
             / \
            7   9
           /     \
          6       10
         /         \
        5           11
       /             \
      4               12
     /                 \
    3                   13
   /                     \
  2                       14
 /                         \
1                           15

```

---

#### 2. Используя в качестве образца итератор класса `list`, запишите класс итератора для `bstree`. Для перемещения по узлам дерева следует использовать статические методы `next` и `prev` класса `bstree`. Сами методы ьудут реализованы позже, то есть итератор будет пока неработоспособен, но ошибок он при этом вызывать не должен. Итератор должен соответсвовать концепции двунаправленного итератора. 

```cpp
class iterator {
private:
    basenode* node;

public:
    friend class bstree<T>;

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

    const T& operator*() const {
        return static_cast<const bstnode*>(node)->key;
    }
};


```

#### 3. Реализуйте метод `insert` в классе `bstree` и инициализирующий конструктор для списка инициализации. Конструктор должен просто вызывать медот `insert` для всех элементов. Метод `insert` не должен добавлять повторяющиеся элементы. Метод `insert` должен возвращать пару из итератора и логического значения так же как и у стандартного класса `set`. 


```cpp
#ifndef SRTTREE_H
#define SRTTREE_H

#include <utility>
#include <iostream>
#include <initializer_list>
#include <map>
template <typename T>
class bstree {
    struct basenode {
        basenode *left;
        basenode *right;
        basenode *parent;
        basenode(basenode *p) : left(nullptr),right(nullptr),parent(p) {}
    };
    struct bstnode:basenode {
        T key;
        bstnode(T k,basenode *p) : basenode(p),key(k) {}
    };
// Методы для поиска следующего и предыдущего узла для итератора
    static basenode* next(basenode* node);
    static basenode* prev(basenode* node);

    

//***********************ITERATOR***********************

    class iterator {
    private:
        basenode* node;

    public:
        friend class bstree<T>;

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

        const T& operator*() const {
            return static_cast<const bstnode*>(node)->key;
        }
    };

//***********************END OF ITERATOR***********************
private:
    int _size;
    basenode header;
    void rec_clear(basenode*);
    void dfs(int dep,basenode* node);
public:
    bstree():header(nullptr),_size(0) {}
    bstree(const std::initializer_list<T> &lst):header(nullptr),_size(0) {
        // Вписать построение дерева по списку инициализации
        for (const T& value : lst) {
        insert(value);
        }
    }
    
    ~bstree() {
        clear();
    }
    int size() {
        return _size;
    }
    void print() {
        std::cout << "size=" << _size << std::endl;
        dfs(0,header.left);
    }
    void dump() {
        std::map<basenode*,int> mn;
        int num=1;
        dfs_dump(num,header.left,mn);
    }
    void dfs_dump(int &num,basenode *node,std::map<basenode*,int> &mn) {
        if (node==nullptr)
            return;
        auto fnd=mn.find(node->parent);
        std::cout << static_cast<bstnode*>(node)->key<<' ';
        if (fnd==mn.end())
            std::cout << 0 << std::endl;
        else
            std::cout << fnd->second << std::endl;
        mn[node]=num++;
        dfs_dump(num,node->left,mn);
        dfs_dump(num,node->right,mn);
    }
    std::pair<iterator,bool> insert(T key);
    iterator find(const T &key);
    iterator begin() {
        basenode* node = header.left;

        while (node != nullptr && node->left != nullptr) {
            node = node->left;
        }

        return iterator(node);
    };

    iterator end() {
        return iterator(nullptr);
    };
    void clear() {
        rec_clear(header.left);
        header.left=nullptr;
        _size=0;
    }
    void erase(iterator it);
};

// Метод для печати дерева. Выводит значение узла, адрес узла и адрес предка.
// Нужен для отладки
// Выводит дерево повернутое на 90 градусов
template <typename T>
void bstree<T>::dfs(int dep,basenode* node) {
       if (node==nullptr)
          return;
       dfs(dep+1,node->left);
       std::cout.width(dep*5);
       std::cout<<*iterator(node)<<' '<<(node)<<' '<<node->parent<<std::endl;
       dfs(dep+1,node->right);
   }

// Метод для вставки нового узла в дерево
template <typename T>
std::pair<typename bstree<T>::iterator,bool> bstree<T>::insert(T key) {
    if (header.left == nullptr) {
        // Если дерево пустое, вставляем новый узел в корень
        basenode* node = new bstnode(key, &header);
        header.left = node;
        ++_size;
        return {iterator(node), true};
    } else {
        // Устанавливаем указатель на корень
        basenode* node = header.left;
        bool insert_left = true;

        while (true) {
            if (key < static_cast<bstnode*>(node)->key) {
                // Вставляем в левое поддерево
                if (node->left == nullptr) {
                    node->left = new bstnode(key, node);
                    ++_size;
                    return {iterator(node->left), true};
                } else {
                    node = node->left;
                }
            } else if (key > static_cast<bstnode*>(node)->key) {
                // Вставляем в правое поддерево
                if (node->right == nullptr) {
                    node->right = new bstnode(key, node);
                    ++_size;
                    return {iterator(node->right), true};
                } else {
                    node = node->right;
                }
            } else {
                // Узел с таким значением уже существует, вставка не производится
                return {iterator(node), false};
            }
        }
    }
}

// Метод для поиска узла в дереве
template <typename T>
typename bstree<T>::iterator bstree<T>::find(const T &key) {
    basenode* node = header.left;

    while (node != nullptr) {
        if (key < static_cast<bstnode*>(node)->key) {
            // Переходим к левому поддереву, так как искомый ключ меньше текущего ключа
            node = node->left;
        } else if (key > static_cast<bstnode*>(node)->key) {
            // Переходим к правому поддереву, так как искомый ключ больше текущего ключа
            node = node->right;
        } else {
            // Ключ найден, возвращаем итератор, указывающий на найденный узел
            return iterator(node);
        }
    }

    // Ключ не найден, возвращаем итератор, указывающий на конец дерева
    return end();
}


// Рекурсивный метод очистки дерева.
// Если указатель не пуст, то чистим левое поддерево,
// чистим правое поддерево, удаляем узел
template <typename T>
void bstree<T>::rec_clear(basenode* node) {
    if (node != nullptr) {
        // Рекурсивно очищаем левое поддерево
        rec_clear(node->left);

        // Рекурсивно очищаем правое поддерево
        rec_clear(node->right);

        // Удаляем текущий узел
        delete static_cast<bstnode*>(node);
    }
}

// Метод для нахождения следущего узла по порядку
template <typename T>
typename bstree<T>::basenode *bstree<T>::next(basenode *node) {
    if (node->right != nullptr) {
            // Смещаемся влево до упора
            node = node->right;
            while (node->left != nullptr) {
                node = node->left;
            }
        } else {
            bstnode* pn = static_cast<bstnode*>(node);
            node = node->parent;
            while (node != nullptr && pn == node->right) {
                pn = static_cast<bstnode*>(node);
                node = node->parent;
            }
        }
    return node;
}
// Метод для нахождения предыдущего узла по порядку
template <typename T>
typename bstree<T>::basenode* bstree<T>::prev(basenode* node) {
    if (node->left != nullptr) {
            // Смещаемся вправо до упора
            node = node->left;
            while (node->right != nullptr) {
                node = node->right;
            }
        } else {
            bstnode* pn = static_cast<bstnode*>(node);
            node = node->parent;
            while (node != nullptr && pn == node->left) {
                pn = static_cast<bstnode*>(node);
                node = node->parent;
            }
        }
    return node;
}

// Метод удаления узла
template <typename T>
void bstree<T>::erase(bstree<T>::iterator it) {
    basenode* node = it.node;

    if (node == nullptr)
        return;

    basenode* parent = node->parent;

    // Случай 1: У узла нет детей
    if (node->left == nullptr && node->right == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = nullptr;
            else
                parent->right = nullptr;
        } else {
            header.left = nullptr;  // Удаляем корень
        }

        delete static_cast<bstnode*>(node);
        --_size;
    }
    // Случай 2: У узла есть только один ребенок
    else if (node->left == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = node->right;
            else
                parent->right = node->right;
        } else {
            header.left = node->right;  // Удаляем корень
        }

        node->right->parent = parent;
        delete static_cast<bstnode*>(node);
        --_size;
    } else if (node->right == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = node->left;
            else
                parent->right = node->left;
        } else {
            header.left = node->left;  // Удаляем корень
        }

        node->left->parent = parent;
        delete static_cast<bstnode*>(node);
        --_size;
    }
    // Случай 3: У узла есть два ребенка
    else {
        // Находим наименьший узел в правом поддереве (следующий по значению)
        basenode* successor = next(node);

        // Заменяем значение удаляемого узла значением наименьшего узла
        static_cast<bstnode*>(node)->key = static_cast<bstnode*>(successor)->key;

        // Рекурсивно удаляем наименьший узел в правом поддереве
        erase(successor);
    }
}

#endif

```

## Работающая программа
```cpp
#ifndef SRTTREE_H
#define SRTTREE_H

#include <utility>
#include <iostream>
#include <initializer_list>
#include <map>
template <typename T>
class bstree {
    struct basenode {
        basenode* left;
        basenode* right;
        basenode* parent;
        basenode(basenode* p) : left(nullptr), right(nullptr), parent(p) {}
    };
    struct bstnode :basenode {
        T key;
        bstnode(T k, basenode* p) : basenode(p), key(k) {}
    };
    // Методы для поиска следующего и предыдущего узла для итератора
    static basenode* next(basenode* node);
    static basenode* prev(basenode* node);



    //***********************ITERATOR***********************

    class iterator {
    private:
        basenode* node;

    public:
        friend class bstree<T>;

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

        const T& operator*() const {
            return static_cast<const bstnode*>(node)->key;
        }
    };

    //***********************END OF ITERATOR***********************
private:
    int _size;
    basenode header;
    void rec_clear(basenode*);
    void dfs(int dep, basenode* node);
public:
    bstree() :header(nullptr), _size(0) {}
    bstree(const std::initializer_list<T>& lst) :header(nullptr), _size(0) {
        // Вписать построение дерева по списку инициализации
        for (const T& value : lst) {
            insert(value);
        }
    }

    ~bstree() {
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
        if (node == nullptr)
            return;
        auto fnd = mn.find(node->parent);
        std::cout << static_cast<bstnode*>(node)->key << ' ';
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
    iterator begin() {
        basenode* node = header.left;

        while (node != nullptr && node->left != nullptr) {
            node = node->left;
        }

        return iterator(node);
    };

    iterator end() {
        return iterator(nullptr);
    };
    void clear() {
        rec_clear(header.left);
        header.left = nullptr;
        _size = 0;
    }
    void erase(iterator it);
};

// Метод для печати дерева. Выводит значение узла, адрес узла и адрес предка.
// Нужен для отладки
// Выводит дерево повернутое на 90 градусов
template <typename T>
void bstree<T>::dfs(int dep, basenode* node) {
    if (node == nullptr)
        return;
    dfs(dep + 1, node->left);
    std::cout.width(dep * 5);
    std::cout << *iterator(node) << ' ' << (node) << ' ' << node->parent << std::endl;
    dfs(dep + 1, node->right);
}

// Метод для вставки нового узла в дерево
template <typename T>
std::pair<typename bstree<T>::iterator, bool> bstree<T>::insert(T key) {
    if (header.left == nullptr) {
        // Если дерево пустое, вставляем новый узел в корень
        basenode* node = new bstnode(key, &header);
        header.left = node;
        ++_size;
        return { iterator(node), true };
    }
    else {
        // Устанавливаем указатель на корень
        basenode* node = header.left;
        bool insert_left = true;

        while (true) {
            if (key < static_cast<bstnode*>(node)->key) {
                // Вставляем в левое поддерево
                if (node->left == nullptr) {
                    node->left = new bstnode(key, node);
                    ++_size;
                    return { iterator(node->left), true };
                }
                else {
                    node = node->left;
                }
            }
            else if (key > static_cast<bstnode*>(node)->key) {
                // Вставляем в правое поддерево
                if (node->right == nullptr) {
                    node->right = new bstnode(key, node);
                    ++_size;
                    return { iterator(node->right), true };
                }
                else {
                    node = node->right;
                }
            }
            else {
                // Узел с таким значением уже существует, вставка не производится
                return { iterator(node), false };
            }
        }
    }
}

// Метод для поиска узла в дереве
template <typename T>
typename bstree<T>::iterator bstree<T>::find(const T& key) {
    basenode* node = header.left;

    while (node != nullptr) {
        if (key < static_cast<bstnode*>(node)->key) {
            // Переходим к левому поддереву, так как искомый ключ меньше текущего ключа
            node = node->left;
        }
        else if (key > static_cast<bstnode*>(node)->key) {
            // Переходим к правому поддереву, так как искомый ключ больше текущего ключа
            node = node->right;
        }
        else {
            // Ключ найден, возвращаем итератор, указывающий на найденный узел
            return iterator(node);
        }
    }

    // Ключ не найден, возвращаем итератор, указывающий на конец дерева
    return end();
}


// Рекурсивный метод очистки дерева.
// Если указатель не пуст, то чистим левое поддерево,
// чистим правое поддерево, удаляем узел
template <typename T>
void bstree<T>::rec_clear(basenode* node) {
    if (node != nullptr) {
        // Рекурсивно очищаем левое поддерево
        rec_clear(node->left);

        // Рекурсивно очищаем правое поддерево
        rec_clear(node->right);

        // Удаляем текущий узел
        delete static_cast<bstnode*>(node);
    }
}

// Метод для нахождения следущего узла по порядку
template <typename T>
typename bstree<T>::basenode* bstree<T>::next(basenode* node) {
    if (node->right != nullptr) {
        // Смещаемся влево до упора
        node = node->right;
        while (node->left != nullptr) {
            node = node->left;
        }
    }
    else {
        bstnode* pn = static_cast<bstnode*>(node);
        node = node->parent;
        while (node != nullptr && pn == node->right) {
            pn = static_cast<bstnode*>(node);
            node = node->parent;
        }
    }
    return node;
}
// Метод для нахождения предыдущего узла по порядку
template <typename T>
typename bstree<T>::basenode* bstree<T>::prev(basenode* node) {
    if (node == nullptr) {
        return nullptr;
    }
    
    if (node->left != nullptr) {
        // Смещаемся вправо до упора
        node = node->left;
        while (node->right != nullptr) {
            node = node->right;
        }
    }
    else {
        bstnode* pn = static_cast<bstnode*>(node);
        node = node->parent;
        while (node != nullptr && pn == node->left) {
            pn = static_cast<bstnode*>(node);
            node = node->parent;
        }
    }
    return node;
}

// Метод удаления узла
template <typename T>
void bstree<T>::erase(bstree<T>::iterator it) {
    basenode* node = it.node;

    if (node == nullptr)
        return;

    basenode* parent = node->parent;

    // Случай 1: У узла нет детей
    if (node->left == nullptr && node->right == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = nullptr;
            else
                parent->right = nullptr;
        }
        else {
            header.left = nullptr;  // Удаляем корень
        }

        delete static_cast<bstnode*>(node);
        --_size;
    }
    // Случай 2: У узла есть только один ребенок
    else if (node->left == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = node->right;
            else
                parent->right = node->right;
        }
        else {
            header.left = node->right;  // Удаляем корень
        }

        node->right->parent = parent;
        delete static_cast<bstnode*>(node);
        --_size;
    }
    else if (node->right == nullptr) {
        if (parent != nullptr) {
            if (parent->left == node)
                parent->left = node->left;
            else
                parent->right = node->left;
        }
        else {
            header.left = node->left;  // Удаляем корень
        }

        node->left->parent = parent;
        delete static_cast<bstnode*>(node);
        --_size;
    }
    // Случай 3: У узла есть два ребенка
    else {
        // Находим наименьший узел в правом поддереве (следующий по значению)
        basenode* successor = next(node);

        // Заменяем значение удаляемого узла значением наименьшего узла
        static_cast<bstnode*>(node)->key = static_cast<bstnode*>(successor)->key;

        // Рекурсивно удаляем наименьший узел в правом поддереве
        erase(successor);
    }
}

#endif

#include <iostream>
//#include "bstree.h"
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

int main() {

    // ПРОВЕРКА ЗАДАНИЯ 3
    cout << "******************" << endl;
    cout << "*     task 3     *" << endl;
    cout << "******************" << endl;

    // Проверка вставки и повторной вставки в пустой список
    bstree<int> empty_set;
    {
        auto t = empty_set.insert(100);
        cout << *(t.first) << ' ' << (int)t.second << endl;
        empty_set.print();
        t = empty_set.insert(100);
        cout << *(t.first) << ' ' << (int)t.second << endl;
        empty_set.print();
    }
    /* Правильный ответ (адреса могут быть другие)
    100 1
    size=1
    100 0xec1750 0x62fdc8
    100 0
    size=1
    100 0xec1750 0x62fdc8
    */
    // Проверка на тесте из лекции (слайд с удалением узла)
    bstree<int> s0 = { 35,18,56,30,43,69,27,32,39,61,21,29,34,42 };
    s0.print();
    /* Правильный ответ (адреса могут быть другие)
    size=14
       18 0x1a1af0 0x1a1ac0
                      21 0x1a1ca0 0x1a1be0
                 27 0x1a1be0 0x1a1b50
                      29 0x2582490 0x1a1be0
            30 0x1a1b50 0x1a1af0
                 32 0x1a1c10 0x1a1b50
                      34 0x25824c0 0x1a1c10
    35 0x1a1ac0 0x62fd38
                 39 0x1a1c40 0x1a1b80
                      42 0x25824f0 0x1a1c40
            43 0x1a1b80 0x1a1b20
       56 0x1a1b20 0x1a1ac0
                 61 0x1a1c70 0x1a1bb0
            69 0x1a1bb0 0x1a1b20
    */
    // Проверка вставки и повторной вставки
    {
        for (int x : {1, 99, 20, 35, 18, 56, 30, 43, 69, 27, 32, 39, 61, 21, 29, 34, 42}) {
            auto t = s0.insert(x);
            cout << *(t.first) << ' ' << (int)t.second << endl;
        }
    }
    /* Правильный ответ
    1 1
    99 1
    20 1
    35 0
    18 0
    56 0
    30 0
    43 0
    69 0
    27 0
    32 0
    39 0
    61 0
    21 0
    29 0
    34 0
    42 0
    */
    s0.print();
    /* Правильный ответ (адреса могут быть другие но должны совпать с предыдущим выводом)
    size=17
             1 0x24b2520 0x21af0
       18 0x21af0 0x21ac0
                           20 0x24b2780 0x21ca0
                      21 0x21ca0 0x21be0
                 27 0x21be0 0x21b50
                      29 0x24b2490 0x21be0
            30 0x21b50 0x21af0
                 32 0x21c10 0x21b50
                      34 0x24b24c0 0x21c10
    35 0x21ac0 0x62fc38
                 39 0x21c40 0x21b80
                      42 0x24b24f0 0x21c40
            43 0x21b80 0x21b20
       56 0x21b20 0x21ac0
                 61 0x21c70 0x21bb0
            69 0x21bb0 0x21b20
                 99 0x24b25d0 0x21bb0
    */
    // Проверка по заданиям на построение дерева
    // В скобки надо вписать требуемую последоватльность чисел
    bstree<int> s1 = {};
    s1.print();
    /* Правильный ответ
    size=15
        1 0x6427b0 0x642520
                  2 0x642600 0x642690
                            3 0x642630 0x642870
                                      4 0x6427e0 0x642660
                                                5 0x6426c0 0x6428a0
                                                          6 0x6428d0 0x642810
                                                                    7 0x642840 0x6426f0
                                                                         8 0x642900 0x642840
                                                               9 0x6426f0 0x6428d0
                                                    10 0x642810 0x6426c0
                                          11 0x6428a0 0x6427e0
                                12 0x642660 0x642630
                      13 0x642870 0x642600
            14 0x642690 0x6427b0
    15 0x642520 0x62fcc8
    */
    bstree<int> s2 = {};
    s2.print();
    /* Правильный ответ
    size=15
                  1 0x25e3170 0x25e2840
             2 0x25e2840 0x25e28d0
                  3 0x25e2db0 0x25e2840
        4 0x25e28d0 0x25e28a0
                  5 0x25e31a0 0x25e2600
             6 0x25e2600 0x25e28d0
                  7 0x25e3290 0x25e2600
    8 0x25e28a0 0x62fc58
                  9 0x25e2e10 0x25e3380
            10 0x25e3380 0x25e27e0
                 11 0x25e31d0 0x25e3380
       12 0x25e27e0 0x25e28a0
                 13 0x25e34d0 0x25e2e70
            14 0x25e2e70 0x25e27e0
                 15 0x25e32f0 0x25e2e70
    */
    bstree<int> s3 = {};
    s3.print();
    /* Правильный ответ
    size=15
                                      1 0x25431a0 0x2542e40
                                 2 0x2542e40 0x2543350
                            3 0x2543350 0x25430e0
                       4 0x25430e0 0x2542ff0
                  5 0x2542ff0 0x2543170
             6 0x2543170 0x2542f00
        7 0x2542f00 0x2543470
    8 0x2543470 0x62fbe8
        9 0x2543020 0x2543470
            10 0x2543260 0x2543020
                 11 0x25433e0 0x2543260
                      12 0x2543290 0x25433e0
                           13 0x2543080 0x2543290
                                14 0x2543440 0x2543080
                                     15 0x25432c0 0x2543440
    */

    // ПРОВЕРКА ЗАДАНИЯ 4

    cout << "******************" << endl;
    cout << "*     task 4     *" << endl;
    cout << "******************" << endl;

    for (int i = 0; i < 100; i++) {
        auto t = s0.find(i);
        if (t != s0.end())
            cout << *t << ' ';
    }
    cout << endl;
    cout << *s0.begin() << endl;
    /* Правильный ответ
    1 18 20 21 27 29 30 32 34 35 39 42 43 56 61 69 99
    1
    */

    // ПРОВЕРКА ЗАДАНИЯ 5

    cout << "******************" << endl;
    cout << "*     task 5     *" << endl;
    cout << "******************" << endl;

    s0.clear();
    s0.print();
    for (auto x : { 35,18,56,30,43,69,27,32,39,61,21,29,34,42 })
        s0.insert(x);
    s0.print();
    /* Правильный ответ
    size=0
    size=14
       18 0x2513020 0x25132f0
                      21 0x2513140 0x2513050
                 27 0x2513050 0x2513080
                      29 0x2513170 0x2513050
            30 0x2513080 0x2513020
                 32 0x2512f90 0x2513080
                      34 0x25131a0 0x2512f90
    35 0x25132f0 0x62fba8
                 39 0x25130b0 0x2513530
                      42 0x25127e0 0x25130b0
            43 0x2513530 0x2513500
       56 0x2513500 0x25132f0
                 61 0x2513110 0x2512f00
            69 0x2512f00 0x2513500
    */

    // ПРОВЕРКА ЗАДАНИЯ 6

    cout << "******************" << endl;
    cout << "*     task 6     *" << endl;
    cout << "******************" << endl;

    // Потребуется удалить комментарии из функции print()
    empty_set.clear();
    print(empty_set);
    print(s0);
    print(s1);
    print(s2);
    print(s3);
    /* Правильный ответ
    size=0


    size=14
    18 21 27 29 30 32 34 35 39 42 43 56 61 69
    69 61 56 43 42 39 35 34 32 30 29 27 21 18
    size=15
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
    15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    size=15
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
    15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    size=15
    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
    15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    */

    cout << "******************" << endl;
    cout << "*     task 7     *" << endl;
    cout << "******************" << endl;

    // ПРОВЕРКА ЗАДАНИЯ 7
    // Удаляем лист
    s0.erase(s0.find(21));
    s0.print();
    /* Ответ
    size=13
       18 0x25f31d0 0x25f2db0
                 27 0x25f2e40 0x25f3350
                      29 0x25f30e0 0x25f2e40
            30 0x25f3350 0x25f31d0
                 32 0x25f34d0 0x25f3350
                      34 0x25f3110 0x25f34d0
    35 0x25f2db0 0x62fba8
                 39 0x25f3050 0x25f3380
                      42 0x25f2660 0x25f3050
            43 0x25f3380 0x25f2ff0
       56 0x25f2ff0 0x25f2db0
                 61 0x25f2e70 0x25f3020
            69 0x25f3020 0x25f2ff0
    */
    // удаляем узел без левых потомков
    s0.erase(s0.find(18));
    s0.print();
    /* Ответ
    size=12
            27 0x643260 0x642fc0
                 29 0x642ed0 0x643260
       30 0x642fc0 0x643020
            32 0x6430b0 0x642fc0
                 34 0x642f00 0x6430b0
    35 0x643020 0x62fb98
                 39 0x642ea0 0x6431a0
                      42 0x642750 0x642ea0
            43 0x6431a0 0x642f60
       56 0x642f60 0x643020
                 61 0x6432f0 0x643050
            69 0x643050 0x642f60
    */
    // удаляем узел без правых потомков
    s0.erase(s0.find(69));
    s0.print();
    /* Ответ
    size=11
            27 0x643110 0x643080
                 29 0x643410 0x643110
       30 0x643080 0x6430e0
            32 0x642f30 0x643080
                 34 0x6434a0 0x642f30
    35 0x6430e0 0x62fb98
                 39 0x642ff0 0x643350
                      42 0x642750 0x642ff0
            43 0x643350 0x642f60
       56 0x642f60 0x6430e0
            61 0x643050 0x642f60
    */

    // Удаляем узел, когда у правого потомка нет левого
    s0.erase(s0.find(30));
    s0.print();
    /* Ответ
    size=10
            27 0x25f3140 0x25f3380
                 29 0x25f33b0 0x25f3140
       32 0x25f3380 0x25f2e40
            34 0x25f2f30 0x25f3380
    35 0x25f2e40 0x62fb98
                 39 0x25f2ea0 0x25f31a0
                      42 0x25f26c0 0x25f2ea0
            43 0x25f31a0 0x25f3080
       56 0x25f3080 0x25f2e40
            61 0x25f3200 0x25f3080
    */
    // И самый сложный случай удаления
    s0.erase(s0.find(35));
    s0.print();
    /*Ответ
    size=9
            27 0x643260 0x643290
                 29 0x6434d0 0x643260
       32 0x643290 0x643470
            34 0x643500 0x643290
    39 0x643470 0x62fb98
                 42 0x642780 0x643350
            43 0x643350 0x643380
       56 0x643380 0x643470
            61 0x6432c0 0x643380
    */
    //И еще раз
    s0.erase(s0.find(39));
    s0.print();
    print(s0);
    /*Ответ
    size=8
            27 0x25832f0 0x2583020
                 29 0x2583380 0x25832f0
       32 0x2583020 0x2582840
            34 0x2583530 0x2583020
    42 0x2582840 0x62fb88
            43 0x2582ea0 0x25831d0
       56 0x25831d0 0x2582840
            61 0x2583320 0x25831d0
    size=8
    27 29 32 34 42 43 56 61
    61 56 43 42 34 32 29 27
    */
    return 0;
}

```















---
# SRC

[Двоичные деревья сортировки (BST)](https://youtu.be/Kw5bQbGkdlA)

[Удаление узла из BST](https://youtu.be/fRGXS9P9aiU)

[Итерация по дереву](https://youtu.be/2F6l2zvaq64)

[Рекомендации по выполнению заданий](https://youtu.be/vVokxMaHU1Y)

[Контест номер 90](http://olymp.isu.ru/cgi-bin/new-client?contest_id=90&locale_id=1)

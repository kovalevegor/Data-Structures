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


















---
# SRC

[Двоичные деревья сортировки (BST)](https://youtu.be/Kw5bQbGkdlA)

[Удаление узла из BST](https://youtu.be/fRGXS9P9aiU)

[Итерация по дереву](https://youtu.be/2F6l2zvaq64)

[Рекомендации по выполнению заданий](https://youtu.be/vVokxMaHU1Y)

[Контест номер 90](http://olymp.isu.ru/cgi-bin/new-client?contest_id=90&locale_id=1)

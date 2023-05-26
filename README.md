# Лекция 9 | Списки и деревья

#### 1. Ответьте на следующие вопросы: 

+ **Протестируйте стандартную реализацию контейнера `list`. Проверьте, что происходит при декременте итератора, указывавшего на начало списка, и при инкременте итератора, указывавшего на конец списка. Чем можно объяснть такое поведение?** 

    1. При декременте итератора, указывающего на начало списка, происходит неопределенное поведение. Это означает, что результат такой операции может быть непредсказуемым и зависит от конкретной реализации и компилятора. В большинстве случаев попытка декрементировать итератор, указывающий на начало списка, приведет к ошибке или нежелательному поведению программы.

    2. При инкременте итератора, указывающего на конец списка `(std::list::end())`, происходит корректное поведение. Инкрементирование итератора, указывающего на конец списка, не вызывает ошибок и позволяет обращаться к следующему элементу после последнего элемента списка.

    3. Поведение при декременте итератора, указывающего на начало списка, и инкременте итератора, указывающего на конец списка, объясняется спецификой реализации двусвязного списка, на котором основан контейнер `std::list`. Двусвязный список не имеет ни начала, ни конца в привычном понимании. Вместо этого, он представляет собой циклическую структуру, где каждый элемент списка содержит указатели на предыдущий и следующий элементы.

    4. Аналогично, итератор, возвращаемый функцией `std::list::end()`, указывает на псевдоэлемент, который является "за последним" элементом списка. При инкрементировании итератора, указывающего на конец списка, мы фактически переходим к псевдоэлементу, который обозначает конец списка и позволяет обращаться к элементам списка после последнего элемента.

+ **Стандартная библиотека реализует также класс однонаправленного списка. У узлов такого списка нет указателя `prev` и перемещаться по нему миожно только вперед. У такого списка нет метода `insert`, но есть метод `insert_after`. Объясните, в чем причина такой замены**

    При реализации однонаправленного списка без указателя `prev`, перемещение по списку возможно только вперед, то есть от головы к хвосту. В этом случае вставка элемента перед заданным узлом требует модификации указателей в предшествующем узле, чтобы он указывал на новый узел.

    Метод `insert_after` используется для вставки нового узла после заданного узла в однонаправленном списке. Этот метод принимает итератор на узел, после которого должен быть вставлен новый узел, и выполняет следующие шаги:

    1. Создает новый узел с заданным значением.
    2. Меняет указатель `next` нового узла, чтобы он указывал на следующий узел после заданного узла.
    3. Меняет указатель `next` заданного узла, чтобы он указывал на новый узел.

    Такая замена (использование `insert_after` вместо `insert`) связана с особенностями работы однонаправленного списка и ограничением перемещения только вперед. Метод `insert_after` позволяет эффективно вставлять новый узел после заданного узла без необходимости обновлять указатели предшествующего узла.

---

#### 2. Обобщенная задача Флавия

```cpp
#include <iostream>
#include <list>

void printList(const std::list<int>& myList) {
    for (const auto& elem : myList) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;
}

int main() {
    int n;
    std::cin >> n;
    std::list<int> children;
    for (int i = 1; i <= n; i++) {
        children.push_back(i);
    }
    std::list<int>::iterator it = children.begin();
    for (int i = 0; i < n - 1; i++) {
        int count;
        std::cin >> count;
        //printList(children);
        for (int j = 0; j < count - 1; j++) {
            ++it;
            if (it == children.end()) {
                it = children.begin();
            }
        }
        //std::cout << "Count " << count << " Текущий элемент: " << *it << std::endl;
        it = children.erase(it);
        //std::cout << "Текущий элемент после удаления: " << *it << std::endl;
        //std::cout << "Текущий элемент после удаления и условия: " << *it << std::endl;
        if (it == children.end()) {
            it = children.begin();
        }
    }

    std::cout << *it << std::endl;

    return 0;
}

```

---


#### 3. Ответьте на следующие вопросы: 
+ **Может ли метод `splice` использоваться для перестановки данных в пределах одного списка?**

Да, метод `splice` в `C++` может быть использован для перестановки данных в пределах одного списка.

В этом случае будет один и тот же список, и перемещение элементов будет происходить внутри него. Нужно указать позицию, на которую переместить элементы, а также указать итераторы, определяющие диапазон элементов, которые нужно переместить.

```cpp
std::list<int> myList = {1, 2, 3, 4, 5};

// Перестановка элементов с индексами 1, 2, 3 в конец списка
auto it1 = std::next(myList.begin(), 1);
auto it2 = std::next(myList.begin(), 4);
myList.splice(myList.end(), myList, it1, it2);

// Вывод элементов после перестановки
for (const auto& elem : myList) {
    std::cout << elem << " "; // Вывод: 1 5 2 3 4
}
std::cout << std::endl;
```

+ **Может ли итератор `pos` в методе `splice` совпадать с итератором  `first` Произойдет ли нарушение целостности данных в этом случае?**

    В методе `splice` стандартной библиотеки `C++`, итератор `pos` может совпадать с итератором `first`, но в таком случае произойдет нарушение целостности данных.

    При использовании `splice`, метод перемещает элементы из одного контейнера в другой или в тот же самый контейнер, изменяя их положение. Если итератор `pos` итерирует элемент, который будет удален и перемещен с помощью `splice`, итератор `first`, который указывает на этот же элемент, будет недействителен после перемещения.

    В результате, при совпадении итераторов `pos` и `first`, возникает неопределенное поведение и нарушение целостности данных, так как будут производиться операции на недействительных итераторах.

    Поэтому не рекомендуется использовать `splice` с одинаковыми итераторами `pos` и `first`. Если необходимо перемещать элементы в пределах одного контейнера, рекомендуется использовать другие методы, такие как `insert` или `emplace`, которые обеспечивают корректное перемещение элементов без нарушения целостности данных.

+ **Может ли итератор `pos` в методе `splice` совпадать с итератором  `list`? Произойдет ли нарушение целостности данных в этом случае?**

    В методе `splice` стандартной библиотеки `C++`, итератор `pos` может совпадать с итератором `list`. В этом случае, если `pos` итерирует элемент, который будет перемещен с помощью `splice`, не происходит нарушения целостности данных.

    В этом случае, элементы будут перемещены на новые позиции в контейнере, включая позицию, на которую указывает `pos`. Итератор `pos` остается действительным и продолжает указывать на тот же самый элемент после перемещения.

    Однако, если `pos` указывает на элемент, который будет удален, а не перемещен, итератор `pos` становится недействительным после удаления элемента. Поэтому необходимо быть осторожным при использовании `splice` и убедиться, что итератор `pos` остается действительным после операции.

+ **Объясните, почему не получается одновременно реализовать методы `splice` и `size` со сложностью `O(1)`**

    Метод `splice` позволяет перемещать элементы между контейнерами или внутри контейнера, не копируя их, а лишь переставляя указатели на элементы. Это позволяет достичь константной сложности времени `O(1)` для операции перемещения элементов.

    Однако, для поддержки операции `size` со сложностью `O(1)`, контейнер должен хранить информацию о текущем размере, чтобы возвращать его непосредственно в методе `size`. Если при перемещении элементов с помощью splice мы не обновляем информацию о размере контейнера, то операция `size` будет работать с неактуальным значением.

    Если мы хотим иметь операцию `size` со сложностью `O(1)`, нам придется вручную обновлять информацию о размере контейнера при каждом вызове `splice`. Это приведет к линейной сложности `O(n)` для операции `splice`, так как при перемещении каждого элемента потребуется обновление информации о размере.


---

#### 4-7 
```cpp

#ifndef LIST_H
#define LIST_H

#include <initializer_list>
#include <iostream>

namespace mylib {

    template <typename T>
    class list {
        struct basenode {
            basenode* prev;
            basenode* next;
            basenode(basenode* prv, basenode* nxt) : prev(prv), next(nxt) {}
        };

        struct listnode : basenode {
            T data;
            listnode(const T& d, basenode* prv, basenode* nxt) : basenode(prv, nxt), data(d) {}
        };

    public:
        class iterator {
        private:
            basenode* node;

        public:
            friend class list<T>;

            using value_type = T;
            using difference_type = std::ptrdiff_t;
            using reference = T&;
            using pointer = T*;
            using iterator_category = std::bidirectional_iterator_tag;

            iterator() : node(nullptr) {}
            iterator(basenode* nd) : node(nd) {}

            reference operator*() {
                return static_cast<listnode*>(node)->data;
            }

            iterator& operator++() {
                node = node->next;
                return *this;
            }

            iterator operator++(int) {
                iterator temp = *this;
                ++(*this);
                return temp;
            }

            iterator& operator--() {
                node = node->prev;
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
        };

    private:
        basenode header;

    public:
        list() : header(&header, &header) {}

        list(const std::initializer_list<T>& lst) : list() {
            for (const auto& elem : lst) {
                push_back(elem);
            }
        }

        ~list() {
            while (!empty()) {
                pop_front();
            }
        }

        void push_back(const T& value) {
            insert(end(), value);
        }

        void push_front(const T& value) {
            insert(begin(), value);
        }

        void pop_back() {
            erase(iterator(header.prev));
        }

        void pop_front() {
            erase(begin());
        }

        iterator insert(iterator pos, const T& d) {
            basenode* node = pos.node;
            listnode* new_node = new listnode(d, node->prev, node);
            node->prev->next = new_node;
            node->prev = new_node;
            return iterator(new_node);
        }

        void erase(iterator pos) {
            basenode* node = pos.node;
            node->prev->next = node->next;
            node->next->prev = node->prev;
            delete static_cast<listnode*>(node);
        }

        int size() {
            int count = 0;
            for (auto it = begin(); it != end(); ++it) {
                ++count;
            }
            return count;
        }

        iterator splice(iterator pos, iterator first, iterator last) {
            if (first == last) {
                return pos;
            }

            basenode* pos_node = pos.node;
            basenode* first_node = first.node;
            basenode* last_node = last.node->prev;

            first_node->prev->next = last_node->next;
            last_node->next->prev = first_node->prev;

            first_node->prev = pos_node->prev;
            last_node->next = pos_node;

            pos_node->prev->next = first_node;
            pos_node->prev = last_node;

            return first;
        }

        iterator begin() {
            return iterator(header.next);
        }

        iterator end() {
            return iterator(&header);
        }

        bool empty() {
            return header.next == &header;
        }
    };

    template <typename T>
    void print(list<T>& lst) {
        std::cout << "size=" << lst.size();
        for (auto& t : lst) {
            std::cout << " " << t;
        }
        for (auto iter = lst.end(); iter != lst.begin();) {
            std::cout << " " << *(--iter);
        }
        std::cout << std::endl;
    }

}  // namespace mylib

#endif  // 
```




---

# SRC 

[Ссылочные структуры данных](https://youtu.be/sHdIrn49824)

[Реализация списка](https://youtu.be/i9AjSDX4da4)

[Рекомендации по выполнению заданий](https://youtu.be/G9P7KfZt8Ts)

[Тренировка по теме "Структуры данных"](http://olymp.isu.ru/cgi-bin/new-client?contest_id=90&locale_id=1)

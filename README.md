# Лекция 4 | Двухуровневая организация памяти

#### 1. Ответьте на следущие вопросы:

<table>
<tr>
</tr>
<td>
Становятся ли указатели, ссылки итераторы на элементы недопустимыми при увеличении или уменьшении размера рассмотренного вида динамических массивов? 
</td>
<td>
Элементы никогда не изменяют свое местоположение в памяти, потому что фрагменты памяти, выделенные для элементов массива, не могут переполниться. Переполниться может только вектор указателей на эти элементы, тем самым изменить только свое местоположение. Поэтому и указатели, и ссылки на элементы не становятся недопустимыми. А итераторы могут стать недопустимыми, поскольку итератор должен содержать местоположение в слое указателей.
</td>
<tr>
<td>
Почему в реализации данного вида динамических массивов всегда требуется иметь как минимум один резервний элемент?
</td>
<td>
Это нужно для итераторов. Итераторы в реализации данного вида динамических массивов представлены как два указателя: один на верхний слой, другой – нижний. Из-за этого и требуется иметь как минимум один резервный элемент, на который указывает итератор end, потому что если будет занят весь верхний слой, мы не сможем установить итератор на нижний слой, потому что его просто не будет
</td> 
</tr>
</table>




---

#### 2. Реализовать vector с двухуровневой схемой выделения памяти. Имеется шаблон, и нужно дополнить его, заменив многоточие на соответсвующие команды. Интерфейс класса должен соответсвовать интерфейсу стандартного класса vector. Требуется реализовать конструктор по умолчанию, конструктор с параметрами, деструктор, метод size(), оператор [] и метод log. При реализации нужно использовать представленный шаблон
```cpp
#include <iostream>
#include <vector>
using std::vector;

template <typename T>
class vector2d {
private:
    // Определение размера чанка в зависимости от размера типа T
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    int n;                      // Общее количество элементов
    vector<T*> chunks;          // Вектор указателей на чанки

public:
    vector2d() : n(0) {
        chunks.push_back(new T[chunk_size]);  // Создаем первый чанк вектора
    }

    vector2d(int _n, T val = T()) : n(_n) {
        int chunk_count = 1 + n / chunk_size;  // Вычисляем общее количество чанков
        int last_chunk_size = n % chunk_size;  // Вычисляем размер последнего чанка
        for (int i = 0; i < chunk_count - 1; i++) {
            chunks.push_back(new T[chunk_size]);              // Создаем новый чанк
            std::fill(chunks[i], chunks[i] + chunk_size, val); // Заполняем чанк значением по умолчанию
        }
        if (last_chunk_size > 0) {
            chunks.push_back(new T[last_chunk_size]);                     // Создаем последний чанк
            std::fill(chunks[chunk_count - 1], chunks[chunk_count - 1] + last_chunk_size, val);  // Заполняем последний чанк значением по умолчанию
        }
    }

    ~vector2d() {
        // Освобождаем память, занятую чанками
        for (auto& chunk : chunks) {
            delete[] chunk;
        }
    }

    T& operator[](int i) {
        int chunk_index = i / chunk_size;    // Вычисляем индекс чанка
        int element_index = i % chunk_size;  // Вычисляем индекс элемента внутри чанка
        return chunks[chunk_index][element_index];  // Возвращаем элемент по указанным индексам
    }

    int size() {
        return n;  // Возвращаем общее количество элементов
    }

    void log() {
        std::cout << "size=" << n << std::endl;                  // Выводим общее количество элементов
        std::cout << "chunks=" << chunks.size() << std::endl;   // Выводим количество чанков
        for (auto p : chunks)
            std::cout << p << ' ';  // Выводим адреса чанков
        std::cout << std::endl;
    }
};

using namespace std;
int main() {
    vector2d<double> v(100, 0.5);          // Создаем объект vector2d с 100 элементами типа double, инициализируя все значения 0.5
    for (int i = 0; i < 50; i++)
        v[i] = i / 100.0;                  // Присваиваем значения элементам вектора
    v.log();                               // Выводим информацию о векторе
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << ' ';               // Выводим элементы вектора
    cout << endl;
    vector2d<double> w;                    // Создаем пустой объект vector2d
    w.log();                               // Выводим информацию о векторе
    vector2d<int> u(128, -1);              // Создаем объект vector2d с 128 элементами типа int, инициализируя все значения -1
    u.log();                               // Выводим информацию о векторе
    return 0;
}

```
В этом примере реализован класс ```vector2d```, который представляет собой двумерный вектор с двухуровневой схемой выделения памяти. Класс имеет ```конструкторы``` по умолчанию и с параметрами, ```деструктор```, метод ```size()```, ```оператор []``` для доступа к элементам и метод ```log()```, который выводит информацию о размере и чанках вектора. Пример использования класса ```vector2d``` также представлен в функции ```main()```.


```txt
size=100
chunks=2
000001C650C51340 000001C650C65060
0 0.01 0.02 0.03 0.04 0.05 0.06 0.07 0.08 0.09 0.1 0.11 0.12 0.13 0.14 0.15 0.16 0.17 0.18 0.19 0.2 0.21 0.22 0.23 0.24 0.25 0.26 0.27 0.28 0.29 0.3 0.31 0.32 0.33 0.34 0.35 0.36 0.37 0.38 0.39 0.4 0.41 0.42 0.43 0.44 0.45 0.46 0.47 0.48 0.49 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5
size=0
chunks=1
000001C650C54490
size=128
chunks=1
000001C650C666C0
```

---

#### 3. Нужно реализовать методы resize, push_back(x), pop_back() для этого шаблона

Шаблон
```cpp
#include <iostream>
#include <vector>
using std::vector;

template <typename T>
class vector2d {
private:
    // Определение размера чанка в зависимости от размера типа T
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    int n;                      // Общее количество элементов
    vector<T*> chunks;          // Вектор указателей на чанки

public:
    vector2d() : n(0) {
        chunks.push_back(new T[chunk_size]);  // Создаем первый чанк вектора
    }

    vector2d(int _n, T val = T()) : n(_n) {
        int chunk_count = 1 + n / chunk_size;  // Вычисляем общее количество чанков
        int last_chunk_size = n % chunk_size;  // Вычисляем размер последнего чанка
        for (int i = 0; i < chunk_count - 1; i++) {
            chunks.push_back(new T[chunk_size]);              // Создаем новый чанк
            std::fill(chunks[i], chunks[i] + chunk_size, val); // Заполняем чанк значением по умолчанию
        }
        if (last_chunk_size > 0) {
            chunks.push_back(new T[last_chunk_size]);                     // Создаем последний чанк
            std::fill(chunks[chunk_count - 1], chunks[chunk_count - 1] + last_chunk_size, val);  // Заполняем последний чанк значением по умолчанию
        }
    }

    ~vector2d() {
        // Освобождаем память, занятую чанками
        for (auto& chunk : chunks) {
            delete[] chunk;
        }
    }

    T& operator[](int i) {
        int chunk_index = i / chunk_size;    // Вычисляем индекс чанка
        int element_index = i % chunk_size;  // Вычисляем индекс элемента внутри чанка
        return chunks[chunk_index][element_index];  // Возвращаем элемент по указанным индексам
    }

    int size() {
        return n;  // Возвращаем общее количество элементов
    }

    void resize(int newsize, const T& val = T()) {
        int new_chunk_count = 1 + newsize / chunk_size;
        int last_chunk_size = newsize % chunk_size;

        // Удаляем лишние чанки
        while (chunks.size() > new_chunk_count) {
            delete[] chunks.back();
            chunks.pop_back();
        }

        // Создаем новые чанки при необходимости
        while (chunks.size() < new_chunk_count) {
            if (chunks.size() == new_chunk_count - 1 && last_chunk_size > 0) {
                // Создаем последний чанк с размером, соответствующим новому размеру
                chunks.push_back(new T[last_chunk_size]);
                std::fill(chunks.back(), chunks.back() + last_chunk_size, val);
            }
            else {
                // Создаем чанк с размером chunk_size
                chunks.push_back(new T[chunk_size]);
                std::fill(chunks.back(), chunks.back() + chunk_size, val);
            }
        }

        n = newsize;  // Обновляем размер вектора
    }

    void push_back(const T& val) {
        int last_chunk_size = n % chunk_size;

        if (last_chunk_size == 0) {
            // Создаем новый чанк, так как последний чанк полностью заполнен
            chunks.push_back(new T[chunk_size]);
        }

        // Добавляем элемент в последний чанк
        chunks.back()[last_chunk_size] = val;
        n++;  // Увеличиваем размер вектора
    }

    void pop_back() {
        if (n > 0) {
            int last_chunk_size = n % chunk_size;

            // Удаляем последний элемент вектора
            if (last_chunk_size == 1) {
                // Удаляем последний чанк, так как он останется пустым после удаления элемента
                delete[] chunks.back();
                chunks.pop_back();
            }

            n--;  // Уменьшаем размер вектора
        }
    }

    void log() {
        std::cout << "size=" << n << std::endl;                  // Выводим общее количество элементов
        std::cout << "chunks=" << chunks.size() << std::endl;   // Выводим количество чанков
        for (auto p : chunks)
            std::cout << p << ' ';  // Выводим адреса чанков
        std::cout << std::endl;
    }
};

using namespace std;
int main() {
    vector2d<double> v(100, 0.5);
    v.log();
    v.resize(50);
    v.log();
    v.resize(128, 0.1);
    v.log();
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << ' ';
}

```
Теперь у класса vector2d есть методы ```resize```, ```push_back``` и ```pop_back```, которые позволяют изменять размер вектора и добавлять/удалять элементы.

```txt
size=100
chunks=2
00000219F446E8D0 00000219F446B8F0
size=50
chunks=1
00000219F446E8D0
size=128
chunks=3
00000219F446E8D0 00000219F446E620 00000219F446F330
0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1
```



#### 4. Нужно дописать класс итератора на элементы разработанного класса. Реализуй методы сооьветствующие интерфейсу двунаправленного итератора. Добавить в класс методы begin() и end(), возвращающие итераторы на начало и конец последовательности элементов. При реализации использовать представленный шаблон

```cpp
#include <iostream>
#include <vector>
using std::vector;

// Класс итератора для двумерного вектора
template <typename T>
class vector2diter {
private:
    // Константа, определяющая размер блока данных
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    // Итератор блока данных
    typename vector<T*>::iterator chunk_iter;
    // Итератор элемента в блоке данных
    T* elem_iter;

public:
    // Категория итератора
    using iterator_category = std::bidirectional_iterator_tag;
    // Тип значения, на которое указывает итератор
    using value_type = T;
    // Тип разности между итераторами
    using difference_type = std::ptrdiff_t;
    // Тип указателя на значение
    using pointer = T*;
    // Тип ссылки на значение
    using reference = T&;

    // Конструктор итератора
    vector2diter(typename vector<T*>::iterator c_it, T* e_it) : chunk_iter(c_it), elem_iter(e_it) {}

    // Перегруженный оператор инкремента (префиксный)
    vector2diter& operator++() {
        ++elem_iter;
        // Переход к следующему блоку данных, если достигнут конец текущего блока
        if (elem_iter == *chunk_iter + chunk_size) {
            ++chunk_iter;
            elem_iter = *chunk_iter;
        }
        return *this;
    }

    // Перегруженный оператор декремента (префиксный)
    vector2diter& operator--() {
        --elem_iter;
        // Переход к предыдущему блоку данных, если достигнуто начало текущего блока
        if (elem_iter < *chunk_iter) {
            --chunk_iter;
            elem_iter = *chunk_iter + chunk_size - 1;
        }
        return *this;
    }

    // Перегруженный оператор разыменования
    T& operator*() {
        return *elem_iter;
    }

    // Перегруженный оператор равенства
    friend bool operator==(const vector2diter<T>& a, const vector2diter<T>& b) {
        return a.chunk_iter == b.chunk_iter && a.elem_iter == b.elem_iter;
    }

    // Перегруженный оператор неравенства
    friend bool operator!=(const vector2diter<T>& a, const vector2diter<T>& b) {
        return !(a == b);
    }
};

// Класс двумерного вектора
template <typename T>
class vector2d {
private:
    // Константа, определяющая размер блока данных
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    // Количество элементов в векторе
    int n;
    // Вектор указателей на блоки данных
    vector<T*> chunks;

public:
    // Конструктор по умолчанию
    vector2d() : n(0) {
        chunks.push_back(new T[chunk_size]);
    }

    // Конструктор с заданным размером и начальным значением
    vector2d(int _n, const T& val = T()) : n(_n) {
        // Вычисление количества блоков данных и размера последнего блока
        int chunk_count = 1 + n / chunk_size;
        int last_chunk_size = n % chunk_size;
        // Создание блоков данных и заполнение их начальным значением
        for (int i = 0; i < chunk_count - 1; i++) {
            chunks.push_back(new T[chunk_size]);
            std::fill(chunks[i], chunks[i] + chunk_size, val);
        }
        if (last_chunk_size > 0) {
            chunks.push_back(new T[last_chunk_size]);
            std::fill(chunks[chunk_count - 1], chunks[chunk_count - 1] + last_chunk_size, val);
        }
    }

    // Перегруженный оператор индексации
    T& operator[](int i) {
        // Вычисление индекса блока и индекса элемента в блоке
        int chunk_index = i / chunk_size;
        int element_index = i % chunk_size;
        return chunks[chunk_index][element_index];
    }

    // Возвращает количество элементов в векторе
    int size() {
        return n;
    }

    // Изменяет размер вектора
    void resize(int newsize, const T& val = T()) {
        // Вычисление количества новых блоков данных
        int new_chunk_count = 1 + newsize / chunk_size;
        // Добавление или удаление блоков данных в зависимости от нового размера
        if (new_chunk_count > chunks.size()) {
            for (int i = chunks.size(); i < new_chunk_count; i++) {
                if (i == new_chunk_count - 1) {
                    // Создание последнего блока данных с размером, не превышающим chunk_size
                    chunks.push_back(new T[newsize % chunk_size]);
                    std::fill(chunks[i], chunks[i] + newsize % chunk_size, val);
                } else {
                    // Создание блока данных с размером chunk_size
                    chunks.push_back(new T[chunk_size]);
                    std::fill(chunks[i], chunks[i] + chunk_size, val);
                }
            }
        } else if (new_chunk_count < chunks.size()) {
            // Удаление блоков данных, превышающих новое количество блоков
            for (int i = new_chunk_count; i < chunks.size(); i++) {
                delete[] chunks[i];
            }
            chunks.resize(new_chunk_count);
        }
        n = newsize;
    }

    // Добавляет элемент в конец вектора
    void push_back(const T& val) {
        if (n % chunk_size == 0) {
            // Создание нового блока данных, если текущий блок заполнен
            chunks.push_back(new T[chunk_size]);
        }
        // Добавление элемента в текущий блок данных
        chunks.back()[n % chunk_size] = val;
        n++;
    }

    // Удаляет последний элемент из вектора
    void pop_back() {
        if (n > 0) {
            n--;
            // Удаление последнего блока данных, если он стал ненужным
            if (n % chunk_size == 0 && chunks.size() > 1) {
                delete[] chunks.back();
                chunks.pop_back();
            }
        }
    }

    // Возвращает итератор, указывающий на начало вектора
    vector2diter<T> begin() {
        return vector2diter<T>(chunks.begin(), chunks[0]);
    }

    // Возвращает итератор, указывающий на конец вектора
    vector2diter<T> end() {
        if (chunks.empty()) {
            return vector2diter<T>(chunks.begin(), nullptr);
        } else {
            return vector2diter<T>(chunks.end() - 1, chunks.back() + n % chunk_size);
        }
    }

    // Выводит информацию о размере вектора и количестве блоков данных
    void log() {
        std::cout << "size=" << n << std::endl;
        std::cout << "chunks=" << chunks.size() << std::endl;
        for (auto p : chunks)
            std::cout << p << ' ';
        std::cout << std::endl;
    }
};

int main() {
    // Создание и использование двумерного вектора
    vector2d<double> v(100, 0.5);
    v.log();
    v.resize(50);
    v.log();
    v.resize(128, 0.1);
    v.log();
    for (auto& x : v)
        std::cout << x << ' ';
    std::cout << std::endl;

    return 0;
}

```
В данном коде реализованы методы класса ```vector2diter``` в соответствии с требованиями двунаправленного итератора. Класс ```vector2d``` имеет методы ```begin()``` и ```end()```, возвращающие итераторы на начало и конец последовательности элементов.
```txt
size=100
chunks=2
000001EDD953E940 000001EDD953B970
size=50
chunks=1
000001EDD953E940
size=128
chunks=3
000001EDD953E940 000001EDD953E690 000001EDD9536F20
0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1
```

---

#### 5.

```cpp
#include <iostream>
#include <vector>
using std::vector;

template <typename T>
class vector2diter {
private:
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    typename vector<T*>::iterator chunk_iter;
    T* elem_iter;

public:
    using iterator_category = std::bidirectional_iterator_tag;
    using value_type = T;
    using difference_type = std::ptrdiff_t;
    using pointer = T*;
    using reference = T&;

    vector2diter(typename vector<T*>::iterator c_it, T* e_it) : chunk_iter(c_it), elem_iter(e_it) {}

    vector2diter& operator++() {
        // Переход к следующему элементу
        ++elem_iter;
        if (elem_iter == *chunk_iter + chunk_size) {
            // Переход к следующему блоку данных
            ++chunk_iter;
            elem_iter = *chunk_iter;
        }
        return *this;
    }

    vector2diter& operator--() {
        // Переход к предыдущему элементу
        --elem_iter;
        if (elem_iter < *chunk_iter) {
            // Переход к предыдущему блоку данных
            --chunk_iter;
            elem_iter = *chunk_iter + chunk_size - 1;
        }
        return *this;
    }

    T& operator*() {
        // Возвращает ссылку на текущий элемент
        return *elem_iter;
    }

    // Операторы сравнения
    friend bool operator==(const vector2diter<T>& a, const vector2diter<T>& b) {
        return a.chunk_iter == b.chunk_iter && a.elem_iter == b.elem_iter;
    }

    friend bool operator!=(const vector2diter<T>& a, const vector2diter<T>& b) {
        return !(a == b);
    }

    // Операторы сдвига на произвольное значение
    vector2diter& operator+=(difference_type n) {
        // Переход на n позиций вперед
        difference_type pos = elem_iter - *chunk_iter;
        pos += n;
        if (pos >= chunk_size) {
            // Переход к следующему блоку данных
            difference_type chunk_offset = pos / chunk_size;
            chunk_iter += chunk_offset;
            elem_iter = *chunk_iter + (pos % chunk_size);
        }
        else if (pos < 0) {
            // Переход к предыдущему блоку данных
            difference_type chunk_offset = -1 - ((-pos - 1) / chunk_size);
            chunk_iter += chunk_offset;
            elem_iter = *chunk_iter + (chunk_size - (-pos - 1) % chunk_size - 1);
        }
        else {
            // Позиция находится в пределах текущего блока данных
            elem_iter = *chunk_iter + pos;
        }
        return *this;
    }

    vector2diter operator+(difference_type n) const {
        // Создание нового итератора, сдвинутого на n позиций вперед
        vector2diter iter(*this);
        iter += n;
        return iter;
    }

    vector2diter& operator-=(difference_type n) {
        // Переход на n позиций назад
        return *this += -n;
    }

    vector2diter operator-(difference_type n) const {
        // Создание нового итератора, сдвинутого на n позиций назад
        vector2diter iter(*this);
        iter -= n;
        return iter;
    }

    difference_type operator-(const vector2diter& other) const {
        // Вычисление разницы между двумя итераторами
        difference_type pos1 = elem_iter - *chunk_iter;
        difference_type pos2 = other.elem_iter - *other.chunk_iter;
        return (chunk_iter - other.chunk_iter) * chunk_size + (pos1 - pos2);
    }

    T& operator[](difference_type n) const {
        // Возвращает ссылку на элемент, смещенный на n позиций от текущего итератора
        return *(*this + n);
    }

    bool operator<(const vector2diter& other) const {
        // Проверка, находится ли текущий итератор левее other
        return chunk_iter < other.chunk_iter || (chunk_iter == other.chunk_iter && elem_iter < other.elem_iter);
    }

    bool operator>(const vector2diter& other) const {
        // Проверка, находится ли текущий итератор правее other
        return chunk_iter > other.chunk_iter || (chunk_iter == other.chunk_iter && elem_iter > other.elem_iter);
    }

    bool operator<=(const vector2diter& other) const {
        // Проверка, находится ли текущий итератор левее или совпадает с other
        return !(other < *this);
    }

    bool operator>=(const vector2diter& other) const {
        // Проверка, находится ли текущий итератор правее или совпадает с other
        return !(*this < other);
    }
};

template <typename T>
class vector2d {
private:
    static const int chunk_size = (512ULL > sizeof(T) ? 512ULL : sizeof(T)) / sizeof(T);
    int n;
    vector<T*> chunks;

public:
    vector2d() : n(0) {
        chunks.push_back(new T[chunk_size]);
    }

    vector2d(int _n, const T& val = T()) : n(_n) {
        int chunk_count = 1 + n / chunk_size;
        int last_chunk_size = n % chunk_size;
        for (int i = 0; i < chunk_count - 1; i++) {
            chunks.push_back(new T[chunk_size]);
            std::fill(chunks[i], chunks[i] + chunk_size, val);
        }
        if (last_chunk_size > 0) {
            chunks.push_back(new T[last_chunk_size]);
            std::fill(chunks[chunk_count - 1], chunks[chunk_count - 1] + last_chunk_size, val);
        }
    }

    T& operator[](int i) {
        int chunk_index = i / chunk_size;
        int element_index = i % chunk_size;
        return chunks[chunk_index][element_index];
    }

    int size() {
        return n;
    }

    void resize(int newsize, const T& val = T()) {
        int new_chunk_count = 1 + newsize / chunk_size;
        if (new_chunk_count > chunks.size()) {
            for (int i = chunks.size(); i < new_chunk_count; i++) {
                if (i == new_chunk_count - 1) {
                    chunks.push_back(new T[newsize % chunk_size]);
                    std::fill(chunks[i], chunks[i] + newsize % chunk_size, val);
                }
                else {
                    chunks.push_back(new T[chunk_size]);
                    std::fill(chunks[i], chunks[i] + chunk_size, val);
                }
            }
        }
        else if (new_chunk_count < chunks.size()) {
            for (int i = new_chunk_count; i < chunks.size(); i++) {
                delete[] chunks[i];
            }
            chunks.resize(new_chunk_count);
        }
        n = newsize;
    }

    void push_back(const T& val) {
        if (n % chunk_size == 0) {
            chunks.push_back(new T[chunk_size]);
        }
        chunks.back()[n % chunk_size] = val;
        n++;
    }

    void pop_back() {
        if (n > 0) {
            n--;
            if (n % chunk_size == 0 && chunks.size() > 1) {
                delete[] chunks.back();
                chunks.pop_back();
            }
        }
    }

    vector2diter<T> begin() {
        return vector2diter<T>(chunks.begin(), chunks[0]);
    }

    vector2diter<T> end() {
        if (chunks.empty()) {
            return vector2diter<T>(chunks.begin(), nullptr);
        }
        else {
            return vector2diter<T>(chunks.end() - 1, chunks.back() + n % chunk_size);
        }
    }

    void log() {
        std::cout << "size=" << n << std::endl;
        std::cout << "chunks=" << chunks.size() << std::endl;
        for (auto p : chunks)
            std::cout << p << ' ';
        std::cout << std::endl;
    }
};

using namespace std;
int main() {
    vector2d<double> v(100, 0.5);
    v.log();
    v.resize(50);
    v.log();
    v.resize(128, 0.1);
    v.log();
    double d = 0;
    for (auto& x : v)
        x = d += 0.1;
    auto it = lower_bound(v.begin(), v.end(), 5.0);
    cout << *it << endl;
    it += 35;
    cout << *it << endl;
    it += -55;
    cout << *it << endl;
}

```
```txt
chunks=1
0x5555ee35eeb0 
size=128
chunks=3
0x5555ee35eeb0 0x5555ee35f640 0x5555ee35f0c0 
5.1
8.6
3.1
```





--- 

### SRC

[Двухуровневая организация памяти](https://www.youtube.com/watch?v=J1kY7Av5TwM)

[Итераторы для динамического массива с двухуровневой организацие памяти](https://youtu.be/3DToWXBu6Uw)

[Рекомендации по выполнению задания 2](https://youtu.be/JqUBR5tZHro)

[Рекомендации по выполнению задания 3](https://youtu.be/wctt7RRB78w)

[Рекомендации по выполнению задания 4](https://youtu.be/QL4JgPvS1_E)

[Рекомендации по выполнению задания 5](https://youtu.be/K2ked6XTULo)

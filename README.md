# Лекция 5 | Дек

#### 1. Ответьте на следущие вопросы:

+ По функциональности дек в С++ очень похож на вектор, однако их реализации заметно различаются. Объясните почему
1. Структура данных:
    Вектор (std::vector) - это динамический массив, который хранит элементы в непрерывном блоке памяти. Это позволяет обеспечить быстрый доступ к элементам по индексу, константное время для доступа к произвольному элементу и эффективную работу с памятью при увеличении размера контейнера.

    Дек (std::deque) - это контейнер, реализующий двустороннюю очередь. В отличие от вектора, дек состоит из нескольких блоков памяти, называемых чанками, которые связаны между собой. Это позволяет эффективно добавлять и удалять элементы как в начале, так и в конце контейнера.

2. Доступ и операции:
    Вектор обеспечивает эффективный произвольный доступ к элементам, а также быструю вставку/удаление элементов в конец контейнера. Однако, вставка/удаление элементов в середине или в начале вектора требует переноса большого количества элементов, что может быть затратным по времени.

    Дек обеспечивает эффективный доступ к обоим концам контейнера, что позволяет быстро добавлять и удалять элементы как в начале, так и в конце. Вставка/удаление элементов в середине дека также выполняется эффективно. Это делает дек удобным выбором, когда требуется частая вставка/удаление элементов в начале или конце контейнера.

3. Память и ресурсы:
    Вектор использует меньше памяти, чем дек, так как элементы хранятся в непрерывном блоке памяти.
    Дек требует дополнительных затрат на управление блоками памяти и связывание чанков, что может привести к небольшим накладным расходам по памяти и производительности.

Выбор между вектором и деком зависит от конкретных требований и особенностей задачи. Если вставка/удаление элементов происходит главным образом в начале или конце контейнера, или требуется эффективный произвольный доступ к элементам, то дек может быть предпочтительнее. В случае, когда важна эффективность памяти и производительность при операциях с произвольными элементами, вектор может быть лучшим выбором.


+ Стандарс С++ явно не требует выбрать ту или иную реализацию дека, однако, дает недвусмысленный намек, какая из реализаций предполагается. Стандарт требует, чтобы ссылки и указатели на элемент не становились недопустимыми в ходе работы. Как это требование влияет на выбор способа реализации дека? Обоснуйте ответ

Требование стандарта C++ о сохранении допустимости ссылок и указателей на элементы дека влияет на выбор способа реализации дека.

Поскольку дек позволяет эффективно вставлять и удалять элементы как в начале, так и в конце контейнера, одним из возможных способов его реализации является использование двусвязного списка. При использовании двусвязного списка в качестве основной структуры данных для дека, ссылки и указатели на элементы остаются допустимыми в ходе работы с контейнером.

При вставке или удалении элементов в середине дека, двусвязный список позволяет эффективно изменять связи между элементами без необходимости перемещения остальных элементов. Таким образом, дек, реализованный с использованием двусвязного списка, соответствует требованию стандарта C++ о сохранении допустимости ссылок и указателей на элементы.

С другой стороны, если бы для реализации дека использовался векторка или, то встав удаление элементов в середине контейнера может привести к перераспределению всего массива и, как следствие, недопустимости ссылок и указелей наат элементы, которые были получены до перераспределения.

**Таким образом, использование двусвязного списка или подобной структуры данных позволяет удовлетворить требованию стандарта C++ о сохранении допустимости ссылок и указателей на элементы дека в ходе его работы.**

---

#### 2. Напишите программу с использованием дека на C++ и на Python. Последовательность чисел строится по следующему правилу. Изначельно в последовательности дву единицы. Далее, если сумма крайних элементов последовательности делина на 2 или на 3, то сумма добавляется в последовательность справа, иначе слеува. Требуется вывести последовательность из n элементов, где число n подается на вход программы. 

<table><tr><td>


```cpp
#include <iostream>
#include <deque> // Включаем заголовочный файл для использования дека
using namespace std;

deque<int> generateSequence(int n) {
    deque<int> sequence; // Создаем пустой дек для хранения последовательности
    sequence.push_back(1); // Добавляем первую единицу в дек
    sequence.push_back(1); // Добавляем вторую единицу в дек

    while (sequence.size() < n) { // Пока размер последовательности меньше заданного числа n
        int sum = sequence.front() + sequence.back(); // Вычисляем сумму крайних элементов
        if (sum % 2 == 0 || sum % 3 == 0) { // Если сумма делится на 2 или на 3
            sequence.push_back(sum); // Добавляем сумму в конец последовательности
        } else {
            sequence.push_front(sum); // Добавляем сумму в начало последовательности
        }
    }

    return sequence;
}

int main() {
    int n;
    cout << "Enter the number of elements: ";
    cin >> n;

    deque<int> sequence = generateSequence(n); // Генерируем последовательность

    cout << "Sequence of " << n << " elements: ";
    for (int num : sequence) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```


</td><td>


```python
from collections import deque

def generate_sequence(n):
    sequence = deque([1, 1]) # Создаем дек с начальными двумя единицами

    while len(sequence) < n: # Пока размер последовательности меньше заданного числа n
        sum = sequence[0] + sequence[-1] # Вычисляем сумму крайних элементов
        if sum % 2 == 0 or sum % 3 == 0: # Если сумма делится на 2 или на 3
            sequence.append(sum) # Добавляем сумму в конец последовательности
        else:
            sequence.appendleft(sum) # Добавляем сумму в начало последовательности

    return sequence

n = int(input("Enter the number of elements: "))
sequence = generate_sequence(n) # Генерируем последовательность

print(f"Sequence of {n} elements:", end=" ")
for num in sequence:
    print(num, end=" ")
print()
```


</td></tr></table>

Оба варианта программы генерируют последовательность чисел, используя дек для хранения элементов и применяя заданное правило. Разница между программами заключается в синтаксисе и использовании соответствующих функций и методов для работы с деком в каждом языке.

---

#### 3. Необходимо реализовать кольцевой дек в шаблонном классе cdeque. Класс должен содрежать конструктор с одним параметром, задающим размер зарезервивованной памяти, деструктор, методы size(), push_back(x), push_front(x), pop_back(), pop_front(), back(), front() и оператор []. Заменить многотичия на необходимы команды

```cpp
#include <iostream>
#include <algorithm>
#include <exception>

template <typename T>
class cdeque {
private:
    int _reserved;   //объем зарезервированной памяти
    int _size;       //количество элементов в деке
    int _offset;     //смещение (до первого элемента)
    T* _data;        //указатель на массив
    void reallocate(int __reserve);
public:
   cdeque(int _reserve) {		//инициализирующий конструктор
      if (_reserve<1)			//пустой элемент, чтобы итераторы на начало и конец не совпали
         throw std::length_error("reserve must be >0");
      _reserved=_reserve;
      _size=0;
      _offset=0;
      _data=new T [_reserved];
   }


   ~cdeque() {
      delete[] _data; 
   }


   int size() {
      return _size;
   }


   T& operator[](int i) {
      return _data[(_offset + i)%_reserved];
   }


   T& front() {
      return _data[_offset];		//_offset указывает на первый элемент
   }


   T& back() {
      if ((_reserved - _offset) < _size) 		//если конец кольца находится слева от его начала
            return _data[_size - (_reserved - _offset) - 1];
        else 						//если элементы помещаются без цикла по кольцу
            return _data[_offset + _size - 1];
   }


   void push_back(const T &x) {
      if (_size+1==_reserved)
         reallocate(2*_reserved);			//чтобы end() не совпал с begin()
      _size++;

      if (_reserved - _offset < _size)			//если конец кольца находится слева от его начала
          _data[(_size - (_reserved - _offset)) - 1] = x;
      else						//если элементы помещаются без цикла по кольцу
          _data[(_offset + _size) - 1] = x;
   }


   void push_front(const T &x) {
      if (_size+1==_reserved)
         reallocate(2*_reserved);		//чтобы end() не совпал с begin()
      _size++;
      if (_offset == 0) { 			//если первая позиция и есть начало
            _data[_reserved - 1] = x; 		//записываем в конец
            _offset = _reserved - 1; 		//обновляем смещение до первого элемента
      } else {
            _data[_offset - 1] = x; 		//просто пушим в начало
            _offset--; 				//обновляем смещение до первого элемента
      }
      
   }


   void pop_back() {
      if ((_reserved - _offset) < _size) 			//если конец левее начала
            _data[_size - (_reserved - _offset) - 1] = T(); 	//удаляем элемент с конца
        else
            _data[_offset + _size - 1] = T();
        _size--;
   }


   void pop_front() {
      _data[_offset] = T(); 		//_offset указывает на первый элемент, поэтому его легко удалить
        if (_offset == _reserved-1) 	//если он был в конце
            _offset = 0; 		//теперь начало совпадает с фактическим концом
        else
            _offset++; 			//теперь первый элемент следующий по списку
        _size--;
   }
};
template <typename T>
void cdeque<T>::reallocate(int __reserve) {
}

int main() {
  cdeque<int> d(10);
  for (int j=1;j<10;j++) {
     d.push_back(j);
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
  }
  for (int j=0;j<10;j++) {
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
     int x=d.front();
     d.pop_front();
     d.push_back(x);
  } 
  for (int j=0;j<11;j++) {
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
     int x=d.back();
     d.pop_back();
     d.push_front(x);
  } 
  return 0;
}
```
```txt
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
1 2 3 4 5 6
1 2 3 4 5 6 7
1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
3 4 5 6 7 8 9 1 2
4 5 6 7 8 9 1 2 3
5 6 7 8 9 1 2 3 4
6 7 8 9 1 2 3 4 5
7 8 9 1 2 3 4 5 6
8 9 1 2 3 4 5 6 7
9 1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
1 2 3 4 5 6 7 8 9
9 1 2 3 4 5 6 7 8
8 9 1 2 3 4 5 6 7
7 8 9 1 2 3 4 5 6
6 7 8 9 1 2 3 4 5
5 6 7 8 9 1 2 3 4
4 5 6 7 8 9 1 2 3
3 4 5 6 7 8 9 1 2
2 3 4 5 6 7 8 9 1
1 2 3 4 5 6 7 8 9
```

Приведенная программа реализует кольцевой дек (циклический буфер), который поддерживает различные операции добавления и удаления элементов с обоих концов дека, а также доступ к элементам по индексу.

---

#### 4. Добавьте в класс cdeque реализацию метода reallocate. Проверьте работу класса, используя шаблон программы

```cpp
#include <iostream>
#include <algorithm>
#include <exception>

template <typename T>
class cdeque {
private:
    int _reserved;   //объем зарезервированной памяти
    int _size;       //количество элементов в деке
    int _offset;     //смещение (до первого элемента)
    T* _data;        //указатель на массив
    void reallocate(int __reserve);
public:
   cdeque(int _reserve) {		//инициализирующий конструктор
      if (_reserve<1)			//пустой элемент, чтобы итераторы на начало и конец не совпали
         throw std::length_error("reserve must be >0");
      _reserved=_reserve;
      _size=0;
      _offset=0;
      _data=new T [_reserved];
   }


   ~cdeque() {
      delete[] _data; 
   }


   int size() {
      return _size;
   }


   T& operator[](int i) {
      return _data[(_offset + i)%_reserved];
   }


   T& front() {
      return _data[_offset];		//_offset указывает на первый элемент
   }


   T& back() {
      if ((_reserved - _offset) < _size) 			//если конец кольца находится слева от его начала
            return _data[_size - (_reserved - _offset) - 1];
        else 										//если элементы помещаются без цикла по кольцу
            return _data[_offset + _size - 1];
   }


   void push_back(const T &x) {
      if (_size+1==_reserved)
         reallocate(2*_reserved);				//чтобы end() не совпал с begin()
      _size++;

      if (_reserved - _offset < _size)			//если конец кольца находится слева от его начала
          _data[(_size - (_reserved - _offset)) - 1] = x;
      else										//если элементы помещаются без цикла по кольцу
          _data[(_offset + _size) - 1] = x;
   }


   void push_front(const T &x) {
      if (_size+1==_reserved)
         reallocate(2*_reserved);			//чтобы end() не совпал с begin()
      _size++;

      if (_offset == 0) { 					//если первая позиция и есть начало
            _data[_reserved - 1] = x; 		//записываем в конец
            _offset = _reserved - 1; 		//обновляем смещение до первого элемента
      } else {
            _data[_offset - 1] = x; 		//просто пушим в начало
            _offset--; 						//обновляем смещение до первого элемента
      }
      
   }


   void pop_back() {
      if ((_reserved - _offset) < _size) 						//если конец левее начала
            _data[_size - (_reserved - _offset) - 1] = T(); 	//удаляем элемент с конца
        else
            _data[_offset + _size - 1] = T();
        _size--;
   }


   void pop_front() {
      _data[_offset] = T(); 				//_offset указывает на первый элемент, поэтому его легко удалить
        if (_offset == _reserved-1) 		//если он был в конце
            _offset = 0; 					//теперь начало совпадает с фактическим концом
        else
            _offset++; 						//теперь первый элемент следующий по списку
        _size--;
   }
};

template <typename T>
void cdeque<T>::reallocate(int __reserve) {

    T *new_deque = new T[__reserve];	//выделяем новую область памяти по параметру
    int current = _offset;
    for (int i = 0; i < _size; i++) { 	//переносим элементы в начало
        new_deque[i] = _data[current];
        if (current == (_reserved - 1)) //если дошли до конца старого дека
            current = 0; 				//переходим в начало старого дека
        else
            current++;
    }
    _reserved = __reserve; 		//новый размер
    _offset = 0; 				//т.к. данные теперь идут с начала
    delete[] _data; 			//освобождаем память, занятую старым деком
    _data = new_deque; 			//перезаписываем новый дек
}

int main() {
  cdeque<int> d(1);
  for (int j=1;j<10;j++) {
     d.push_back(j);
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
  }
  for (int j=0;j<10;j++) {
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
     int x=d.front();
     d.pop_front();
     d.push_back(x);
  } 
  for (int j=0;j<11;j++) {
     for (int i=0;i<d.size();i++)
        std::cout<<d[i]<<' ';
     std::cout<<std::endl;
     int x=d.back();
     d.pop_back();
     d.push_front(x);
  } 
  cdeque<int> e(1);
  for (int j=1;j<=10;j++) {
     if (j&1)
        e.push_back(j);
     else
        e.push_front(j);
     for (int i=0;i<e.size();i++)
        std::cout<<e[i]<<' ';
     std::cout<<std::endl;
  }
  return 0;
}
```
```txt
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
1 2 3 4 5 6
1 2 3 4 5 6 7
1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
3 4 5 6 7 8 9 1 2
4 5 6 7 8 9 1 2 3
5 6 7 8 9 1 2 3 4
6 7 8 9 1 2 3 4 5
7 8 9 1 2 3 4 5 6
8 9 1 2 3 4 5 6 7
9 1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 8 9
2 3 4 5 6 7 8 9 1
1 2 3 4 5 6 7 8 9
9 1 2 3 4 5 6 7 8
8 9 1 2 3 4 5 6 7
7 8 9 1 2 3 4 5 6
6 7 8 9 1 2 3 4 5
5 6 7 8 9 1 2 3 4
4 5 6 7 8 9 1 2 3
3 4 5 6 7 8 9 1 2
2 3 4 5 6 7 8 9 1
1 2 3 4 5 6 7 8 9
1
2 1
2 1 3
4 2 1 3
4 2 1 3 5
6 4 2 1 3 5
6 4 2 1 3 5 7
8 6 4 2 1 3 5 7
8 6 4 2 1 3 5 7 9
10 8 6 4 2 1 3 5 7 9
```

В данном коде добавлена реализация метода reallocate для программы из предыдущего задания.

---

#### 5. Реализовать итератор произвольного доступа для класса cdeque. Добавить в дек методы begin и end. Проверить работу класса, используя шаблон программы, который представлен ниже 

```cpp
#include <iostream>
#include <algorithm>
#include <exception>

template <typename T>
class cdeque_iterator {
private:
   T *_data;
   int _reserved;
   int _offset;
   int _pos;
public:
   cdeque_iterator(T* __data, int __reserved, int __offset, int __pos) :
      _data(__data), _reserved(__reserved), _offset(__offset), _pos(__pos) {}

   cdeque_iterator<T>& operator++() {
      _pos = (_pos + 1) % _reserved;  // Инкрементируем позицию
      return *this;
   }

   cdeque_iterator<T>& operator--() {
      _pos = (_pos - 1 + _reserved) % _reserved;  // Декрементируем позицию
      return *this;
   }

   T& operator*() {
      int index = (_offset + _pos) % _reserved;  // Вычисляем индекс элемента
      return _data[index];  // Возвращаем ссылку на элемент
   }
  
   friend bool operator==(const cdeque_iterator<T> &a, const cdeque_iterator<T> &b) {
      return (a._data == b._data && a._pos == b._pos);
   }

   friend bool operator!=(const cdeque_iterator<T> &a, const cdeque_iterator<T> &b) {
      return !(a == b);
   }

   friend bool operator<(const cdeque_iterator<T> &a, const cdeque_iterator<T> &b) {
      return (a._pos < b._pos);
   }

   cdeque_iterator& operator+=(int k) {
      _pos = (_pos + k) % _reserved;  // Смещаем позицию на k элементов
      return *this;
   }

   friend cdeque_iterator operator+(const cdeque_iterator<T> &a, int k) {
      cdeque_iterator<T> other(a);
      return other += k;
   }

   friend cdeque_iterator operator-(const cdeque_iterator<T> &a, int k) {
      cdeque_iterator<T> other(a);
      return other += -k;
   }

   friend int operator-(const cdeque_iterator<T> &a, const cdeque_iterator<T> &b) {
      return (a._pos - b._pos + a._reserved) % a._reserved;  // Вычисляем разницу позиций
   }
};

template <typename T>
class cdeque {
private:
   int _reserved;
   int _size;
   int _offset;
   T* _data;
   void reallocate(int __reserve);
public:
   cdeque(int _reserve) {
      if (_reserve<1)
         throw std::length_error("reserve must be >0");
      _reserved=_reserve;
      _size=0;
      _offset=0;
      _data=new T [_reserved];
   }
   ~cdeque() {
      delete[] _data; 
   }
   int size() {
      return _size;
   }
   T& operator[](int i) {
      int index = (_offset + i) % _reserved;  // Вычисляем индекс элемента
      return _data[index];  // Возвращаем ссылку на элемент
   }
   T& front() {
      return _data[_offset];
   }
   T& back() {
      int index = (_offset + _size - 1) % _reserved;  // Вычисляем индекс последнего элемента
      return _data[index];  // Возвращаем ссылку на последний элемент
   }
   void push_back(const T &x) {
      if (_size + 1 == _reserved)
         reallocate(2 * _reserved);
      
      int index = (_offset + _size) % _reserved;  // Вычисляем индекс для добавления элемента
      _data[index] = x;  // Добавляем элемент в конец дека
      _size++;  // Увеличиваем размер дека
   }
   void push_front(const T &x) {
      if (_size + 1 == _reserved)
         reallocate(2 * _reserved);
      
      _offset = (_offset - 1 + _reserved) % _reserved;  // Обновляем смещение _offset
      _data[_offset] = x;  // Добавляем элемент в начало дека
      _size++;  // Увеличиваем размер дека
   }
   void pop_back() {
      if (_size == 0)
         throw std::out_of_range("deque is empty");

      _size--;  // Уменьшаем размер дека
   }
   void pop_front() {
      if (_size == 0)
         throw std::out_of_range("deque is empty");

      _offset = (_offset + 1) % _reserved;  // Увеличиваем смещение _offset
      _size--;  // Уменьшаем размер дека
   }
   cdeque_iterator<T> begin() {
      return cdeque_iterator<T>(_data, _reserved, _offset, 0);  // Возвращаем итератор на начало дека
   }
   cdeque_iterator<T> end() {
      return cdeque_iterator<T>(_data, _reserved, _offset, _size);  // Возвращаем итератор на конец дека
   }
};

template <typename T>
void cdeque<T>::reallocate(int __reserve) {
   T* new_data = new T[__reserve];  // Создаем новый массив с новым размером

   for (int i = 0; i < _size; i++) {
      int index = (_offset + i) % _reserved;  // Вычисляем индекс элемента
      new_data[i] = _data[index];  // Копируем элементы из старого массива в новый
   }

   delete[] _data;  // Освобождаем память старого массива

   _data = new_data;  // Обновляем указатель на новый массив
   _reserved = __reserve;  // Обновляем значение _reserved
}

int main() {
   cdeque<int> d(1);
   for (int j = 1; j < 10; j++) {
      d.push_back(j);
      for (int i = 0; i < d.size(); i++)
         std::cout << d[i] << ' ';
      std::cout << std::endl;
   }
   for (int j = 0; j < 10; j++) {
      for (int i = 0; i < d.size(); i++)
         std::cout << d[i] << ' ';
      std::cout << std::endl;
      int x = d.front();
      d.pop_front();
      d.push_back(x);
   }
   for (int j = 0; j < 11; j++) {
      for (int i = 0; i < d.size(); i++)
         std::cout << d[i] << ' ';
      std::cout << std::endl;
      int x = d.back();
      d.pop_back();
      d.push_front(x);
   }
   cdeque<int> e(1);
   for (int j = 1; j <= 10; j++) {
      if (j & 1)
         e.push_back(j);
      else
         e.push_front(j);
      for (int i = 0; i < e.size(); i++)
         std::cout << e[i] << ' ';
      std::cout << std::endl;
   }
   std::sort(e.begin(), e.end());
   for (int i = 0; i < e.size(); i++)
      std::cout << e[i] << ' ';
   std::cout << std::endl;
   return 0;
}
```

---

#### 6. Используя класс аллокатора с логом действия, проверить работу класса std::deque, а именно, как и когда происходит выделение памяти под указатели, при каких условиях указатели перемещаются по существующей памяти, а не переносятся во вновь выделенный участок. Описать алгоритм работы дека с массивом указателей

```cpp
#include <iostream>
#include <deque>
#include <vector>

template <class T>
struct myallocator {
    typedef T value_type;
    myallocator() {};
    template <typename U>
    myallocator(const myallocator<U>&) {};
    T* allocate(std::size_t n) {
        std::cout << "\nallocating " << n << " elements\n";
        return new T[n];
    }
    void deallocate(T* p, std::size_t n) {
        std::cout << "\ndeallocating " << n << " elements\n";
        delete[] p;
    }
};
template <class T, class U>
bool operator==(const myallocator<T>&, const myallocator<U>&) { return true; }
template <class T, class U>
bool operator!=(const myallocator<T>&, const myallocator<U>&) { return false; }

using namespace std;

void test1(int n, int m) {
    deque<int, myallocator<int>> d;
    cout << "\npushing\n";
    for (int i = 1; i <= n; i++) {
        cout << i << ' ';
        d.push_back(i);
    }
    cout << "\npush and pop\n";
    for (int i = 1; i <= m; i++) {
        cout << i << ' ';
        d.push_back(d.front());
        d.pop_front();
    }
}

void test2(const vector<int>& len) {
    deque<int, myallocator<int>> d;
    for (auto ln : len) {
        cout << "len=" << ln << endl;
        d.resize(ln, 0);
    }
}


int main() {
    test1(200, 10000);
    test2({ 10,20,30,100,200,300,1000,2000,5000,10000,20000 });
    return 0;
}
```

Данный код содержит две функции для тестирования класса ```std::deque``` с аллокатором ```myallocator```, который ведет лог действий при выделении и освобождении памяти.

Функция ```test1``` создает дек ```d``` с аллокатором ```myallocator<int>``` и заполняет его значениями от ```1``` до ```n``` с помощью операции ```push_back```. Затем происходит m итераций, в каждой из которых первый элемент дека перемещается в конец с помощью ```push_back```, а затем удаляется с помощью ```pop_front```. Цель этой функции - проверить, как и когда происходит выделение и освобождение памяти в деке при добавлении и удалении элементов.

Функция ```test2``` создает пустой дек ```d``` с аллокатором ```myallocator<int>```. Затем происходит цикл по вектору ```len```, в котором каждый элемент ```ln``` используется для изменения размера дека с помощью операции ```resize```. Цель этой функции - проверить, как и когда происходит выделение и освобождение памяти в деке при изменении его размера.

В ```main``` функции вызываются обе тестовые функции с различными параметрами для проверки работы дека с аллокатором ```myallocator``` и выводится лог действий при выделении и освобождении памяти.

Программа выводит информацию о выделении и освобождении памяти, чтобы показать, как и когда происходят эти операции при использовании ```std::deque``` с пользовательским аллокатором.

---

При добавлении элементов в дек с помощью операции ```push_back```, выделение памяти происходит следующим образом:

При добавлении первого элемента в пустой дек выделяется блок памяти достаточного размера для хранения этого элемента.

При добавлении каждого последующего элемента проверяется, достаточно ли места в выделенном блоке для хранения нового элемента. Если места достаточно, новый элемент помещается в существующий блок.

Если места в выделенном блоке недостаточно для добавления нового элемента, дек выделяет новый блок памяти большего размера, копирует все существующие элементы в новый блок и добавляет новый элемент в конец нового блока. При этом происходит освобождение предыдущего блока памяти.

Таким образом, выделение памяти происходит при необходимости увеличения размера блока, а освобождение памяти происходит при переносе элементов в новый блок и при явном освобождении дека.

При удалении элементов из дека с помощью операции ```pop_front```, освобождение памяти происходит следующим образом:

При удалении первого элемента из дека освобождения памяти не происходит, так как блок, содержащий первый элемент, все еще используется для хранения остальных элементов.

При удалении каждого последующего элемента дека, если блок, содержащий этот элемент, больше не содержит других элементов, освобождается соответствующий блок памяти.

Таким образом, освобождение памяти происходит при удалении элементов, если блоки становятся пустыми и больше не нужны для хранения элементов.







--- 

### SRC

[Дек в С++ и Python](https://youtu.be/4ffpSIxj5AA)

[Способы реализации дека](https://youtu.be/8up4wiEWBOQ)

[Рекомендации по выполнению заданий](https://youtu.be/TxdbdWXhcZ0)

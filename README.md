# Лекция 3 | Массивы и вектора

#### 1. Ответьте на следующие вопросы:

+ Почему в классе ```std::array``` итераторы и ссылки на элементы всегда допустимы, а в классе ```std::vector``` - нет? 

Причина заключается в способе реализации этих двух классов.

```std::array``` - это контейнер, который хранит свои элементы в непрерывном блоке памяти. Это означает, что адреса памяти его элементов гарантированно последовательны, и поэтому итераторы и ссылки на элементы всегда допустимы, пока массив не был изменен или уничтожен.

```std::vector``` - это контейнер, управляющий динамически выделенным массивом. При добавлении или удалении элементов из вектора массив может потребоваться перевыделить, а элементы переместить в новое местоположение в памяти. Это означает, что адреса памяти элементов вектора могут изменяться при изменении вектора, что может привести к недопустимости итераторов и ссылок на элементы.

Для решения этой проблемы std::vector предоставляет методы-члены, такие как ```reserve()``` и ```shrink_to_fit()```, которые могут использоваться для управления емкостью вектора и уменьшения вероятности перевыделения. Кроме того, вектор предоставляет методы-члены, такие как ```emplace_back()``` и ```erase()```, которые возвращают допустимые итераторы, даже после изменения вектора. Однако, в общем случае, важно быть осторожным при использовании итераторов и ссылок на элементы в ```std::vector```, и учитывать возможность их недопустимости.

+ Почему массивы всегда хранят элементы одного типа? 

Массивы в C++ всегда хранят элементы одного типа из-за способа их реализации в памяти. При объявлении массива выделяется непрерывный блок памяти для хранения всех элементов массива. Поскольку каждый элемент занимает одинаковое количество памяти, компилятор должен знать размер каждого элемента, чтобы вычислить адрес каждого элемента в блоке памяти.

Если бы элементы разных типов размещались в одном массиве, компилятор не смог бы правильно вычислить адрес каждого элемента. Это связано с тем, что элементы разных типов могут иметь разные размеры и выравнивания, которые могут повлиять на расположение блока памяти. В результате массивы в C++ всегда однородные, то есть все элементы должны быть одного типа.

+ Предположим, что в программе вы собираетесь использовать достаточно много динамических массивов, причем их размер в ходе работы программы может сильно изменяться как в сторону увеличения, так и в сторону уменьшения. Стоит ли в этом случае использовать класс ```std::vector``` для работы? 

Не стоит, так как вектор никогда не уменьшает размер зарезервированной памяти 

---

#### 2. В работе по изучению времени сортировки на ```C++``` замените ```vector``` на ```array``` и сравните время работы. Сделайте выводы

<table>
<tr>
<td>


#### Vector
```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include "testlib.h"

using namespace std;

int main(){

	ios_base::sync_with_stdio(false);
	cin.tie(0);cout.tie(0);

	string fo, name;
	int groups;
	ifstream fin("param.txt");
	fin>> fo >> name >> groups;
	setName(name.c_str());
	ofstream fout(fo);
	fout << groups << '\n';

	vector<int> t(groups);
	for(int i = 0; i < groups; i++)
		fin >> t[i];

	vector<int> d1(groups);
	for(int i = 0; i < groups; i++)
		fin >> d1[i];

	int a,b;
	fin >> a >> b;
	set<int> s;

	for(int i = 0; i < t.size(); i++){
		fout << t[i] << ' ' << d1[i] << '\n';
		for(int j=0; j<t[i]; j++){
			while(s.size() < d1[i])
				s.insert(rnd.next(a,b));
			vector<int> shf(s.size());
			shf.assign(s.begin(), s.end());
			s.clear();
			shuffle(shf.begin(), shf.end());
			for(auto q:shf)
				fout << q << ' ';
			fout << '\n';
		}
	}

}
```


</td>
<td>


#### Array
```cpp
#include <iostream>
#include <chrono>
#include <fstream>
#include <vector>
#include <set>
#include <algorithm>
#include <list>

using namespace std;

double sort1(vector<int> V){
  auto t1=chrono::high_resolution_clock::now();
  sort(V.begin(), V.end());
  auto t2=chrono::high_resolution_clock::now();
  chrono::duration<double> dur=t2-t1;
  return dur.count();
}



int main(){
  ios_base::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);

  ifstream fin("output.txt");
  ofstream fout("protocol.txt");

  int groups, tests, dl;
  fin>>groups;
  int h=0;
  double t1,t2,t3;

  for(int i=0; i<groups; i++){
    fin>>tests>>dl;

    vector<int> v(dl);
    vector<double> sr(5);
    vector<double> mini(5*tests);
    int n=0;

    for(int j=0; j<tests; j++){

      for(int k=0; k<dl; k++)
        fin>>v[k];
      t1=sort1(v);
      t2=sort1(v);
      t3=sort1(v);
      mini[n]=min({t1,t2,t3});

      
      n+=5;
    }
   for(int i=0; i<5; i++){
     h=i;
     for(h; h<mini.size(); h+=5)
     	sr[i]+=mini[h];
     sr[i]/=tests;
   }
   fout<<dl<<' '<<' '<<sr[0]<<'\n';

}
  return 0;
}
```


</tr>
<tr>
<td>


```txt
VECTOR

400000 0.103451
```


</td>
<td>


```txt
ARRAY

400000 0.5211291
```


</td>
</tr>
</table>


**```std::vector``` и ```array``` могут различаться в скорости выполнения. В целом, std::vector является динамическим контейнером, который может изменять свой размер во время выполнения программы, тогда как array является статическим контейнером с фиксированным размером, указанным на этапе компиляции. Поскольку std::vector должен выделять и освобождать память динамически, он может быть медленнее, чем array в некоторых случаях.**

---

#### 3. В работе по изучению времени сортировки на ```Python``` замените ```list``` на ```ndarray``` и сравните время работы. Сортировку следует выполнять методом ```numpy.sort```

<table>
<tr>
<td>


#### list
```python
import time
def sort1(a):
  a_copy = a.copy()
  t = time.time()
  sorted(a_copy)
  return time.time() - t
with open('output.txt','r') as fin:
  group = fin.readline()
  for i in range(int(group)):
    tests, dl = map(int,fin.readline().split(' '))
    sum = 0
    for k in range(tests):
      v = fin.readline().split()
      v = [int(elem) for elem in v]
      t1 = sort1(v)
      t2 = sort1(v)
      t3 = sort1(v)
      sum += min(t1,t2,t3)
    print(dl, end=' ')
    print(sum/tests)
```


</td>
<td>


#### ndarray
```python
import time
import numpy as np
def sort1(a):
  a_copy = a.copy()
  t = time.time()
  np.sort(a)
  return time.time() - t
with open('output.txt','r') as fin:
  group = fin.readline()
  for i in range(int(group)):
    tests, dl = map(int,fin.readline().split(' '))
    sum = 0
    for k in range(tests):
      num = np.ndarray([dl])
      count = 0
      while count < dl:
          line = list(map(int, fin.readline().split()))
          for x in line:
              num[count] = x
              count += 1
      t1 = sort1(num)
      t2 = sort1(num)
      t3 = sort1(num)
      sum += min(t1,t2,t3)
    print(dl, end=' ')
    print(sum/tests)
```


</tr>
<tr>
<td>


```txt
list:

400000 0.06012598673502604

```


</td>
<td>


```txt
ndarray:

400000 0.024288098017374676

```


</td>
</tr>
</table>

**Разница в скорости получается из-за того, что ndarray не определяет тип каждого элемента, так как он для всех одинаковый, это ускоряет доступ к элементам**

---

#### 4. Реализуйте на ```Python``` функцию спуска до нуля с использованием библиотеки ```numpy```

```python
import numpy as np

def zero(matrix):
    ox = np.array(matrix.min(axis = 1)) # 	минимум в строке 
    matrix -= ox[:, np.newaxis] # 		вычли матрицу столбцов
    oy = np.array(matrix.min(axis = 0)) # 	минимум в столбце
    matrix -= oy[np.newaxis, :]
    return matrix

a = np.array([[3,5,7], [2,4,6], [1,8,9]])
print(zero(a))

# [[0 0 0]
#  [0 0 0]
#  [0 5 4]]
	
	
# ox - массив или матрица.
# np.array() - функция из библиотеки NumPy, которая преобразует входные данные в массив.
# matrix - исходная матрица, предположительно двумерный массив.
# .min(axis=1) - метод min(), примененный к матрице matrix с параметром axis=1, который указывает на вычисление минимального значения по каждой строке. Это означает, что минимальное значение будет возвращено для каждой строки, и результат будет одномерным массивом.
# .min(axis=0) - метод min(), примененный к матрице matrix с параметром axis=0, который указывает на вычисление минимального значения по каждому столбцу. Это означает, что минимальное значение будет возвращено для каждого столбца, и результат будет одномерным массивом.
# В итоге, np.array(matrix.min(axis=1)) создает новый одномерный массив, содержащий минимальные значения по каждой строке из исходной матрицы matrix.
# : - оператор среза, который выбирает все элементы по определенной оси. В данном случае он выбирает все элементы по всем осям массива ox.
# np.newaxis - константа в библиотеке NumPy, которая представляет добавление новой оси.
```
---

#### 5. Создайте вектор, используя, аллокатор из примера в лекции. Проверьте, при каких условиях происходит реаллокация элементов и как определяется размер выделяемой области. Запилите алгоритм определния размера резервируемой памяти при выполнении метода ```resize```

```cpp
#include <vector>
#include <iostream>

template <class T>
struct myallocator {
    typedef T value_type;
    myallocator() {};
    myallocator(const myallocator<T> &) {};

    T* allocate(std::size_t n) {
        std::cout << "allocating " << n << " elements\n";
        return new T[n]; //выделение памяти
    }

    void deallocate(T* p, std::size_t n) {
        std::cout << "deallocating " << n << " elements\n";
        delete[] p; //освобождение памяти
    }
};

template <class T, class U>
bool operator==(const myallocator<T>&, const myallocator<U>&) {
    return true;
}
template <class T, class U>
bool operator!=(const myallocator<T>&, const myallocator<U>&) {
    return false;
}

int main() {
    std::vector<int, myallocator<int>> V,X;
    std::cout<<"resize 50:\n";
    V.resize(50,1);
    std::cout<<"resize 75:\n";
    V.resize(75,1);
    std::cout<<"resize 140:\n";
    V.resize(140,1);
    std::cout<<"resize 500:\n";
    V.resize(500,1);
    std::cout<<"resize 100:\n\n";
    V.resize(100);
    for (int i=0; i<100; i++) {
        std::cout<<i<<":";
        X.push_back(i);
    }
    return 0;
}

```

```txt
resize 50:
allocating 50 elements
resize 75:
allocating 100 elements
deallocating 50 elements
resize 140:
allocating 150 elements
deallocating 100 elements
resize 500:
allocating 500 elements
deallocating 150 elements
resize 100:

0:allocating 1 elements
1:allocating 2 elements
deallocating 1 elements
2:allocating 4 elements
deallocating 2 elements
3:4:allocating 8 elements
deallocating 4 elements
5:6:7:8:allocating 16 elements
deallocating 8 elements
9:10:11:12:13:14:15:16:allocating 32 elements
deallocating 16 elements
17:18:19:20:21:22:23:24:25:26:27:28:29:30:31:32:allocating 64 elements
deallocating 32 elements
33:34:35:36:37:38:39:40:41:42:43:44:45:46:47:48:49:50:51:52:53:54:55:56:57:58:59:60:61:62:63:64:allocating 128 elements
deallocating 64 elements
65:66:67:68:69:70:71:72:73:74:75:76:77:78:79:80:81:82:83:84:85:86:87:88:89:90:91:92:93:94:95:96:97:98:99:deallocating 128 elements
deallocating 500 elements


...Program finished with exit code 0
Press ENTER to exit console.
```

Размер выделяемой области определяется следующим образом:

1. Если размер вектора уменьшается, то реаллокации не происходит, т.к. размер резервируемой памяти не изменяется в меньшую сторону
2. Если новый размер вектора помещается в уже зарезервированную память, то реаллокации не происходит
3. Если возникает ситуация, что новый размер вектора превышает резерв старого, то в зависимости от соотношения размера старого (кол-во элементов) $n$ к размеру нового $N$, происходит аллокация новой памяти и деаллокация старой по следующему принципу:

+ Если $2 \times n >= N$, то память выделяется на $2 \times n$ элементов
+ Если же $2 \times n < N$, то память выделяется уже на $N$ элементов

---


#### 6. Разработайте шаблонный класс, близкий по возможностям к ```std::vector```. В качестве итераторов в нем можно использовать указатели. Требуется реализовать методы ```resize```, ```begin```, ```end```, ```push_back```, ```pop_back```, конструкторы перемещения и копирования, оператор индексации. При реаллокации элементов не забывайте использовать функцию ```move```.

```cpp
template <typename T>
class Vetcor {
public:
	Vector() { // Конструктор класса
		arr_ = new T[1];
		capacity_ = 1;
	}

private:
	T* arr_; // Указатель на область памяти, где будет находиться массив
	size_t size_{}; // Размер вектора
	size_t capacity_{}; // Максимальный размер вектора

	void add_new_memory() { // Выделение новой памяти
		capacity_ *= 2;
		T* tmp = arr_;
		arr_ = new T[capacity_];
		for (size_t i = 0; i < size_; ++i) arr_[i] = tmp[i];
		delete[] tmp;
	}

	void push_back(const T& value) { // push_back
		if (size_ >= capacity_) add_new_memory();
		arr_[size_++] = value;
	}

	void pop_back() {
        if (size_ > 0) {
            size_--;
        }
    }
/*
	void resize(size_t new_size) {
        if (new_size > capacity) {
            capacity = new_size;
            T* new_data = new T[capacity];
            std::move(begin(), end(), new_data);
            delete[] data;
            data = new_data;
        }
        size_ = new_size;
    }
	*/
	
	void resize(size_t newSize) {
        if (newSize <= size_) {
            size_ = newSize;
        } else {
            double ratio = static_cast<double>(newSize) / size_;
            
            if (ratio <= 2.0 && newSize <= capacity) {
                size_ = newSize;
            } else {
                capacity = static_cast<size_t>(ratio * newSize);
                T* newData = new T[capacity];
                
                std::copy(data, data + size, newData);
                
                delete[] data;
                
                data = newData;
                size = newSize;
            }
        }
    }
}

	T* begin() { // begin обычная ссылка
		return &arr_[0];
	}

	T* end() { // end обычная ссылка
		return &arr_[size_];
	}

	T& operator[](size_t index)const { // Оператор индексации
		return arr_[index];
	} 
	// Операторы присваивания с семантикой перемещения и копирования

	Vector(Vector& other) {
		if (this != &other) {
			delete[] arr_;
			arr_ = new T[other.capacity_];
			for (size_t i = 0; i < other.size_; ++i) arr_[i] = other.arr_[i];
			size_ = other.size_;
			capacity_ = other.capacity_;
		}
	}
	Vector(Vector&& other)  noexcept {
		if (this != &other) {
			delete[] arr_;
			arr_ = other.arr_;
			size_ = other.size_;
			capacity_ = other.capacity_;
			other.arr_ = nullptr;
			other.size_ = other.capacity_ = 0;
		}
	}

	~Vector() {
		delete[] arr_;
	}
};

```

---


#### SRC

[Массивы в С++ и Python](https://www.youtube.com/watch?v=bdR4BiG1oQw)

[Выделение памяти](https://youtu.be/2-1XMTOayyo)

[Вектора](https://youtu.be/3eC60ClekNQ)

[Рекомендации по выполнению заданий 1-4](https://youtu.be/GeLgzmQ7bWk)

[Рекомендации по выполнению заданий 5-6](https://youtu.be/e7mB-wJjPiE)

---

[vector класс | Microsoft Learn](https://learn.microsoft.com/ru-ru/cpp/standard-library/vector-class?view=msvc-170)

[Vector in C++ Standard Template Library (STL) with Example](https://www.guru99.com/cpp-vector-stl.html)

[C++ | STL | Библиотека стандартных шаблонов](https://youtube.com/playlist?list=PLQOaTSbfxUtDWAtIYme5MLZ1l0GTyUYkB)

[C++. Класс vector. Динамический массив. Общие сведения](https://www.bestprog.net/ru/2021/10/10/c-the-vector-class-dynamic-array-general-information-ru/)

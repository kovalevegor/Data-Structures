# Лекция 1 | Техника работы

#### 1. В этом задании требуется научиться использовать компилятор C++ из командной строки или некоторого файлового менеджера.

- Напишите программу A + B на C++ в редакторе Far Manager. Можно использовать любой базовый текстовый редактор
- Выполните трансляцию программы из командной строки. Для упрощения работы можно использовать bat-файл
- Запустите программу из командной строки и проверьте ее работоспособность
- В отсчет вставьте текст программы и команды ее трансляции и запуска

```
Внимание. При проверке этого зания вам необходимо будет проедлать все эти действия в аудиториию При этом будет учитываться время выполнения задания. Задание будет засчитано, если время его выполнения не превысит две минуты
```

[testlib.h](https://github.com/MikeMirzayanov/testlib/blob/master/testlib.h)

### **Компилятор g++ флаги, оптимизация, сборка**

**Стандарты С++**

* ```-std=c++98``` - C++98
* ```-std=c++11``` - C++11
* ```-std=c++14``` - C++14
* ```-std=c++17``` - C++17
* ```-std=c++2a``` - C++20

**Предупреждения**

```-Wall``` - Выводит большинство предупреждений

```-Wfloat-equal``` - Предупреждает о не безопасном сравнении

```-Wsign-conversion``` или ```-Wsign-promo``` - Предупреждения преобразования signed в unsigned (и обратно)

```-Wold-style-cast``` - Выводит C Style преобразования типов

```-Warray-bounds``` - Доступ за пределы массива

```-Wdiv-by-zero``` - Предупреждать о делении на 0

```-Wdouble-promotion``` - Предупреждает о преобразовании с ```float``` на ```double```

```-Wbool-compire``` - Предупреждение о сравнении ```int``` с ```bool```


**Сборка**

Для сборки программы необходимо указать компилятору g++ файлы исходного кода, например команда g++ `main.cpp` скомпилирует исходный код файла `main.cpp` в исполняемый фаил `a.out` _(если компилятору не указать имя выходного файла то по умолчанию именем будет `a.out`)_

`-o <name>` - Имя выходного файла

**Пример:** Команда `g++ -o myexe` `main.cpp` скомпилирует фаил `main.cpp` в исполняемый фаил `myexe`.

Можно передавать несколько исходных файлов для сборки, например `g++ -o myexe` `file1.cpp file2.cpp`.

-c - Создание объектного файла

**Пример:** Для создания объектного файла необходимо указать компилятору ключи `-c и -o: g++ -c -o main.o` `main.cpp`, данной командой компилятор `g++` создает объектный фаил main.o из файла `main.cpp`

Для сборки программы из объектных файлов необходимо указать компилятору в качестве входных параметров не файлы исходного кода а объектные файлы: `g++ -o myexe` foo.o main.o bar.o - создает программу из объектных файлов foo.o main.o bar.o

-I<include_path> - Указание каталога для поиска подключаемых файлов

**Пример:** `g++ -o myexe` ` -I/my/path/to/include ``````main.cpp `

`-L<library_path>` - Указание каталога для поиска библиотек

`-l<library>` - Указание конкретной библиотеки для линковки

### **Компиляция программы на языке C++/CLI из командной строки**

[Пошаговое руководство. Компиляция программы на языке C из командной строки](https://learn.microsoft.com/ru-ru/cpp/build/walkthrough-compile-a-c-program-on-the-command-line?view=msvc-170)

[Предварительные требования](https://learn.microsoft.com/ru-ru/cpp/build/walkthrough-compiling-a-cpp-cli-program-on-the-command-line?view=msvc-170#prerequisites)

[Компиляция программы на C++/CLI](https://learn.microsoft.com/ru-ru/cpp/build/walkthrough-compiling-a-cpp-cli-program-on-the-command-line?view=msvc-170#compiling-a-ccli-program)

### **Программа суммы**

```cpp
#include <iostream>

int main()
{
    int a, b;
    std::cin >> a >> b;
    std::cout << a + b << '\n';
}
```

### **Команды для трансляции программы**
```txt
C:\Program Files\Microsoft Visual Studio\2022\Community>cd c:\
c:\ md c:\main
c:\ cd c:\main

c:\main>dir
Том в устройстве C имеет метку Eidos
 Серийный номер тома: 9C37-A8CD

 Содержимое папки c:\main

13.02.2023  14:34    <DIR>          .
13.02.2023  14:33               698 main.cpp
13.02.2023  14:46           246 784 main.exe
13.02.2023  14:46           151 526 main.obj
               3 файлов        399 008 байт
               1 папок  498 085 363 712 байт свободно

c:\main notepad main.cpp

c:\main>cl main.cpp
Оптимизирующий компилятор Microsoft (R) C/C++ версии 19.34.31937 для x86
(C) Корпорация Майкрософт (Microsoft Corporation).  Все права защищены.

main.cpp
C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\include\ostream(287): warning C4530: Использован обработчик исключений C++, но семантика уничтожения объектов не включена. Задайте параметр /EHsc
C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.34.31933\include\ostream(272): note: во время компиляции функции-члена класс шаблон "std::basic_ostream<char,std::char_traits<char>> &std::basic_ostream<char,std::char_traits<char>>::operator <<(int)"
main.cpp(13): note: выполняется компиляция ссылки на экземпляр шаблон функции "std::basic_ostream<char,std::char_traits<char>> &std::basic_ostream<char,std::char_traits<char>>::operator <<(int)"
main.cpp(13): note: выполняется компиляция ссылки на экземпляр класс шаблон функции "std::basic_ostream<char,std::char_traits<char>>"
Microsoft (R) Incremental Linker Version 14.34.31937.0
Copyright (C) Microsoft Corporation.  All rights reserved.

/out:main.exe
main.obj

c:\main>main
15 7
22

c:\main>
```

---

#### 2. Для используемого вами транслятора подготовьте список основных опций трансляции. В него обязательно должны входить следующие опции: оптимизация кода, выбора стандарта языка, работы с предупреждениями и другие, которые покажутся вам важными. Сделайте краткое описание этих опций.

[Настройка компилятора и свойств сборки](https://learn.microsoft.com/ru-ru/cpp/build/working-with-project-properties?view=msvc-170)

[Использование набора инструментов Microsoft C++ из командной строки](https://learn.microsoft.com/ru-ru/cpp/build/building-on-the-command-line?view=msvc-170)

![1](1.png)

---

#### 3. Напишите программы с использованием особенностей С++11 и С++17. Выполните их трансляцию с указанием стандарта языка. Запустите эти программы и проверьте их работоспособность

Задание выполненно в среде Visual Studio Community 2022 17.4.5 
Среда разработки не включает стандарт С++11, поэтому, вместо него использовался стандарт С++14

```cpp

// Стандарт языка C++   Стандарт ISO C++14(std:c++14)

#include <iostream>
#include <algorithm>

int main()
{
    int a, b, c;
    std::cin >> a >> b >> c;
    std::cout << std::min({ a,b,c });
    return 0;
}
```

**Результат отладки**
```txt
9 1 5
1
```
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
```cpp

// Стандарт языка C++   Стандарт ISO C++17(std:c++17)

#include <iostream>
#include <vector>

int main()
{
    int a, b, c;
    std::cin >> a >> b >> c;
    std::vector v = { a, b, c };
    for (auto x : v)
        std::cout << x << ' ';
    return 0;
}
```

**Результат отладки**
```txt
1 a Ж
1 0 -858993460
```
---

#### 4. Требуется сравнить время работы алгоритмов сортировки на С++ и других языках. Для этого потребуется приготовить тесты. Каждый тест будет последовательностью целых чисел. Каждый файл будет содержать несколько тестов. Все тесты одной группы имеют одинаковую длину. Будет использоваться следующий формат фойла. В файле в первой строке надо записать количество групп тестов. Далее количесвто тестов в первой группе длина тестов. Далее через пробел сами тесты, по одному тесту в каждой строке. После этого аналогично описание группы тестов и так далее. Например

3 // количество тестов
2 5 // первая группа. Два теста по 5 чисел
1 2 3 4 5 // первый тест
5 4 3 2 1 // второй тест
2 8 // вторая группа. Два теста по 8 чисел
5 1 2 8 3 10 4 7
8 1 7 2 4 3 6 5
2 3 // третья группа. Два теста по 3 чисел
1 3 2
9 10 11

#### **Преднастройки трансляции**

![2](2.png)


#### **Листинг программы**
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <fstream>
#include <set>
#include <vector>
#include <string>

#include "testlib.h"

using namespace std;
int main(int argc, char *argv[]) {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    
    ifstream cin("param.txt");

    string fname, initstr;
    getline(cin, fname);
    getline(cin, initstr);
    registerGen(argc, argv, 1);
    setName(initstr.c_str());
    ofstream txt(fname);

    int groups, tests;
    cin >> groups >> tests;

    vector<int> len(groups);
    for (int& x : len)
        cin >> x;

    int from, to;
    cin >> from >> to;
    set <int> s;
    txt << groups << endl;
    for (int i = 0; i < groups; i++) {
        txt << tests << " " << len[i] << endl;
        for (int j = 0; j < tests; j++) {
            while (s.size() < len[i])
                s.insert(rnd.next(from, to));
            vector<int> shf(s.size());
            shf.assign(s.begin(), s.end());
            s.clear();
            shuffle(shf.begin(), shf.end());
            for (auto q : shf)
                txt << q << ' ';
            txt << '\n';
        }
    }
    return 0;
}
```

**param.txt**

Для данных параметров

```txt
output.txt
test
3
3 2 2
100000 10000 50000
1 100000
```

---

#### 5. В этом задании вам надо будет написать программу для выполнения сотировки последовательности целых чисел в С++ разными способами. Их достаточно много. Выберем пять следующих способов.



```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <fstream>
#include <set>
#include <vector>
#include <algorithm>
#include <chrono>

#define _CRT_SECURE_NO_WARNINGS
#include "testlib.h"

using namespace std;

double srt1(vector<int> v) {
    auto start = chrono::high_resolution_clock::now();
    sort(v.begin(), v.end());
    auto end = chrono::high_resolution_clock::now();
    chrono::duration<double>dur = end - start;
    return dur.count();

}
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

double sort2(vector<int> V){
  auto t1=chrono::high_resolution_clock::now();
  stable_sort(V.begin(), V.end());
  auto t2=chrono::high_resolution_clock::now();
  chrono::duration<double> dur=t2-t1;
  return dur.count();
}

double sort3(vector<int> V){
  auto t1=chrono::high_resolution_clock::now();
  sort_heap(V.begin(), V.end());
  auto t2=chrono::high_resolution_clock::now();
  chrono::duration<double> dur=t2-t1;
  return dur.count();
}

double sort4(vector<int> V){
  auto t1=chrono::high_resolution_clock::now();
  set<int> S(V.begin(), V.end());
  copy(S.begin(), S.end(), V.begin());
  auto t2=chrono::high_resolution_clock::now();
  chrono::duration<double> dur=t2-t1;
  return dur.count();
}

double sort5(vector<int> V){
  auto t1=chrono::high_resolution_clock::now();
  list<int> L(V.begin(), V.end());
  L.sort();
  copy(L.begin(), L.end(), V.begin());
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

      t1=sort2(v);
      t2=sort2(v);
      t3=sort2(v);
      mini[n+1]=min({t1,t2,t3});

      t1=sort3(v);
      t2=sort3(v);
      t3=sort3(v);
      mini[2+n]=min({t1,t2,t3});

      t1=sort4(v);
      t2=sort4(v);
      t3=sort4(v);
      mini[3+n]=min({t1,t2,t3});

      t1=sort5(v);
      t2=sort5(v);
      t3=sort5(v);
      mini[4+n]=min({t1,t2,t3});
      n+=5;
    }
   for(int i=0; i<5; i++){
     h=i;
     for(h; h<mini.size(); h+=5)
     	sr[i]+=mini[h];
     sr[i]/=tests;
   }
   fout<<dl<<' '<<' '<<sr[0]<<' '<<sr[1]<<' '<<sr[2]<<' '<<sr[3]<<' '<<sr[4]<<'\n';

}
  return 0;
}

```


---

#### 6. Используя программы из предыдущих заданий, определите, вектор какой размера сортируется на вашем компьютере примерно за секунду. Для этого сделайте тестовый файл из одной группы тестов. В группе должно быть пять тестов некоторой длины _m_. Подберите это число так, чтобы среднее время работы саного быстрого метода сортивовки в файле protocol.txt равнялось примерной одной секунде. Будет достаточно, если число _m_ будет округлено до 100000, то есть число будет оканчиваться на 5 нулей. В отчете запишиет, какие длины тестов вы пробовали и какое время работы получили.

---

#### 7. Используя программы из предыдущих заданий и найденное ранее число _m_, создайте тестовый файл из 10 групп тестов. В каждой группе должно быть по три теста с длинами равными _0:2m, 0:4m, 0:6m, 0:8m, m, 1:2m, 1:4m, 1:6m, 1:8m, 2m_. Проверьте время сортировки на этих группах тестов. В отчет запишите данне из файла protocol.txt

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
  
   for (int i = 0; i < V.size() - 1; i++) {
        for (int j = 0; j < V.size() - i - 1; j++) {
            if (V[j] > V[j + 1]) {
                int a = V[j];
                V[j] = V[j + 1];
                V[j + 1] = a;
            }
        }
    }
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


#### 8.

```python
import matplotlib.pyplot as plt

dl=[]
s1 = []
s2 = []
s3 = []
s4 = []
s5 = []

with open("protocol.txt", 'r') as fin:
    for str in fin:
        a = list(map(float, str.split()))

        dl.append(int(a[0]))
        s1.append(a[1])
        s2.append(a[2])
        s3.append(a[3])
        s4.append(a[4])
        s5.append(a[5])

plt.plot(dl, s1, dl, s2, dl, s3, dl, s4, dl, s5)
plt.legend(['sort', 'stable_sort', 'sort_heap', 'set', 'list'])
plt.show()
```

```
protocol.txt

400000  0.177193 0.204968 0.413562 0.551875 0.550258
800000  0.459126 0.508504 1.01687 1.34236 1.29557
1200000  0.697849 0.803045 1.57415 2.13733 2.04891
1600000  0.948925 1.08195 2.17168 2.96782 2.79744
2000000  1.32735 1.64619 3.23357 3.79455 3.5024
2400000  1.4881 1.64922 3.39567 4.70083 4.42133
2800000  1.75307 1.96671 3.94915 5.58853 5.15345
3200000  2.02279 2.25556 4.62188 6.48013 6.04289
3600000  2.29864 2.55916 5.20503 7.36648 6.70942
4000000  2.53996 2.95118 5.82939 8.37552 7.5642
```

#### 9.

#### 10.

#### 11. 

**java**

```java
package javaapplication34;

import java.io.File;
import java.io.IOException;
import java.util.Arrays;
import java.util.Scanner;
public class JavaApplication34 {
    
    public static double sort(int[] v){
        int[] v1 = v.clone();
        long t = System.nanoTime();
        for (int i = 0;i < v1.length; i++){
            for(int j = i; j < v1.length - 1;j++){
                if(v1[j] > v1[j+1]) {
                    int a = v1[j];
                    v1[j] = v1[j+1];
                    v1[j+1] = a;
                }
            }
        }
        return ((double) (System.nanoTime() - t)/1000000000.0);
    }
    public static void main(String[] args) throws IOException {
        System.out.println(new File("").getAbsolutePath());
        File file = new File("output.txt");
        Scanner in = new Scanner(file);
        
        int n = in.nextInt();
        for(int i = 0; i < n; i++){
            int tests = in.nextInt();
            int dl = in.nextInt();
            double sums = 0;
            
            for(int j = 0; j < tests;j++){
                int[] v = new int[dl];
                for(int k = 0; k < dl; k++){
                    v[k] = in.nextInt();
                }
                sums += Math.min(Math.min(sort(v),sort(v)),sort(v));
            }
            System.out.println(dl + "  " + sums/tests);
        }
    }
}
```

```
java output

2000  0.012021567333333332
4000  0.03109113366666667
6000  0.054992067000000006
8000  0.088008033
10000  0.11898740000000001
12000  0.16461966633333333
14000  0.2240137336666667
16000  0.29743863333333337
18000  0.38308086599999996
20000  0.4769536003333334
```

**python**


```python
# -*- coding: utf-8 -*-
"""
Created on Fri Feb 10 11:24:35 2023

@author: gogak
"""
import random

special = '!#?'

password = ''
type1, type2, type3, type4 = 0, 0, 0, 0
for i in range (0, 4, 1):
    
    for j in range (0, 4, 1):
        type = random.randrange(0,4)
        if type == 0:
            password += chr(random.randrange(65, 90))
            type1 += 1
        elif type == 1:
            password += chr(random.randrange(97, 122))
            type2 += 1
        elif type == 2:
            password += str(random.randrange(0, 10))
            type3 += 1
        elif type == 3:
            password += special[random.randrange(0, 3)]
            type4 += 1
    password += '-'
    
print(type1)
print(type2)
print(type3)
print(type4)

        
print (password[:-1], end='\n')
```

```
python output

2000 0.8118159770965576
4000 2.9580763975779214
6000 7.9191586176554365
8000 12.436465581258139
10000 16.410430669784546
12000 23.874129056930542
14000 31.75672483444214
16000 42.5342071056366
18000 56.91302458445231
20000 70.37112991015117
```

#### 12.

#### SRC

[KirichenkoKD 1](https://www.youtube.com/watch?v=SFxZbttrFvM)

[KirichenkoKD 2](https://www.youtube.com/watch?v=YsAHT_6ZxXA)

[KirichenkoKD 3](https://www.youtube.com/watch?v=rx4vD6qE1KI)

[KirichenkoKD 4](https://www.youtube.com/watch?v=w7IrnNbCx8g)

[KirichenkoKD 5](https://www.youtube.com/watch?v=aj1PEQRnFB0)

[KirichenkoKD 6](https://www.youtube.com/watch?v=jVym-3clSeY)

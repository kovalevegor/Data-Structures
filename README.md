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


#### 8.

#### 9.

#### 10.

#### 11.

#### 12.

#### SRC

[KirichenkoKD 1](https://www.youtube.com/watch?v=SFxZbttrFvM)

[KirichenkoKD 2](https://www.youtube.com/watch?v=YsAHT_6ZxXA)

[KirichenkoKD 3](https://www.youtube.com/watch?v=rx4vD6qE1KI)

[KirichenkoKD 4](https://www.youtube.com/watch?v=w7IrnNbCx8g)

[KirichenkoKD 5](https://www.youtube.com/watch?v=aj1PEQRnFB0)

[KirichenkoKD 6](https://www.youtube.com/watch?v=jVym-3clSeY)

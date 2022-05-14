## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```bash
git clone https://github.com/tp-labs/lab03
```

```bash
cd formatter_lib
vim CMakeLists.txt
```
```cmake
cmake_minimum_required(VERSION 3.4)
project(formatter_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
```
```bash
cmake -H. -B_build && cmake --build _build  
```


### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```bash
cd ../formatter_ex_lib
vim CMakeLists.txt
```
```cmake
cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
include_directories("../formatter_lib")
add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
```
```bash
cmake -H. -B_build && cmake --build _build 
```


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```bash
cd ../hello_world_application
vim CMakeLists.txt
```
```cmake
cmake_minimum_required(VERSION 3.4)
project(lab03)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories("../formatter_lib")
include_directories("../formatter_ex_lib")

add_library(formatter_lib STATIC "../formatter_lib/formatter.cpp")
add_library(formatter_ex_lib STATIC "../formatter_ex_lib/formatter_ex.cpp")




add_executable(lab03 "hello_world.cpp")
target_link_libraries(lab03 formatter_ex_lib formatter_lib)
```
```bash
cmake -H. -B_build && cmake --build _build
```

```bash
cd ../solver_lib
```
```cmake
cmake_minimum_required(VERSION 3.4)
project (solver_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC ${CMAKE_CURRENT_SOURCE_DIR}/solver.h ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
```
```bash
cmake -H. -B_build && cmake --build _build
cd ../solver_application
vim CMakeLists.txt
```
```cmake
cmake_minimum_required(VERSION 3.4)
project(lab03)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories("../formatter_lib")
include_directories("../formatter_ex_lib")
include_directories("../solver_lib")
add_library(formatter_lib STATIC "../formatter_lib/formatter.cpp")
add_library(formatter_ex_lib STATIC "../formatter_ex_lib/formatter_ex.cpp")
add_library(solver_lib STATIC "../solver_lib/solver.cpp")




add_executable(lab03 "equation.cpp")
target_link_libraries(lab03 solver_lib formatter_ex_lib formatter_lib)
```
```bash
cmake -H. -B_build && cmake --build _build
```
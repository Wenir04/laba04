# laba3
# Задание 1

- [x] Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.
```
mkdir lab3 && cd laba3
git clone https://github.com/tp-labs/lab03.git
cd lab03
git remote remove origin
git remote add origin https://github.com/Wenir04/laba03.git
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10) 
project(formatter) 
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
cmake -H. -B_build
ls _build
```

# Задание 2

- [x] У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeLists.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeLists.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.
```
cd formatter_ex_lib
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter_ex)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//artem/lab3/lab03)
add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
target_link_libraries(formatter_ex formatter)
EOF
cmake -H. -B_build
ls ls _build
```

# Задание 3

- [x] Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

    hello_world, которое использует библиотеку formatter_ex;
    solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.
    
    
 ```
 cd hello_world_application
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(hello_world)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//artem/lab3/lab3)
add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
target_link_libraries(hello_world \${formatter} \${formatter_ex})
EOF

cmake -H. -B_build
_build/hello_world &&
$ cd solver_application
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(solver)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//artem/lab3/lab3)
add_library(solver_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
$ cmake -H. -B_build
$ cat >> CMakeLists.txt <<EOF
add_executable(solver \${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(solver_lib NAMES libsolver_lib.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
target_link_libraries(solver \${formatter} \${formatter_ex} \${solver_lib})
EOF

$ cmake -H. -B_build
$ cmake --build _build # Сборка всего проекта
$ cmake --build _build --target solver_lib 
$ cmake --build _build --target solver 
$ _build/solver && echo
```
![изображение](https://github.com/Wenir04/laba3/assets/113133600/0ac4427b-fb99-45c9-bc0f-7ec5e16f448f)
![изображение](https://github.com/Wenir04/laba3/assets/113133600/8d3f0d5a-0d63-411a-8f0a-e3e9355cc508)


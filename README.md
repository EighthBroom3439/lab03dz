# Laboratory work III dz

B данной работе было выполнено следующее:

## 1. Был скопирован репозиторий из лабораторной работы и исходные папки из репозитория лабораторной работы
```bash
vdeo@deo:~$ export GITHUB_USERNAME=EighthBroom3439
vdeo@deo:~$ cd ${GITHUB_USERNAME}/workspace/
vdeo@deo:~/EighthBroom3439/workspace$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git lab03dz
Клонирование в «lab03dz»...
remote: Enumerating objects: 73, done.
remote: Counting objects: 100% (73/73), done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 73 (delta 24), reused 73 (delta 24), pack-reused 0 (from 0)
Получение объектов: 100% (73/73), 23.10 КиБ | 2.89 МиБ/с, готово.
Определение изменений: 100% (24/24), готово.
vdeo@deo:~/EighthBroom3439/workspace$ cd lab03dz/
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ git remote remove origin
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03dz
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cd ..
vdeo@deo:~/EighthBroom3439/workspace$ git clone https://github.com/tp-labs/lab03.git templab03
Клонирование в «templab03»...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Получение объектов: 100% (91/91), 1.02 МиБ | 3.31 МиБ/с, готово.
Определение изменений: 100% (41/41), готово.
vdeo@deo:~/EighthBroom3439/workspace$ cp - r templab03/formatter_lib ./lab03dz
cp: не удалось выполнить stat для '-': Нет такого файла или каталога
cp: не удалось выполнить stat для 'r': Нет такого файла или каталога
cp: не указан -r; пропускается каталог 'templab03/formatter_lib'
vdeo@deo:~/EighthBroom3439/workspace$ cp -r templab03/formatter_lib ./lab03dz
vdeo@deo:~/EighthBroom3439/workspace$ cp -r templab03/formatter_ex_lib ./lab03dz
vdeo@deo:~/EighthBroom3439/workspace$ cp -r templab03/solver_lib ./lab03dz
vdeo@deo:~/EighthBroom3439/workspace$ cp -r templab03/hello_world_application ./lab03dz
vdeo@deo:~/EighthBroom3439/workspace$ cp -r templab03/solver_application ./lab03dz
vdeo@deo:~/EighthBroom3439/workspace$ rm -rf templab03/
```

## 2. Был создан CMakeLists.txt для папки formatter_lib
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > formatter_lib/CMakeLists.txt << 'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(formatter_lib VERSION 1.0)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> 
> add_library(formatter STATIC formatter.cpp)
> 
> target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
> EOF
```

## 3. Был создан CMakeLists.txt для formatter_ex_lib
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > formatter_ex_lib/CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(formatter_ex_lib VERSION 1.0)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> 
> add_library(formatter_ex STATIC formatter_ex.cpp)
> 
> target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
> 
> target_link_libraries(formatter_ex PUBLIC formatter)
> EOF
```

## 4. Был создан CMakeLists.txt для solver_lib
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > solver_lib/CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(solver_lib VERSION 1.0)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> 
> add_library(solver_lib STATIC solver.cpp)
> 
> target_include_directories(solver_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
> EOF
```

## 5. Был создан CMakeLists.txt для hello_world_application
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > hello_world_application/CMakeLists.txt << 'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(hello_world)
> 
> set(CMAKE_CXX_STANDARD 11)
> 
> add_executable(hello_world hello_world.cpp)
> 
> target_link_libraries(hello_world PRIVATE formatter_ex)
> EOF
```

## 6. Был создан CMakeLists.txt для solver_application
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > solver_application/CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(solver)
> 
> set(CMAKE_CXX_STANDARD 11)
> 
> add_executable(solver equation.cpp)
> 
> target_link_libraries(solver PRIVATE formatter_ex solver_lib)
> EOF
```

## 7. Был создан CMakeLists.txt, объединяющий весь проект
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cat > CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.10)
> project(FormatterInc)
> 
> add_subdirectory(formatter_lib)
> add_subdirectory(formatter_ex_lib)
> add_subdirectory(solver_lib)
> 
> add_subdirectory(hello_world_application)
> add_subdirectory(solver_application)
> EOF
```

## 8. Была обнаружена ошибка в solver.cpp
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ rm -rf build
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ mkdir build && cd build
vdeo@deo:~/EighthBroom3439/workspace/lab03dz/build$ cmake ..
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (3.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vdeo/EighthBroom3439/workspace/lab03dz/build
vdeo@deo:~/EighthBroom3439/workspace/lab03dz/build$ cmake --build .
[ 10%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[ 20%] Linking CXX static library libformatter.a
[ 20%] Built target formatter
[ 30%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 40%] Linking CXX static library libformatter_ex.a
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
/home/vdeo/EighthBroom3439/workspace/lab03dz/solver_lib/solver.cpp: In function ‘void solve(float, float, float, float&, float&)’:
/home/vdeo/EighthBroom3439/workspace/lab03dz/solver_lib/solver.cpp:14:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘strtof’?
   14 |     x1 = (-b - std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     strtof
/home/vdeo/EighthBroom3439/workspace/lab03dz/solver_lib/solver.cpp:15:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘strtof’?
   15 |     x2 = (-b + std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     strtof
gmake[2]: *** [solver_lib/CMakeFiles/solver_lib.dir/build.make:79: solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o] Ошибка 1
gmake[1]: *** [CMakeFiles/Makefile2:262: solver_lib/CMakeFiles/solver_lib.dir/all] Ошибка 2
gmake: *** [Makefile:91: all] Ошибка 2
```

## 9. Она была исправлена
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ sed -i 's/std::sqrtf/sqrtf/g' solver_lib/solver.cpp
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ sed -i '1i#include <cmath>' solver_lib/solver.cpp
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ head -5 solver_lib/solver.cpp
```

## 10. Проект был собран и протестирован
```bash
vdeo@deo:~/EighthBroom3439/workspace/lab03dz$ cd build/
vdeo@deo:~/EighthBroom3439/workspace/lab03dz/build$ cmake --build .
[ 20%] Built target formatter
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 60%] Linking CXX static library libsolver_lib.a
[ 60%] Built target solver_lib
[ 70%] Building CXX object hello_world_application/CMakeFiles/hello_world.dir/hello_world.cpp.o
[ 80%] Linking CXX executable hello_world
[ 80%] Built target hello_world
[ 90%] Building CXX object solver_application/CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
vdeo@deo:~/EighthBroom3439/workspace/lab03dz/build$ ./hello_world_application/hello_world
-------------------------
hello, world!
-------------------------
vdeo@deo:~/EighthBroom3439/workspace/lab03dz/build$ ./solver_application/solver
1 -3 2
-------------------------
x1 = 1.000000
-------------------------
-------------------------
x2 = 2.000000
-------------------------
```

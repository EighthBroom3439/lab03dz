# Laboratory work III

Цель данной лабораторной работы заключалась в том, чтобы ознакомиться с системами автоматизации сборки проекта на примере CMake.
B данной работе было выполнено следующее:

## 1. Был скопирован репозиторий из предыдущей лабораторной работы
```bash
vdeo@deo:~$ export GITHUB_USERNAME=EighthBroom3439
vdeo@deo:~$ cd ${GITHUB_USERNAME}/workspace
vdeo@deo:~/EighthBroom3439/workspace$ pushd .
~/EighthBroom3439/workspace ~/EighthBroom3439/workspace
vdeo@deo:~/EighthBroom3439/workspace$ source scripts/activate
vdeo@deo:~/EighthBroom3439/workspace$ git clone -b master https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
Клонирование в «projects/lab03»...
remote: Enumerating objects: 67, done.
remote: Counting objects: 100% (67/67), done.
remote: Compressing objects: 100% (46/46), done.
remote: Total 67 (delta 23), reused 46 (delta 13), pack-reused 0 (from 0)
Получение объектов: 100% (67/67), 19.31 КиБ | 420.00 КиБ/с, готово.
Определение изменений: 100% (23/23), готово.vdeo@deo:~/EighthBroom3439/workspace$ cd projects/lab03
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ git remote remove origin
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

## 2. Была выполнена компиляция ручная компиляция файлов из предыдущей лабораторной работы
```bash
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ g++ -std=c++11 -I./include -c sources/print.cpp
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ ls print.o 
print.o
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ nm print.o | grep print
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ ar rvs print.a print.o
ar: создаётся print.a
a - print.o
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ file print.a
print.a: current ar archive
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ g++ -std=c++11 -I./include -c examples/example1.cpp 
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ ls example1.o
example1.o
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ g++ example1.o print.a -o example1
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ ./example1 && echo
hello
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ g++ -std=c++11 -I./include -c examples/example2.cpp 
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ nm example2.o
0000000000000000 V DW.ref.__gxx_personality_v0
                 U __gxx_personality_v0
0000000000000000 T main
                 U _Unwind_Resume
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED2Ev
0000000000000000 n _ZNSt15__new_allocatorIcED5Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZSt21ios_base_library_initv
0000000000000000 r _ZStL19piecewise_construct
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ g++ example2.o print.a -o example2
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ ./example2
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat log.txt && echo
hello
```

## 3. Был создан CMakeList.txt
```bash
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4)
> project(print)
> EOF
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> EOF
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
> EOF
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
> EOF
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake -H. -B_build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


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
-- Configuring done (1.8s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_build
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
> add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
> EOF
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ nano CMakeLists.txt 
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> target_link_libraries(example1 print)
> target_link_libraries(example2 print)
> EOF
```

## 4. Была произведена сборка проекта, и были протестированы файлы example1 и example2
```bash
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_build
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[ 66%] Linking CXX executable example1
[ 66%] Built target example1
[ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[100%] Linking CXX executable example2
[100%] Built target example2
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build --target print
[100%] Built target print
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ _build/example1 && echo
hello
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ _build/example2
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat log.txt && echo
hello
```

## 5. Из предоставленного репозитория текущей лабораторной работы был скопирован и собран файл CMakeList.txt
```bash
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ git clone https://github.com/tp-labs/lab03 tmp
Клонирование в «tmp»...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Получение объектов: 100% (91/91), 1.02 МиБ | 2.86 МиБ/с, готово.
Определение изменений: 100% (41/41), готово.
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ mv -f tmp/CMakeLists.txt .
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ rm -rf tmp
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cat CMakeLists.txt 
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_build
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ cmake --build _build --target install
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_install/include
-- Installing: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/vdeo/EighthBroom3439/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
vdeo@deo:~/EighthBroom3439/workspace/projects/lab03$ tree _install
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

4 directories, 4 files
```
В данном репозитории лежит этот же файл

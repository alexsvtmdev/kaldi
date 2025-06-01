cmake_minimum_required(VERSION 3.18)
project(kaldi-android-16k)

# Настройки компилятора
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Отключаем CUDA и всё, что его требует
set(CUDA_TOOLKIT_ROOT_DIR "" CACHE PATH "Disable CUDA")
set(CUDA_FOUND OFF)
set(HAVE_CUDA OFF)

# Используем OpenBLAS и CLAPACK
set(MATHLIB "OpenBLAS" CACHE STRING "OpenBLAS")
add_definitions(-DHAVE_CLAPACK=1)

# Пути к CLAPACK и OpenBLAS
include_directories(${CMAKE_SOURCE_DIR}/../openblas/install_arm64/include)
include_directories(${CMAKE_SOURCE_DIR}/tools/CLAPACK)

# Устанавливаем путь к libopenblas.a, если не задан извне
if(NOT DEFINED OPENBLAS_LIB_PATH)
  set(OPENBLAS_LIB_PATH "${CMAKE_SOURCE_DIR}/../openblas/install_arm64/lib/libopenblas.a")
endif()

# Подключаем OpenFST
include_directories(${CMAKE_SOURCE_DIR}/../openfst/install/include)
link_directories(${CMAKE_SOURCE_DIR}/../openfst/install/lib)

# Линкуем openblas и fst
link_libraries("${OPENBLAS_LIB_PATH}" fst)

# Подключаем только нужные библиотеки Kaldi
add_subdirectory(src/base)
add_subdirectory(src/util)
# add_subdirectory(src/online2)




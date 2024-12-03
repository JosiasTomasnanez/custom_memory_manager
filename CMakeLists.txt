cmake_minimum_required(VERSION 3.27 FATAL_ERROR)

# Nombre del proyecto
project(memory_program VERSION 1.0.0 LANGUAGES C)

include(CTest)

# Habilitar compilación con símbolos de depuración
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -g")

# Encontrar paquetes requeridos
find_package(PkgConfig REQUIRED)

# Asegurarse de que Unity esté disponible (si se requiere instalar, indicar ruta)
find_package(Unity REQUIRED)

# Incluir directorios de cabecera
include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/lib/memory/include)

# Archivos fuente principales
set(MAIN_SRC ${CMAKE_SOURCE_DIR}/src/main.c)


# Subdirectorio de la biblioteca
add_subdirectory(${CMAKE_SOURCE_DIR}/lib/memory)

# Ejecutable principal
add_executable(memory_program ${MAIN_SRC})

target_include_directories(memory_program PUBLIC ${CMAKE_SOURCE_DIR}/lib/memory/include)

# Enlazar la biblioteca estática con el ejecutable
target_link_libraries(memory_program memory unity::unity)

if(RUN_TESTS EQUAL 1 OR RUN_COVERAGE EQUAL 1)
    add_subdirectory(${CMAKE_SOURCE_DIR}/tests)
endif()

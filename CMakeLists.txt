cmake_minimum_required(VERSION 3.21)
project(simple_c_01 VERSION 0.1.0 LANGUAGES C)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_library(math_utils src/math_utils.c)
target_include_directories(math_utils PUBLIC include)

add_executable( ${PROJECT_NAME} src/main.c)
target_link_libraries(${PROJECT_NAME}  PRIVATE math_utils)

if(CMAKE_C_COMPILER_ID MATCHES "AppleClang|Clang|GNU")
  target_compile_options(math_utils PRIVATE -Wall -Wextra -Wpedantic)
  target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
endif()

include(CTest)
if(BUILD_TESTING)
  add_executable(test_math_utils tests/test_math_utils.c)
  target_link_libraries(test_math_utils PRIVATE math_utils)
  if(CMAKE_C_COMPILER_ID MATCHES "AppleClang|Clang|GNU")
    target_compile_options(test_math_utils PRIVATE -Wall -Wextra -Wpedantic)
  endif()
  add_test(NAME math_utils COMMAND test_math_utils)
endif()

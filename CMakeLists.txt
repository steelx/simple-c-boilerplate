cmake_minimum_required(VERSION 3.21)
project(simple_c_01 VERSION 0.1.0 LANGUAGES C)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# SDL3 packages
find_package(SDL3 REQUIRED CONFIG)
find_package(SDL3_image CONFIG REQUIRED)
find_package(SDL3_ttf CONFIG REQUIRED)

# third_party
add_library(cjson STATIC ${CMAKE_CURRENT_SOURCE_DIR}/third_party/cjson/cJSON.c)
target_include_directories(
  cjson PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}/third_party/cjson
)

add_executable(${PROJECT_NAME}
  src/main.c
  # add additional c files below
  src/math_utils.c
)

# Tell the compiler to look inside include/ for header files
target_include_directories(${PROJECT_NAME} PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(
  ${PROJECT_NAME} PRIVATE
  SDL3::SDL3 SDL3_image::SDL3_image SDL3_ttf::SDL3_ttf cjson
)

# global variables
target_compile_definitions(${PROJECT_NAME} PRIVATE
  RPG_ASSETS="${CMAKE_CURRENT_SOURCE_DIR}/assets"
  RPG_MAPS="${CMAKE_CURRENT_SOURCE_DIR}/maps"
)

if(CMAKE_C_COMPILER_ID MATCHES "AppleClang|Clang|GNU")
  target_compile_options(
    ${PROJECT_NAME} PRIVATE
    -Wall -Wextra -Wpedantic -Werror -Wshadow -Wstrict-prototypes
  )
endif()


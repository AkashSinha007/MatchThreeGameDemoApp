cmake_minimum_required(VERSION 3.16)

set(CMAKE_TOOLCHAIN_FILE "${CMAKE_CURRENT_SOURCE_DIR}/package_manager/vcpkg/scripts/buildsystems/vcpkg.cmake"
  CACHE STRING "Vcpkg toolchain file")


project(MatchThreeGameApp VERSION 1.0 LANGUAGES C CXX)


include(CTest)
set(CMAKE_CXX_STANDARD 17)

find_package(OpenGL REQUIRED)

find_package(SDL2 CONFIG REQUIRED)
find_package(SDL2_ttf CONFIG REQUIRED)

find_package(GTest CONFIG REQUIRED)

add_subdirectory(Vendor/gulrak-filesystem)
include_directories(Vendor/gulrak-filesystem/include)

add_executable(${PROJECT_NAME}
  ${CMAKE_SOURCE_DIR}/Source/AssetsManager.cpp
  ${CMAKE_SOURCE_DIR}/Source/Game.cpp
  ${CMAKE_SOURCE_DIR}/Source/Main.cpp
  ${CMAKE_SOURCE_DIR}/Source/Timer.cpp
  ${CMAKE_SOURCE_DIR}/Source/Game/World.cpp
  ${CMAKE_SOURCE_DIR}/Source/Rendering/Graphics.cpp
  ${CMAKE_SOURCE_DIR}/Source/Rendering/TexturePointer.cpp
)

target_include_directories(${PROJECT_NAME} PRIVATE
  ${CMAKE_SOURCE_DIR}/Include
  ${CMAKE_SOURCE_DIR}/Include/Math 
  ${CMAKE_SOURCE_DIR}/Include/Rendering
  ${CMAKE_SOURCE_DIR}/Include/Game

  ${CMAKE_SOURCE_DIR}/Vendor/stb
  ${CMAKE_SOURCE_DIR}/Vendor/nlohmann
  ${CMAKE_SOURCE_DIR}/Vendor/windows_dirent
)

target_link_libraries(${PROJECT_NAME} PRIVATE OpenGL::GL)

target_link_libraries(${PROJECT_NAME} PRIVATE ghc_filesystem)

target_link_libraries(${PROJECT_NAME} PRIVATE
  SDL2::SDL2
  SDL2_ttf::SDL2_ttf-static
)

# Copy Assets folder to build directory
file(COPY Assets DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

file(COPY Settings.json DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
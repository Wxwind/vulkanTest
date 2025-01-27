cmake_minimum_required(VERSION 3.29.6)
project(vulkanTest)

set(CMAKE_CXX_STANDARD 20)

set(LIB_DIR ${PROJECT_SOURCE_DIR}/lib)
set(GLFW_DIR ${LIB_DIR}/glfw)
set(VULKAN_DIR $ENV{HOME}/VulkanSDK/1.4.304.0/)

# 直接运行exe报错找不到libstdc++.dll和winpthread.dll
if (win32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc -static-libstdc++ -static -pthread")
endif ()

add_executable(vulkanTest src/main.cpp
        src/stb_image.cpp
        src/stb_image.h
        src/official_camera.h
)

if (APPLE)
    # 动态链接, 需要手动把libglfw.3.dylib拷贝到输出目录下，


elseif (WIN32)
    # 动态链接, 需要手动把glfw3.dll拷贝到输出目录下，IMPORTED_IMPLIB_DEBUG用于设置动态库，IMPORTED_LOCATION用于设置静态库
    add_library(glfw SHARED IMPORTED)
    SET_TARGET_PROPERTIES(glfw PROPERTIES IMPORTED_IMPLIB "${GLFW_DIR}/lib-mingw-w64/libglfw3dll.a")
    # 静态链接
    # SET_TARGET_PROPERTIES(glfw PROPERTIES IMPORTED_LOCATION "${GLFW_DIR}/lib-mingw-w64/libglfw3.a")
endif ()

# 添加 GLFW3 头文件
target_include_directories(vulkanTest PRIVATE "${GLFW_DIR}/include")
# 添加 glm
target_include_directories(vulkanTest PRIVATE ${LIB_DIR}/glm)


if (APPLE)
    # 链接 GLFW Vulkan
    # 需要手动把vulkansdk/macOS/lib 的 libvulkan.1.dylib 和 libvulkan.1.x.xx.dylib 拷贝到输出目录下
    target_include_directories(vulkanTest PRIVATE "${VULKAN_DIR}/macOS/include")
    target_link_directories(vulkanTest PRIVATE ${VULKAN_DIR}/macOS/lib)

    target_link_libraries(vulkanTest PRIVATE "${GLFW_DIR}/lib-macos-arm64/libglfw.3.dylib")
elseif (WIN32)
    target_link_libraries(vulkanTest glfw)
    # 也可以通过target_link_libraries直接链接静态库或者动态库
    #  target_link_libraries(vulkanTest "${GLFW_DIR}/lib-mingw-w64/libglfw3dll.a")
endif ()


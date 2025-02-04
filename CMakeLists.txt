cmake_minimum_required(VERSION 3.29.6)
project(vulkanTest)

set(CMAKE_CXX_STANDARD 20)

set(LIB_DIR ${PROJECT_SOURCE_DIR}/lib)
set(GLFW_DIR ${LIB_DIR}/glfw)
if (APPLE)
    set(VULKAN_DIR $ENV{HOME}/VulkanSDK/1.4.304.0/)
elseif (WIN32)
    set(VULKAN_DIR C:/VulkanSDK/1.4.304.0)
endif ()

# 直接运行exe报错找不到libstdc++.dll和winpthread.dll
if (win32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc -static-libstdc++ -static -pthread")
endif ()

add_executable(vulkanTest src/main.cpp
        src/stb_image.cpp
        src/stb_image.h
        src/official_camera.h
)

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
    target_include_directories(vulkanTest PRIVATE "${VULKAN_DIR}/Include")
    target_link_directories(vulkanTest PRIVATE ${VULKAN_DIR}/Lib)

    # 通过target_link_libraries直接链接静态库或者动态库，动态库需要复制glfw3.dll到输出目录下
    target_link_libraries(vulkanTest "${GLFW_DIR}/lib-mingw-w64/libglfw3dll.a")
endif ()


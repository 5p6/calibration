cmake_minimum_required(VERSION 3.10)

project(camera_calibration)

### C++17 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Release)

### 设置不同的编译器
message(STATUS "CMake C++ compiler: ${CMAKE_CXX_COMPILER}")
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    ### 链接库
    find_package(OpenCV REQUIRED)
    if(OpenCV_FOUND)
        message("OpenCV finded ")
    else()
        message(FATAL_ERROR "OpenCV not find")
    endif(OpenCV_FOUND)
    
elseif (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    ### 链接库
    # 参杂着debug和release的链接库，利用config可以设置链接库的类型
    file(GLOB OpenCV_LIBS "E:/lib/opencv/opencv4/build/install/x64/vc17/lib/*.lib" )
    set(FMT_INCLUDE_DIR E:/lib/fmt/install/include)
    set(OpenCV_INCLUDE_DIR E:/lib/opencv/opencv++/install/include)
    message(STATUS "Using Microsoft Visual C++ compiler -- MSVC")

else()
    message(WARNING "Unknown compiler")
endif()



### 添加包目录
include_directories(
    ./src
    ${FMT_INCLUDE_DIR}
    ${OpenCV_INCLUDE_DIR}
)

### 生成库文件
add_library(calib STATIC
    ./src/calib.cc
    ./src/calib.h
    ./src/utility.cc
    ./src/utility.h
)

### 链接
target_link_libraries(calib
    ${OpenCV_LIBS}
)


### 运行
add_executable(example example.cc)

target_link_libraries(example
    calib
)

add_executable(example_calib example_calib.cc)

target_link_libraries(example_calib
    calib
)

add_executable(example_rectify example_rectify.cc)

target_link_libraries(example_rectify
    calib
)
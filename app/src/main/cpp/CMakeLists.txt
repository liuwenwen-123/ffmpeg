# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.18.1)
# 1) 设置.so和头文件存放的位置
set(jni_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../ffmpegLibs)
# Declares and names the project.

project("ffmpegdemo")
# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
add_library( # Sets the name of the library.
        ffmpegdemo

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)

# 2) 设置头文件的查找路径
target_include_directories(ffmpegdemo PRIVATE
        ${jni_DIR}/include)

# 3)添加一个.so依赖
add_library(avcodec SHARED IMPORTED)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libavcodec.so)

add_library(avformat SHARED IMPORTED)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libavformat.so)

add_library(avfilter SHARED IMPORTED)
set_target_properties(avfilter PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libavfilter.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libavutil.so)

add_library(swscale SHARED IMPORTED)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libswscale.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION
        ${jni_DIR}/lib/${ANDROID_ABI}/libswresample.so)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
# 4) 添加到链接库
target_link_libraries( # Specifies the target library.
        ffmpegdemo
        avcodec
        avformat
        avfilter
        avutil
        swscale
        swresample
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})

#[[
# 4) 添加到链接库
target_link_libraries( # Specifies the target library.
        native-lib
        avcodec
        avformat
        avfilter
        avutil
        swscale
        swresample
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})]]

#
# Copyright (C) 2016 The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

project(TENSORFLOW_DEMO)
cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_VERBOSE_MAKEFILE on)


if (ANDROID_ABI MATCHES "^armeabi-v7a$")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mfloat-abi=softfp -mfpu=neon")
elseif(ANDROID_ABI MATCHES "^arm64-v8a")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -ftree-vectorize")
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DSTANDALONE_DEMO_LIB \
                    -std=c++11 -fno-exceptions -frtti -O2 -Wno-narrowing \
                    -fPIE")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} \
                              -Wl,--allow-multiple-definition \
                              -Wl,--whole-archive -fPIE -v")
###########################################
MESSAGE(STATUS "Project: tensorflow_inference")

add_library(tensorflow_inference SHARED
            tensorflow/java/src/main/native/exception_jni.cc
            tensorflow/java/src/main/native/graph_jni.cc
            tensorflow/java/src/main/native/operation_builder_jni.cc
            tensorflow/java/src/main/native/operation_jni.cc
            tensorflow/java/src/main/native/saved_model_bundle_jni.cc
            tensorflow/java/src/main/native/session_jni.cc
            tensorflow/java/src/main/native/tensor_jni.cc
            tensorflow/java/src/main/native/tensorflow_jni.cc
            tensorflow/c/c_api.cc
            )

target_include_directories(tensorflow_inference PRIVATE
                           ${CMAKE_SOURCE_DIR}/
                           ${CMAKE_SOURCE_DIR}/third_party/eigen3
                           ${CMAKE_SOURCE_DIR}/third_party/protobuf
                           )

target_link_libraries(tensorflow_inference
                      android
                      log
                      jnigraphics
                      m
                      atomic
                      z)

#########################################################
MESSAGE(STATUS "Project: tensorflow_demo")

get_filename_component(SAMPLE_SRC_DIR  ${CMAKE_SOURCE_DIR}  ABSOLUTE)
file(GLOB_RECURSE tensorflow_demo_sources ${SAMPLE_SRC_DIR}/demo/*.*)
add_library(tensorflow_demo SHARED
            ${tensorflow_demo_sources})
target_include_directories(tensorflow_demo PRIVATE
                           ${CMAKE_SOURCE_DIR}/demo/)

target_link_libraries(tensorflow_demo
                      android
                      log
                      jnigraphics
                      m
                      atomic
                      z)

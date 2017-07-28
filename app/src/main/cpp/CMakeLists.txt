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
                    -fPIE \
                    -DHAVE_PTHREAD \
                    -DEIGEN_USE_SIMPLE_THREAD_POOL")
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
            tensorflow/core/platform/default/logging.cc
            tensorflow/core/framework/tensor_shape.cc
            tensorflow/core/framework/tensor.cc
            tensorflow/core/framework/resource_handle.cc
            tensorflow/core/framework/types.cc
            tensorflow/core/framework/log_memory.cc
            tensorflow/core/framework/allocator.cc
            tensorflow/core/platform/tensor_coding.cc
            tensorflow/core/platform/variant_coding.cc
            tensorflow/core/lib/strings/str_util.cc
            tensorflow/core/lib/core/coding.cc
            tensorflow/core/lib/core/stringpiece.cc
            tensorflow/core/graph/node_builder.cc
            tensorflow/core/framework/allocator_registry.cc
            tensorflow/core/framework/tracking_allocator.cc
            tensorflow/core/graph/graph.cc
            tensorflow/core/common_runtime/session.cc
            tensorflow/core/graph/subgraph.cc
            tensorflow/core/common_runtime/session_factory.cc
            tensorflow/core/common_runtime/session_options.cc
            tensorflow/core/common_runtime/shape_refiner.cc
            tensorflow/core/graph/edgeset.cc
            tensorflow/core/graph/graph_constructor.cc
            tensorflow/core/platform/env.cc
             tensorflow/core/platform/posix/env.cc
            tensorflow/core/platform/posix/load_library.cc
            tensorflow/core/platform/protobuf_util.cc
            tensorflow/core/framework/load_library.cc
            tensorflow/core/framework/op.cc
            tensorflow/core/framework/tensor_slice.pb.cc
            tensorflow/core/framework/function.cc
            tensorflow/core/protobuf/queue_runner.pb_text.cc
            tensorflow/core/lib/core/status.cc
            tensorflow/core/lib/io/path.cc
            tensorflow/core/util/equal_graph_def.cc
            tensorflow/core/lib/hash/hash.cc
            tensorflow/core/lib/core/arena.cc
            tensorflow/core/lib/strings/scanner.cc
            tensorflow/core/framework/node_def_util.cc
            tensorflow/core/framework/attr_value_util.cc
            tensorflow/core/framework/op_def_util.cc
            tensorflow/core/framework/op_def_util.cc
            tensorflow/core/framework/node_def_builder.cc
            tensorflow/core/framework/shape_inference.cc
            tensorflow/core/framework/variant.cc
            tensorflow/core/common_runtime/graph_runner.cc
            tensorflow/core/framework/common_shape_fns.cc
            tensorflow/core/framework/versions.cc
            tensorflow/core/graph/algorithm.cc
            tensorflow/core/graph/tensor_id.cc
            tensorflow/core/platform/env_time.cc
            tensorflow/core/platform/posix/env_time.cc
            tensorflow/core/platform/file_system.cc
            tensorflow/core/platform/posix/posix_file_system.cc
            tensorflow/core/framework/op_kernel.cc
            tensorflow/core/framework/op_def_builder.cc
            tensorflow/core/lib/strings/proto_text_util.cc
            tensorflow/core/framework/rendezvous.cc
            tensorflow/core/common_runtime/executor.cc
            tensorflow/core/framework/tensor_util.cc
            tensorflow/core/common_runtime/executor.cc
            tensorflow/core/lib/strings/strcat.cc
            tensorflow/core/lib/strings/stringprintf.cc
            tensorflow/core/framework/attr_value.pb.cc
            tensorflow/core/framework/tensor_shape.pb.cc
            tensorflow/core/framework/op_def.pb.cc
            tensorflow/core/framework/tensor.pb.cc
            tensorflow/core/framework/types.pb.cc
            tensorflow/core/framework/resource_handle.pb.cc
            tensorflow/core/framework/tensor_description.pb.cc
            tensorflow/core/framework/allocation_description.pb.cc
            tensorflow/core/framework/log_memory.pb.cc
            tensorflow/core/framework/graph.pb.cc
            tensorflow/core/protobuf/config.pb.cc
            tensorflow/core/util/tensor_format.cc
            tensorflow/core/lib/core/threadpool.cc
            tensorflow/core/platform/posix/error.cc
            tensorflow/core/framework/unique_tensor_references.cc
            tensorflow/core/framework/kernel_def.pb.cc
            tensorflow/core/util/memmapped_file_system.pb_text.cc
            tensorflow/core/util/device_name_utils.cc
            tensorflow/core/protobuf/cluster.pb.cc
            tensorflow/core/framework/step_stats.pb.cc
            tensorflow/core/framework/cost_graph.pb.cc
            tensorflow/core/framework/node_def.pb.cc
            tensorflow/core/framework/function.pb.cc
            tensorflow/core/framework/versions.pb.cc
            tensorflow/core/protobuf/queue_runner.pb.cc
            tensorflow/core/lib/core/error_codes.pb_text.cc
            tensorflow/core/framework/types.pb_text.cc
            tensorflow/core/protobuf/device_properties.pb_text.cc
            tensorflow/core/protobuf/tensor_bundle.pb_text.cc
            tensorflow/core/protobuf/saver.pb_text.cc
            tensorflow/core/protobuf/config.pb_text.cc
            tensorflow/core/protobuf/debug.pb_text.cc
            tensorflow/core/protobuf/rewriter_config.pb_text.cc
            tensorflow/core/protobuf/cluster.pb_text.cc
            tensorflow/core/util/memmapped_file_system.pb_text.cc
            tensorflow/core/platform/posix/port.cc
            tensorflow/core/lib/strings/numbers.cc
            tensorflow/core/lib/random/random.cc
            tensorflow/core/util/padding.cc
            tensorflow/core/util/saved_tensor_slice.pb_text.cc
            tensorflow/core/util/tensor_slice_reader_cache.cc
            tensorflow/core/framework/cancellation.cc
            tensorflow/core/protobuf/debug.pb.cc
            tensorflow/core/framework/graph_transfer_info.pb.cc
            tensorflow/core/protobuf/rewriter_config.pb.cc
            tensorflow/core/framework/node_def.pb_text.cc
            tensorflow/core/common_runtime/device_factory.cc
            tensorflow/core/util/tensor_slice_reader.cc
            tensorflow/core/util/saved_tensor_slice.pb.cc
            tensorflow/core/framework/cost_graph.pb_text.cc
            tensorflow/core/lib/core/error_codes.pb.cc
            tensorflow/core/framework/kernel_def.pb_text.cc
            tensorflow/core/protobuf/saver.pb.cc
            tensorflow/core/framework/graph.pb_text.cc
            tensorflow/core/framework/memory_types.cc
            tensorflow/core/common_runtime/step_stats_collector.cc
            tensorflow/core/platform/denormal.cc
            tensorflow/core/platform/setround.cc
            tensorflow/core/framework/tensor_reference.cc
            tensorflow/core/util/memmapped_file_system.pb.cc
            tensorflow/core/framework/device_attributes.pb_text.cc
            tensorflow/core/util/saved_tensor_slice_util.cc
            tensorflow/core/framework/tensor_slice.cc
            tensorflow/core/util/tensor_slice_set.cc
            tensorflow/core/framework/op_def.pb_text.cc
            tensorflow/core/lib/strings/ordered_code.cc
            tensorflow/core/framework/device_attributes.pb.cc
            tensorflow/core/graph/costmodel.cc
            tensorflow/core/common_runtime/costmodel_manager.cc
            tensorflow/core/framework/graph_transfer_info.pb_text.cc
            tensorflow/core/framework/tensor_description.pb_text.cc
            tensorflow/core/framework/function.pb_text.cc
            tensorflow/core/framework/step_stats.pb_text.cc
            tensorflow/core/framework/versions.pb_text.cc
            tensorflow/core/protobuf/device_properties.pb.cc
            tensorflow/core/protobuf/tensor_bundle.pb.cc
            tensorflow/core/framework/resource_handle.pb_text.cc
            tensorflow/core/framework/tensor.pb_text.cc
            tensorflow/core/framework/reader_base.pb_text.cc
            tensorflow/core/framework/variable.pb_text.cc
            tensorflow/core/lib/io/table.cc
            tensorflow/core/framework/reader_base.pb.cc
            tensorflow/core/framework/log_memory.pb_text.cc
            tensorflow/core/lib/io/block.cc
            tensorflow/core/lib/io/format.cc
            tensorflow/core/framework/variable.pb.cc
            tensorflow/core/framework/allocation_description.pb_text.cc
            tensorflow/core/framework/tensor_slice.pb_text.cc
            tensorflow/core/lib/hash/crc32c.cc
            tensorflow/core/lib/io/iterator.cc
            tensorflow/core/lib/io/two_level_iterator.cc
            tensorflow/core/lib/hash/crc32c_accelerate.cc
            tensorflow/core/framework/summary.pb_text.cc
            tensorflow/core/framework/attr_value.pb_text.cc
            tensorflow/core/framework/remote_fused_graph_execute_info.pb_text.cc
            tensorflow/core/framework/summary.pb.cc
            tensorflow/core/framework/tensor_shape.pb_text.cc
            tensorflow/core/framework/remote_fused_graph_execute_info.pb.cc
            tensorflow/core/util/mirror_pad_mode.cc
            third_party/protobuf/google/protobuf/message.cc
            third_party/protobuf/google/protobuf/stubs/once.cc
            third_party/protobuf/google/protobuf/generated_message_util.cc
            third_party/protobuf/google/protobuf/wire_format_lite.cc
            third_party/protobuf/google/protobuf/stubs/common.cc
            third_party/protobuf/google/protobuf/arena.cc
            third_party/protobuf/google/protobuf/io/coded_stream.cc
            third_party/protobuf/google/protobuf/map_field.cc
            third_party/protobuf/google/protobuf/descriptor.cc
            third_party/protobuf/google/protobuf/descriptor.pb.cc
            third_party/protobuf/google/protobuf/generated_message_reflection.cc
            third_party/protobuf/google/protobuf/io/zero_copy_stream_impl.cc
            third_party/protobuf/google/protobuf/stubs/strutil.cc
            third_party/protobuf/google/protobuf/message_lite.cc
            third_party/protobuf/google/protobuf/reflection_ops.cc
            third_party/protobuf/google/protobuf/unknown_field_set.cc
            third_party/protobuf/google/protobuf/wire_format.cc
            third_party/protobuf/google/protobuf/stubs/structurally_valid.cc
            third_party/protobuf/google/protobuf/stubs/status.cc
            third_party/protobuf/google/protobuf/descriptor_database.cc
            third_party/protobuf/google/protobuf/stubs/int128.cc
            third_party/protobuf/google/protobuf/stubs/stringprintf.cc
            third_party/protobuf/google/protobuf/stubs/substitute.cc
            third_party/protobuf/google/protobuf/io/strtod.cc
            third_party/protobuf/google/protobuf/dynamic_message.cc
            third_party/protobuf/google/protobuf/io/tokenizer.cc
            third_party/protobuf/google/protobuf/io/zero_copy_stream_impl_lite.cc
            third_party/protobuf/google/protobuf/text_format.cc
            third_party/protobuf/google/protobuf/stubs/stringpiece.cc
            third_party/protobuf/google/protobuf/extension_set.cc
            third_party/protobuf/google/protobuf/extension_set_heavy.cc
            third_party/protobuf/google/protobuf/io/zero_copy_stream.cc
            third_party/protobuf/google/protobuf/repeated_field.cc
            third_party/protobuf/google/protobuf/any.cc
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

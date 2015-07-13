cmake_minimum_required(VERSION 2.8)
project(lepton)
SET(CMAKE_CXX_FLAGS "-std=c++11")

IF(APPLE)
# not widely supported on linux yet
find_package(LibLZMA REQUIRED)
ELSE()
find_path(LIBLZMA_INCLUDE_DIRS lzma.h )
find_library(LIBLZMA_LIBRARIES lzma)
IF(NOT WIN32)
  SET(LIBLZMA_LIBRARIES ${LIBLZMA_LIBRARIES} pthread)
ENDIF()

ENDIF()

include_directories(
    ${LIBLZMA_INCLUDE_DIRS}
    ${PROJECT_SOURCE_DIR}/src/vp8/util
    ${PROJECT_SOURCE_DIR}/src/vp8/model
    ${PROJECT_SOURCE_DIR}/src/vp8/encoder
    ${PROJECT_SOURCE_DIR}/src/vp8/decoder)

add_executable(lepton
   src/lepton/base_coders.hh
   src/lepton/simple_encoder.hh
   src/lepton/bitops.cc
   src/lepton/smalljpeg.hh
   src/lepton/bitops.hh
   src/lepton/test_driver.cc
   src/lepton/component_info.hh
   src/lepton/htables.hh
   src/lepton/jpgcoder.cc
   src/lepton/uncompressed_components.cc
   src/lepton/jpgcoder.hh
   src/lepton/uncompressed_components.hh
   src/lepton/vp8_decoder.cc
   src/lepton/simple_decoder.cc
   src/lepton/vp8_decoder.hh
   src/lepton/simple_decoder.hh
   src/lepton/vp8_encoder.cc
   src/lepton/simple_encoder.cc
   src/lepton/vp8_encoder.hh
   src/io/Compression.cc
   src/io/Compression.hh
   src/io/Allocator.hh
   src/io/Error.hh
   src/io/Reader.hh
   src/io/ioutil.hh
   src/io/ioutil.cc
   src/io/DecoderPlatform.hh
   src/vp8/util/slice.hh
   src/vp8/util/mmap.cc
   src/vp8/util/mmap.hh
   src/vp8/util/handle.hh
   src/vp8/util/fixed_array.hh
   src/vp8/util/option.hh
   src/vp8/model/block.hh
   src/vp8/model/JpegArithmeticCoder.cc
   src/vp8/model/JpegArithmeticCoder.hh
   src/vp8/model/branch.hh
   src/vp8/model/context.hh
   src/vp8/model/legacy.hh
   src/vp8/model/model.cc
   src/vp8/model/model.hh
   src/vp8/model/numeric.cc
   src/vp8/model/numeric.hh
   src/vp8/model/jpeg_meta.hh
   src/vp8/encoder/encoder.cc
   src/vp8/encoder/bool_encoder.hh
   src/vp8/decoder/decoder.cc
   src/vp8/decoder/bool_decoder.hh
               )
target_link_libraries(lepton ${LIBLZMA_LIBRARIES})

add_executable(print-model
   src/lepton/print-model.cc
   src/vp8/util/mmap.cc
   src/vp8/util/mmap.hh
   src/vp8/model/model.cc
   src/vp8/encoder/encoder.cc
   src/vp8/decoder/decoder.cc
   src/vp8/model/numeric.cc
  )
add_executable(test_invariants
   src/lepton/test_invariants.cc
 )
cmake_minimum_required(VERSION 2.6)
project(kaggle-digit-recognizer-cnn)

option(DEBUG "DEBUG" OFF)

set(MODEL_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(SOURCES ${MODEL_SOURCE_DIR}/DigitRecognizerCNN.cpp)

if(DEBUG)
  message("Compilation with debug info (with ggdb3 flag)")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -ggdb3")
else()
  message("Compilation without debug info (without ggdb3 flag)")
endif()

add_executable(DigitRecognizerCNN ${SOURCES})
target_link_libraries(DigitRecognizerCNN
    PRIVATE ${MLPACK_LIBRARIES}
    PRIVATE ${ARMADILLO_LIBRARIES})
target_include_directories(DigitRecognizerCNN PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/../../")

if (NOT EXISTS ${PROJECT_BINARY_DIR}/../data)
    add_custom_command(TARGET DigitRecognizerCNN POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E tar xvzf ${CMAKE_CURRENT_SOURCE_DIR}/../kaggle_train_test_dataset.zip
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}/../)
endif()

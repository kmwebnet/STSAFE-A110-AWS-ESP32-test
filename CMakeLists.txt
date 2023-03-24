cmake_minimum_required(VERSION 3.16.0)

list(APPEND EXTRA_COMPONENT_DIRS
						 "$ENV{IDF_PATH}/examples/common_components/protocol_examples_common"
						 "${CMAKE_SOURCE_DIR}/components/stsafe-a110"
						 "${CMAKE_SOURCE_DIR}/components/esp-aws-iot/libraries/backoffAlgorithm"
						 "${CMAKE_SOURCE_DIR}/components/esp-aws-iot/libraries/coreMQTT"
						 "${CMAKE_SOURCE_DIR}/components/esp-aws-iot/libraries/common/posix_compat"
	)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(stsafea_esp32_aws)

#  Copyright (c) Microsoft Corporation. All rights reserved.
#  Licensed under the MIT License.

CMAKE_MINIMUM_REQUIRED(VERSION 3.10)
PROJECT(SphereBME280 C)

message("Project ${PROJECT_NAME}")

azsphere_configure_tools(TOOLS_REVISION "21.01")
azsphere_configure_api(TARGET_API_SET "8")

# Add the Bosch BME280 sensor lib
ADD_SUBDIRECTORY(BME280)

# Create executable
ADD_EXECUTABLE(${PROJECT_NAME} main.c azure_iot_utilities.c epoll_timerfd_utilities.c rgbled_utility.c parson.c)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} m azureiot applibs pthread gcc_s c BME280)

# This application targets the Seeed MT3620 Development Board so target for mt3620_rdb
# Target hardware for the sample.
set(TARGET_HARDWARE "mt3620_rdb")
set(TARGET_DEFINITION "mt3620_rdb.json")
azsphere_target_hardware_definition(${PROJECT_NAME} TARGET_DEFINITION "${TARGET_DEFINITION}")

# Add MakeImage post-build command
azsphere_target_add_image_package(${PROJECT_NAME})

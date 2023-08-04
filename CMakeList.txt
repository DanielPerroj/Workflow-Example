cmake_minimum_required(VERSION 3.5)

# Make PROJECT_SOURCE_DIR, PROJECT_BINARY_DIR, and PROJECT_NAME available.
set(PROJECT_NAME BACON-Firmware)
project(${PROJECT_NAME})
set(product "${PROJECT_NAME}" CACHE STRING "Product Name. (mostly used for output file name)")
set(PROJECT_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")

if(source)
	MESSAGE(STATUS "Using toolchain file: ${CMAKE_TOOLCHAIN_FILE}")
endif(source)

# ===============================================================================================
# Options. Turn on with 'cmake -Dmyvarname=ON'.
# ===============================================================================================
option(test "Builds all unit tests." OFF) # Makes boolean 'test' available.
option(mock "Builds all mocks." OFF) # Makes boolean 'mock' available.
option(source "Builds source code." ON) # Makes boolean 'source' available.
# ===============================================================================================
option(PROD "It is in Production mode when PROD = ON" OFF) 
option(PRODUCT_CONFIGURATION "It is the product configuration depending on which the sources are built." OFF)
option(DEBUG "It is in Release mode when Debug = OFF" OFF)
option(DEBUG_PRINT "It is in Release mode when DEBUG_PRINT = OFF" OFF)
option(DEBUG_PROTOCOL "It is in Release mode when DEBUG_PROTOCOL = OFF" OFF)
option(DEBUG_MQTT "It is in Release mode when DEBUG_MQTT = OFF" OFF)
option(WLAN_DISABLE_RECONNECT "Wlan reconnects automatically  when  WLAN_DISABLE_RECONNECT = OFF" OFF)
option(SEC_CSR_ON_EACH_POWERUP "CSR is not executed each time on powerup when SEC_CSR_ON_EACH_POWERUP = OFF" OFF)
option(MQTT_BROKER_HOST_URL "This is the host url of the backend to which the gateway connects to" NULL)
option(OTA_CERTIFICATE_PATH "This is the path of the certificate used to verify the OTA container" NULL)
option(EOL_USE_DEVELOP_CONFIGURATION "EOL process is required if EOL_USE_DEVELOP_CONFIGURATION = OFF" OFF)
option(EOL_NON_PERSISTENT_EOL_CONFIGURATION "EOL data is persistet if EOL_NON_PERSISTENT_EOL_CONFIGURATION = OFF" OFF)

# ===============================================================================================

# add tests
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/scripts/cmakeModules")
include(macros_gtest)
include(macros_src) 
if(test)
    enable_testing()
    include(external_gtest)
    add_subdirectory(testing)
endif(test)

# add sub directories
add_subdirectory(productConfig)

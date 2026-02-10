project(ExtendedKF)

cmake_minimum_required (VERSION 3.5)

add_definitions(-std=c++11)

set(CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS "${CXX_FLAGS}")

set(sources src/main.cpp src/tools.cpp src/FusionEKF.cpp src/kalman_filter.cpp src/tools.h src/FusionEKF.h src/kalman_filter.h)



if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin") 

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

include_directories(/usr/local/include)
link_directories(/usr/local/lib)

# Homebrew on Apple Silicon uses /opt/homebrew and formula is openssl@3
if(EXISTS "/opt/homebrew/opt/openssl@3/include")
	include_directories("/opt/homebrew/opt/openssl@3/include")
	link_directories("/opt/homebrew/opt/openssl@3/lib")
endif()

# Keep legacy /usr/local/opt path for Intel Homebrew
if(EXISTS "/usr/local/opt/openssl/include")
	include_directories("/usr/local/opt/openssl/include")
	link_directories("/usr/local/opt/openssl/lib")
endif()

link_directories(/usr/local/Cellar/libuv/1*/lib)
if(EXISTS "/opt/homebrew/opt/libuv/lib")
	link_directories("/opt/homebrew/opt/libuv/lib")
	link_directories("/opt/homebrew/lib")
endif()

endif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin") 


add_executable(ExtendedKF ${sources})

target_link_libraries(ExtendedKF z ssl uv uWS)

SET(CMAKE_BUILD_TYPE "Debug")
INCLUDE(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
IF(COMPILER_SUPPORTS_CXX11)
    add_definitions("-std=c++11")
ENDIF()

FIND_PACKAGE(CURL REQUIRED)
FIND_PACKAGE(OpenSSL REQUIRED)
SET(CMAKE_CXX_FLAGES_DEBUG "${CMAKE_CXX_FLAGES} -O0   -pipe -Wall -fno-strict-aliasing -D_GNU_SOURCE -D_REENTRANT -fPIC -g -ggdb")
SET(CMAKE_CXX_FLAGES_RELEASE"${CMAKE_CXX_FLAGES} -O0   -pipe -Wall -fno-strict-aliasing -D_GNU_SOURCE -D_REENTRANT -fPIC ")
INCLUDE_DIRECTORIES(../inc/)
AUX_SOURCE_DIRECTORY(. SRC) 

INCLUDE_DIRECTORIES(${CURL_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${OPENSSL_INCLUDE_DIR})

ADD_LIBRARY(CMQ SHARED ${SRC})
ADD_LIBRARY(CMQ_STATIC STATIC ${SRC})

TARGET_LINK_LIBRARIES(CMQ ${OPENSSL_LIBRARIES})
TARGET_LINK_LIBRARIES(CMQ ${CURL_LIBRARIES})

SET_TARGET_PROPERTIES(CMQ_STATIC PROPERTIES OUTPUT_NAME "CMQ")
SET_TARGET_PROPERTIES(CMQ PROPERTIES CLEAN_DIRECT_OUTPUT 1)
SET_TARGET_PROPERTIES(CMQ_STATIC PROPERTIES CLEAN_DIRECT_OUTPUT 1)


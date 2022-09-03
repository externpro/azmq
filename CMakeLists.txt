cmake_minimum_required(VERSION 3.12)
project(azmq)

include(AzmqCPack.cmake)
include(flags OPTIONAL)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++11")

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
endif()

if (${CMAKE_CXX_PLATFORM_ID} STREQUAL "Windows")
    # Build for Windows Vista / Windows Server 2008 and later
    add_definitions(-D_WIN32_WINNT=0x0600)

    add_definitions(-DNOMINMAX -DWIN32_LEAN_AND_MEAN -D_WINSOCK_DEPRECATED_NO_WARNINGS -D_SCL_SECURE_NO_WARNINGS)

    if (NOT DEFINED Boost_USE_STATIC_LIBS)
        set(Boost_USE_STATIC_LIBS ON) # On Windows, Boost is compiled as static libraries by default
    endif()
endif()

if (ZMQ_STATIC)
    add_definitions(-D ZMQ_STATIC)
endif()

set(ADDITIONAL_LIBS "")

if(USE_LIBCXX)
    set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -lc++abi")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -lc++abi")
    set(ADDITIONAL_LIBS "pthread")
endif()

if(COMMAND xpFindPkg)
    set(XP_MODULE_PATH ${CMAKE_MODULE_PATH})
    xpFindPkg(PKGS boost libzmq)
    set(Boost_LIBRARIES Boost::system Boost::thread)
    set(ZeroMQ_LIBRARIES xpro::libzmq-static)
else()
    set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/config")
    find_package(Boost 1.48 COMPONENTS system date_time thread chrono random REQUIRED)
    find_package(ZeroMQ 4.0 REQUIRED)
endif()

set(CMAKE_THREAD_PREFER_PTHREAD TRUE)
find_package(Threads REQUIRED)

if (NOT CMAKE_THREAD_LIBS_INIT)
    set(CMAKE_THREAD_LIBS_INIT "")
endif()

include_directories(${Boost_INCLUDE_DIRS}
                    ${ZeroMQ_INCLUDE_DIRS}
                    ${PROJECT_SOURCE_DIR})

if(NOT AZMQ_NO_TESTS)
    enable_testing()
endif()



if(NOT AZMQ_NO_TESTS)
    add_subdirectory(test)
    add_subdirectory(doc)
endif()

include(GNUInstallDirs)
add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
target_link_libraries(${PROJECT_NAME} INTERFACE ${Boost_LIBRARIES} ${ZeroMQ_LIBRARIES})
install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}-targets)
if(DEFINED XP_NAMESPACE)
    set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
endif()
if(NOT DEFINED XP_INSTALL_CMAKEDIR)
  set(XP_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
install(EXPORT ${PROJECT_NAME}-targets ${nameSpace} DESTINATION ${XP_INSTALL_CMAKEDIR})

install(DIRECTORY ${PROJECT_SOURCE_DIR}/azmq
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

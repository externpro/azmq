cmake_minimum_required(VERSION 3.22.1...4.3)
# -- v3.22.1 is the default version in the current ubuntu lts release

file(READ "${CMAKE_CURRENT_LIST_DIR}/azmq/version.hpp" AZMQ_VERSION_HPP)
string(REGEX MATCH "#define[ \t]+AZMQ_VERSION_MAJOR[ \t]+([0-9]+)" _ "${AZMQ_VERSION_HPP}")
set(AZMQ_VERSION_MAJOR "${CMAKE_MATCH_1}")
string(REGEX MATCH "#define[ \t]+AZMQ_VERSION_MINOR[ \t]+([0-9]+)" _ "${AZMQ_VERSION_HPP}")
set(AZMQ_VERSION_MINOR "${CMAKE_MATCH_1}")
string(REGEX MATCH "#define[ \t]+AZMQ_VERSION_PATCH[ \t]+([0-9]+)" _ "${AZMQ_VERSION_HPP}")
set(AZMQ_VERSION_PATCH "${CMAKE_MATCH_1}")

project(azmq VERSION ${AZMQ_VERSION_MAJOR}.${AZMQ_VERSION_MINOR}.${AZMQ_VERSION_PATCH} LANGUAGES CXX)

if (PROJECT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(CMAKE_MASTER_PROJECT YES)
endif ()

# -- options
option(AZMQ_BUILD_TESTS "run unit tests" ${CMAKE_MASTER_PROJECT})
option(BUILD_SHARED_LIBS "use the libzmq static or shared" yes)
option(AZMQ_DEVELOPER "Add verbose warning for developers " ${CMAKE_MASTER_PROJECT})
option(AZMQ_BUILD_DOC "Add documentation example" ${CMAKE_MASTER_PROJECT})

# -- config settings --
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# --- dependencies  --
if(COMMAND xpFindPkg)
    xpFindPkg(PKGS Boost libzmq)
    set(boost_LIBS Boost::container Boost::random Boost::thread)
    set(libzmq_LIB libzmq::libzmq-static)
else()
find_package(
    Boost 1.68
    COMPONENTS date_time
               thread
               chrono
               random
    REQUIRED)

# Try to find Boost::system
find_package(Boost COMPONENTS system)
if (NOT Boost_SYSTEM_FOUND)
    message(WARNING "Boost::system not found, some examples may not compile")
endif()

include(FindAzmqLibzmq.cmake)
set(libzmq_LIB Azmq::libzmq)
endif()

set(CMAKE_THREAD_PREFER_PTHREAD TRUE)
find_package(Threads REQUIRED)

# --- create library --
add_library(${PROJECT_NAME} INTERFACE)
add_library(Azmq::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

target_link_libraries(${PROJECT_NAME} INTERFACE ${libzmq_LIB} Boost::boost ${boost_LIBS} ${Boost_SYSTEM_LIBRARY} ${CMAKE_THREAD_LIBS_INIT})
target_include_directories(${PROJECT_NAME} INTERFACE "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
                                                     "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")

if (AZMQ_DEVELOPER)
    include(cmake/CompilerWarnings.cmake)
    add_more_warnings_to(${PROJECT_NAME})
endif ()

if (CMAKE_MASTER_PROJECT AND NOT CMAKE_SKIP_INSTALL_RULES)
    if(NOT COMMAND xpExternPackage)
    include(AzmqCPack.cmake)
    endif()

    include(CMakePackageConfigHelpers)

    # generate the version file for the config file
    write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
                                     VERSION ${PROJECT_VERSION} COMPATIBILITY SameMajorVersion)

    # install the public headers
    install(DIRECTORY azmq DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

    install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}Targets RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
    if(COMMAND xpExternPackage)
      xpExternPackage(TARGETS_FILE ${PROJECT_NAME}Targets NO_EXPORT
        LIBRARIES ${PROJECT_NAME}
        BASE v${PROJECT_VERSION} XPDIFF "patch"
        WEB "https://zeromq.org/" UPSTREAM "github.com/zeromq/azmq"
        DESC "provides Boost Asio style bindings for ZeroMQ"
        LICENSE "[BSL-1.0](https://github.com/zeromq/azmq/blob/master/LICENSE-BOOST_1_0 'Boost Software License 1.0')"
        )
      install(EXPORT ${PROJECT_NAME}Targets NAMESPACE ${PROJECT_NAME}:: DESTINATION ${CMAKE_INSTALL_CMAKEDIR})
    else()
    configure_file(${PROJECT_NAME}Config.cmake "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake" COPYONLY)
    set(ConfigPackageLocation lib/cmake/${PROJECT_NAME})
    install(EXPORT ${PROJECT_NAME}Targets NAMESPACE Azmq:: DESTINATION ${ConfigPackageLocation})
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
                  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake DESTINATION ${ConfigPackageLocation})

    install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/FindAzmqLibzmq.cmake" DESTINATION ${ConfigPackageLocation})
    endif()
endif ()

if (AZMQ_BUILD_TESTS)
    enable_testing()
    message(STATUS "Building Tests. Be sure to check out test/constexpr_tests for constexpr testing")
    add_subdirectory(test)
endif ()

if (AZMQ_BUILD_DOC)
  add_subdirectory(doc)
endif()

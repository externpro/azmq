project(test_socket)

add_executable(${PROJECT_NAME} main.cpp)

find_package(Boost COMPONENTS context)

target_link_libraries(${PROJECT_NAME} Azmq::azmq Boost::context)

add_catch_test(${PROJECT_NAME})

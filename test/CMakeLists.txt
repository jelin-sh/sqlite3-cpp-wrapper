project(SQLiteCPP_Test)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(SQLiteCPP_Test main.cpp)
target_link_libraries(SQLiteCPP_Test SQLiteCpp::SQLiteCpp)

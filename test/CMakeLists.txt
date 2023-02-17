project(SQLiteCPP_Test)

add_executable(SQLiteCPP_Test main.cpp)
target_link_libraries(SQLiteCPP_Test SQLiteCpp::SQLiteCpp)

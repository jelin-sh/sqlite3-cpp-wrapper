cmake_minimum_required(VERSION 3.11)

project(sqlite3_cpp_wrapper)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


## add sqlite3
include(FetchContent)
message(STATUS "Download & Install SQLite3 ...")
#set(FETCHCONTENT_QUIET FALSE)
fetchcontent_declare(
    sqlite3
    GIT_REPOSITORY https://gitee.com/skjsnb/sqlite3-amalgamation.git
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
set(NAME sqlite3)
FetchContent_GetProperties(${NAME})
if (NOT ${NAME}_POPULATED)
    FetchContent_Populate(${NAME})
    add_subdirectory(${${NAME}_SOURCE_DIR} ${${NAME}_BINARY_DIR} EXCLUDE_FROM_ALL)
endif ()
message(STATUS "Download & Install SQLite3 ... Done")

# list of sources files of the library
set(SQLITECPP_SRC
    ${PROJECT_SOURCE_DIR}/src/Backup.cpp
    ${PROJECT_SOURCE_DIR}/src/Column.cpp
    ${PROJECT_SOURCE_DIR}/src/Database.cpp
    ${PROJECT_SOURCE_DIR}/src/Exception.cpp
    ${PROJECT_SOURCE_DIR}/src/Savepoint.cpp
    ${PROJECT_SOURCE_DIR}/src/Statement.cpp
    ${PROJECT_SOURCE_DIR}/src/Transaction.cpp
)

# list of header files of the library
set(SQLITECPP_INC
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/SQLiteCpp.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Assertion.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Backup.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Column.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Database.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Exception.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Savepoint.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Statement.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/Transaction.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/VariadicBind.h
    ${PROJECT_SOURCE_DIR}/include/SQLiteCpp/ExecuteMany.h
    )

# add sources of the wrapper as a "SQLiteCpp" static library
add_library(SQLiteCpp ${SQLITECPP_SRC} ${SQLITECPP_INC})
target_link_libraries(SQLiteCpp PUBLIC sqlite3::sqlite3)
target_include_directories(SQLiteCpp PUBLIC ${PROJECT_SOURCE_DIR}/include)

add_library(SQLiteCpp::SQLiteCpp ALIAS SQLiteCpp)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
    add_subdirectory(test)
endif()

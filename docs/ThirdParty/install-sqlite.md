# SQLite3源码编译安装教程

## Windows下安装

1. 在官网下载[sqlite-amalgamation-3490100.zip](https://sqlite.org/download.html)并解压。

2. 参照[sqlite-amalgamation](https://github.com/azadkuh/sqlite-amalgamation)项目编写CMakeLists.txt如下，注意修改版本号：

```
cmake_minimum_required(VERSION 3.8)
project(SQLite3
    VERSION   3.49.1
    LANGUAGES C
    )

include(GNUInstallDirs)

#------------------------------------------------------------------------------
# build options and optional modules:
option(SQLITE_ENABLE_COLUMN_METADATA "enables column metadata"                         OFF)
option(SQLITE_ENABLE_DBSTAT_VTAB     "enables dbstat virtual table"                    OFF)
option(SQLITE_ENABLE_FTS3            "enables full text searches version 3"            OFF)
option(SQLITE_ENABLE_FTS4            "enables full text searches version 3 & 4"        OFF)
option(SQLITE_ENABLE_FTS5            "enables full text searches version 5"            OFF)
option(SQLITE_ENABLE_GEOPOLY         "enables Geopoly extention"                       OFF)
option(SQLITE_ENABLE_ICU             "enables international components for unicode"    OFF)
option(SQLITE_ENABLE_MATH_FUNCTIONS  "enables the built-in SQL math functions"         ON)
option(SQLITE_ENABLE_RBU             "enables resumable bulk update extension"         OFF)
option(SQLITE_ENABLE_RTREE           "enables R*TRee index extension"                  OFF)
option(SQLITE_ENABLE_STAT4           "enhances query planner under certain situations" OFF)
option(SQLITE_OMIT_DECLTYPE          "omit declared type of columns"                   ON)
option(SQLITE_OMIT_JSON              "disables JSON SQL functions"                     OFF)
option(SQLITE_RECOMMENDED_OPTIONS    "compile by SQLite3 recommended options"          ON)
option(SQLITE_USE_URI                "enables the default URI filename processing"     OFF)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Release or Debug?" FORCE)
endif()

if(SQLITE_ENABLE_COLUMN_METADATA AND SQLITE_OMIT_DECLTYPE)
    message(FATAL_ERROR "please unset the SQLITE_OMIT_DECLTYPE if you want to\
    compile with SQLITE_ENABLE_COLUMN_METADATA,\
    compiling with both options ON, is not recommended.")
endif()

#------------------------------------------------------------------------------

# SQLite3 as static library:
add_library(${PROJECT_NAME} STATIC sqlite3.c)
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_NAME   sqlite3
    PUBLIC_HEADER sqlite3.h
    DEBUG_POSTFIX d
    )
target_include_directories(${PROJECT_NAME} PUBLIC $<INSTALL_INTERFACE:include>)
target_compile_definitions(${PROJECT_NAME} PUBLIC # inject user's options
    $<BUILD_INTERFACE:
        $<$<BOOL:${SQLITE_ENABLE_COLUMN_METADATA}>:SQLITE_ENABLE_COLUMN_METADATA>
        $<$<BOOL:${SQLITE_ENABLE_DBSTAT_VTAB}>:SQLITE_ENABLE_DBSTAT_VTAB>
        $<$<BOOL:${SQLITE_ENABLE_FTS3}>:SQLITE_ENABLE_FTS3>
        $<$<BOOL:${SQLITE_ENABLE_FTS4}>:SQLITE_ENABLE_FTS4>
        $<$<BOOL:${SQLITE_ENABLE_FTS5}>:SQLITE_ENABLE_FTS5>
        $<$<BOOL:${SQLITE_ENABLE_GEOPOLY}>:SQLITE_ENABLE_GEOPOLY>
        $<$<BOOL:${SQLITE_ENABLE_ICU}>:SQLITE_ENABLE_ICU>
        $<$<BOOL:${SQLITE_ENABLE_MATH_FUNCTIONS}>:SQLITE_ENABLE_MATH_FUNCTIONS>
        $<$<BOOL:${SQLITE_ENABLE_RBU}>:SQLITE_ENABLE_RBU>
        $<$<BOOL:${SQLITE_ENABLE_RTREE}>:SQLITE_ENABLE_RTREE>
        $<$<BOOL:${SQLITE_ENABLE_STAT4}>:SQLITE_ENABLE_STAT4>
        $<$<BOOL:${SQLITE_OMIT_DECLTYPE}>:SQLITE_OMIT_DECLTYPE>
        $<$<BOOL:${SQLITE_OMIT_JSON}>:SQLITE_OMIT_JSON>
        $<$<BOOL:${SQLITE_USE_URI}>:SQLITE_USE_URI>
        $<$<BOOL:${SQLITE_RECOMMENDED_OPTIONS}>:
            SQLITE_DEFAULT_MEMSTATUS=0
            SQLITE_DEFAULT_WAL_SYNCHRONOUS=1
            SQLITE_DQS=0
            SQLITE_LIKE_DOESNT_MATCH_BLOBS
            SQLITE_MAX_EXPR_DEPTH=0
            SQLITE_OMIT_DEPRECATED
            SQLITE_OMIT_PROGRESS_CALLBACK
            SQLITE_OMIT_SHARED_CACHE
            SQLITE_USE_ALLOCA
        >
    >
    )

# platform/compiler specific settings
if(CMAKE_SYSTEM_NAME MATCHES Linux)
    find_package(Threads REQUIRED)
    target_link_libraries(${PROJECT_NAME} INTERFACE Threads::Threads ${CMAKE_DL_LIBS})
elseif(WIN32 AND ${CMAKE_SIZEOF_VOID_P} LESS 8) # this is a 32bit windows
    option(BUILD_WITH_XPSDK "build for old 32bit (WinXP/2003) targets" OFF)
    if(BUILD_WITH_XPSDK)
        target_compile_definitions(${PROJECT_NAME} PUBLIC
            $<BUILD_INTERFACE:
                -DSQLITE_OS_WINRT=0 -D_WIN32_WINNT=0x0502 -DWINVER=0x0502
            >
            )
    endif()
endif()

#------------------------------------------------------------------------------
configure_file(sqlite3_config.h.in ${CMAKE_BINARY_DIR}/sqlite3_config.h)

install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}Config
    ARCHIVE       DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY       DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    )
install(EXPORT  ${PROJECT_NAME}Config
    NAMESPACE   SQLite::
    DESTINATION cmake
    )
install(FILES
    ${CMAKE_BINARY_DIR}/sqlite3_config.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    )

#------------------------------------------------------------------------------
# SQLite3 shell application:
option(BUILD_SHELL "build SQLite3 shell application" OFF)
if(BUILD_SHELL)
    add_executable(shell_app shell.c)
    set_target_properties(shell_app PROPERTIES OUTPUT_NAME sqlite3)
    target_link_libraries(shell_app PRIVATE ${PROJECT_NAME})
    if(UNIX)
        if(CMAKE_BUILD_TYPE STREQUAL Release)
            add_custom_command(TARGET shell_app POST_BUILD
                COMMAND ${CMAKE_STRIP} sqlite3
                )
        endif()
    elseif(MSVC)
        option(BUILD_SHELL_STATIC "build shell by static c/c++ runtime" ON)
        foreach(flag CMAKE_C_FLAGS_RELEASE CMAKE_C_FLAGS_MINSIZEREL CMAKE_C_FLAGS_DEBUG)
            if(BUILD_SHELL_STATIC)
                string(REGEX REPLACE "/MD" "/MT" ${flag} "${${flag}}")
            else()
                string(REGEX REPLACE "/MT" "/MD" ${flag} "${${flag}}")
            endif()
            set(${flag} "${${flag}}" CACHE STRING "msvc flags" FORCE)
        endforeach()
    endif()
    install(TARGETS shell_app
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        )
endif()
```

编写sqlite3_config.h.in如下：

```
/* the compile-time options used to build the SQLite3 library
 *
 * https://github.com/azadkuh/sqlite-amalgamation
 */

#ifndef SQLITE3_CONFIG_H
#define SQLITE3_CONFIG_H


#cmakedefine SQLITE_ENABLE_COLUMN_METADATA
#cmakedefine SQLITE_ENABLE_DBSTAT_VTAB
#cmakedefine SQLITE_ENABLE_FTS3
#cmakedefine SQLITE_ENABLE_FTS4
#cmakedefine SQLITE_ENABLE_FTS5
#cmakedefine SQLITE_ENABLE_GEOPOLY
#cmakedefine SQLITE_ENABLE_ICU
#cmakedefine SQLITE_ENABLE_MATH_FUNCTIONS
#cmakedefine SQLITE_ENABLE_RBU
#cmakedefine SQLITE_ENABLE_RTREE
#cmakedefine SQLITE_ENABLE_STAT4
#cmakedefine SQLITE_OMIT_DECLTYPE
#cmakedefine SQLITE_OMIT_JSON
#cmakedefine SQLITE_USE_URI

#cmakedefine SQLITE_RECOMMENDED_OPTIONS
#if defined(SQLITE_RECOMMENDED_OPTIONS)
#   define SQLITE_DQS 0
#   define SQLITE_DEFAULT_MEMSTATUS 0
#   define SQLITE_DEFAULT_WAL_SYNCHRONOUS 1
#   define SQLITE_LIKE_DOESNT_MATCH_BLOBS
#   define SQLITE_MAX_EXPR_DEPTH 1
#   define SQLITE_OMIT_DEPRECATED
#   define SQLITE_OMIT_PROGRESS_CALLBACK
#   define SQLITE_OMIT_SHARED_CACHE
#   define SQLITE_USE_ALLOCA
#endif /* SQLITE_RECOMMENDED_OPTIONS */


#endif /* SQLITE3_CONFIG_H */
```

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/sqlite-amalgamation-3490100
- Where to build the binaries: F:/third_party_build/sqlite-amalgamation-3490100/build
- CMAKE_INSTALL_PREFIX: F:/third_party/SQLite3

3. 用Visual Studio打开build目录下生成的SQLite3.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## Linux下安装

1. 和Windows中的步骤一样创建CMakeLists.txt和sqlite3_config.h.in

2. 进入源代码目录执行下面命令：

```
ccmake -S . -B ./build_linux -DCMAKE_POSITION_INDEPENDENT_CODE=ON
```

注意：如果不加`-DCMAKE_POSITION_INDEPENDENT_CODE=ON`，编译依赖了SQLite3的项目时会出现如下错误：

```
/usr/bin/ld: SQLite3/lib/libsqlite3.a(sqlite3.c.o): warning: relocation against `sqlite3_data_directory' in read-only section `.text'
/usr/bin/ld: SQLite3/lib/libsqlite3.a(sqlite3.c.o): relocation R_X86_64_PC32 against symbol `sqlite3_free' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: final link failed: bad value
collect2: error: ld returned 1 exit status
```

3. 输入c，修改完配置，再输入g：

- CMAKE_INSTALL_PREFIX: /path/to/third_party_linux/SQLite3

4. 编译并安装：

```
cmake --build ./build_linux
cmake --install ./build_linux
```

## CMakeList.txt中使用

1. 引入SQLite3依赖：

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

# SQLite3
find_package(SQLite3 CONFIG REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE SQLite::SQLite3)
```

3. 编写测试代码，创建一个表格：

```c
#include <iostream>
#include <sqlite3.h>

int main() {
   sqlite3 *db;
   char *errMsg = 0;
   int rc;

   rc = sqlite3_open("test.db", &db);
   if( rc ) {
      fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));
      return(0);
   } else {
      fprintf(stdout, "Opened database successfully\n");
   }

   // 创建表格
   char *sql = "CREATE TABLE students(" \
               "ID INT PRIMARY KEY     NOT NULL," \
               "NAME           TEXT    NOT NULL," \
               "AGE            INT     NOT NULL," \
               "ADDRESS        CHAR(50)," \
               "GRADE          INT );";

   rc = sqlite3_exec(db, sql, 0, 0, &errMsg);
   if( rc != SQLITE_OK ){
      fprintf(stderr, "SQL error: %s\n", errMsg);
      sqlite3_free(errMsg);
   } else {
      fprintf(stdout, "Table created successfully\n");
   }

   sqlite3_close(db);
}
```

#### 参考资料

- [sqlite-amalgamation](https://github.com/azadkuh/sqlite-amalgamation)
- [SQLite 在Windows上使用C语言操作SQLite数据库](https://geek-docs.com/sqlite/sqlite-questions/170_sqlite_using_sqlite_with_c_on_windows.html)
- [What is the idiomatic way in CMAKE to add the -fPIC compiler option?](https://stackoverflow.com/questions/38296756/what-is-the-idiomatic-way-in-cmake-to-add-the-fpic-compiler-option)

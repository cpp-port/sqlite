cmake_minimum_required(VERSION 3.16)
project(sqlite C)


list(APPEND library_source
       sqlite3.c
)


add_library(${PROJECT_NAME}
  SHARED ${library_source}
)
add_library(static_${PROJECT_NAME}
        STATIC ${library_source}
)

target_compile_definitions(${PROJECT_NAME} PRIVATE _AURA_SQLITE)
target_compile_definitions(static_${PROJECT_NAME} PRIVATE _AURA_SQLITE)

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(static_${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
set_target_properties(static_${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON)

if(CMAKE_SYSTEM_NAME STREQUAL "SunOS")
  target_compile_definitions(${PROJECT_NAME} PRIVATE _REENTRANT)
  target_compile_definitions(static_${PROJECT_NAME} PRIVATE _REENTRANT)
endif()

set(THREADS_PREFER_PTHREAD_FLAG TRUE)
find_package(Threads REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads ${CMAKE_DL_LIBS})
target_link_libraries(static_${PROJECT_NAME} PUBLIC Threads::Threads ${CMAKE_DL_LIBS})

if(this_system_libraries)
  target_link_libraries(${PROJECT_NAME} PRIVATE ${this_system_libraries})
endif()



list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR})
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/include/pixman)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../../../operating_system/operating_system-${PLATFORM_NAME}/configuration/${PROJECT_NAME})


target_include_directories(${PROJECT_NAME} PRIVATE ${library_include_directories})
target_include_directories(static_${PROJECT_NAME} PRIVATE ${library_include_directories})



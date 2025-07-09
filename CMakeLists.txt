cmake_minimum_required(VERSION 3.16)
project(sqlite C)


list(APPEND library_source
        shell.c
       sqlite3.c
        version.c
)


add_library(${PROJECT_NAME}
  SHARED ${library_source}
)
add_library(static_${PROJECT_NAME}
        STATIC ${library_source}
)

target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_CONFIG_H _AURA_SQLITE)
target_compile_definitions(static_${PROJECT_NAME} PRIVATE HAVE_CONFIG_H _AURA_SQLITE)

if(${this_system_libraries})
  target_link_libraries(${PROJECT_NAME} PRIVATE ${this_system_libraries})
endif()



list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR})
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/include/pixman)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../../../operating_system/operating_system-${PLATFORM_NAME}/configuration/${PROJECT_NAME})


target_include_directories(${PROJECT_NAME} PRIVATE ${library_include_directories})
target_include_directories(static_${PROJECT_NAME} PRIVATE ${library_include_directories})



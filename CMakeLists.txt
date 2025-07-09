cmake_minimum_required(VERSION 3.16)
project(pixman C)


list(APPEND library_source
       pixman/pixman.c
       pixman/pixman-access.c
       pixman/pixman-access-accessors.c
       pixman/pixman-arm.c
       pixman/pixman-bits-image.c
       pixman/pixman-combine32.c
       pixman/pixman-combine-float.c
       pixman/pixman-conical-gradient.c
       pixman/pixman-edge.c
       pixman/pixman-edge-accessors.c
       pixman/pixman-fast-path.c
       pixman/pixman-filter.c
       pixman/pixman-general.c
       pixman/pixman-glyph.c
       pixman/pixman-gradient-walker.c
       pixman/pixman-image.c
       pixman/pixman-implementation.c
       pixman/pixman-linear-gradient.c
       pixman/pixman-matrix.c
       pixman/pixman-mips.c
       pixman/pixman-mmx.c
       pixman/pixman-noop.c
       pixman/pixman-ppc.c
       pixman/pixman-radial-gradient.c
       #pixman/pixman-region.c
       pixman/pixman-region16.c
       pixman/pixman-region32.c
       pixman/pixman-solid-fill.c
       pixman/pixman-sse2.c
       pixman/pixman-ssse3.c
       pixman/pixman-timer.c
       pixman/pixman-trap.c
       pixman/pixman-utils.c
       pixman/pixman-version.h.in
)


if(${ALTIVEC})

  list(APPEND library_source
          pixman/pixman-vmx.c
  )

endif()
if(${IS_ARM_NEON})

  list(APPEND library_source
          pixman/pixman-arm-neon.c
          pixman/pixman-arm-neon-asm.S
          pixman/pixman-arm-neon-asm-bilinear.S
  )

endif()
if(${IS_ARM_SIMD})

  list(APPEND library_source
          pixman/pixman-arm-simd.c
          pixman/pixman-arm-simd-asm.S
          pixman/pixman-arm-simd-asm-scaled.S

  )

endif()

if(${IS_X86_ARCH})

  list(APPEND library_source
          pixman/pixman-x86.c

  )

endif()

if(${IS_MIPS})
  list(APPEND library_source

pixman/pixman-mips-dspr2.c
pixman/pixman-mips-dspr2-asm.S
pixman/pixman-mips-memcpy-asm.S
  )

endif()

add_library(${PROJECT_NAME}
  SHARED ${library_source}
)
add_library(static_${PROJECT_NAME}
        STATIC ${library_source}
)

target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_CONFIG_H)
target_compile_definitions(static_${PROJECT_NAME} PRIVATE HAVE_CONFIG_H)

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



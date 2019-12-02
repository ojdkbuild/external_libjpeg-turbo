# Copyright 2016, alex at staticlibs.net
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# this file is based on https://github.com/ojdkbuild/lookaside_libjpeg-turbo/blob/95ce05fc3a001d82bca428faa69ad219bc017f72/CMakeLists.txt
cmake_minimum_required ( VERSION 2.8.12 )
project ( libjpeg-turbo C )

if ( NOT DEFINED ${PROJECT_NAME}_LOOKASIDE_DIR )
    set ( ${PROJECT_NAME}_LOOKASIDE_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../lookaside_libjpeg-turbo CACHE INTERNAL "" )
endif ( )

# standalone build
if ( NOT DEFINED STATICLIB_TOOLCHAIN )
    if ( NOT DEFINED ${PROJECT_NAME}_TOOLCHAIN )
        if ( WIN32 )
            if ( CMAKE_SIZEOF_VOID_P EQUAL 8 )
                set ( ${PROJECT_NAME}_TOOLCHAIN "windows_amd64_msvc" )
            else ( )
                set ( ${PROJECT_NAME}_TOOLCHAIN "windows_i386_msvc" )
            endif ( )
        elseif ( APPLE )
            set ( ${PROJECT_NAME}_TOOLCHAIN "macosx_amd64_clang" )
        elseif ( UNIX )
            set ( ${PROJECT_NAME}_TOOLCHAIN "linux_amd64_gcc" )
        else ( )
            message ( FATAL_ERROR "Cannot determine toolchain to use, please specify [${PROJECT_NAME}_TOOLCHAIN]" )
        endif ( )
    endif ( )
else ( )
    set ( ${PROJECT_NAME}_TOOLCHAIN ${STATICLIB_TOOLCHAIN} )
endif ( )

# library
set ( ${PROJECT_NAME}_INCLUDE 
        ${CMAKE_CURRENT_LIST_DIR}/include_${${PROJECT_NAME}_TOOLCHAIN}
        ${${PROJECT_NAME}_LOOKASIDE_DIR} )
set ( ${PROJECT_NAME}_SRC
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcapimin.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcapistd.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jccoefct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jccolor.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcdctmgr.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jchuff.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcinit.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcmainct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcmarker.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcmaster.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcomapi.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcparam.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcphuff.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcprepct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcsample.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jctrans.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdapimin.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdapistd.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdatadst.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdatasrc.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdcoefct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdcolor.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jddctmgr.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdhuff.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdinput.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdmainct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdmarker.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdmaster.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdmerge.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdphuff.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdpostct.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdsample.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdtrans.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jerror.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jfdctflt.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jfdctfst.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jfdctint.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jidctflt.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jidctfst.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jidctint.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jidctred.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jquant1.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jquant2.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jutils.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jmemmgr.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jmemnobs.c 
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jaricom.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jcarith.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/jdarith.c )
set ( ${PROJECT_NAME}_DEFINITIONS )
set ( ${PROJECT_NAME}_OPTIONS )

if ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "windows_[^_]+_[^_]+" )
    # simd
    list ( APPEND  ${PROJECT_NAME}_DEFINITIONS -DWITH_SIMD )
    set ( NASM nasm )
    if ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "windows_i386_[^_]+" )
        set ( NAFLAGS 
                -fwin32
                -DWIN32 )
    elseif ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "windows_amd64_[^_]+" )
        set ( NAFLAGS 
                -fwin64
                -DWIN64
                -D__x86_64__ )
    endif ( )
    list ( APPEND NAFLAGS 
                -I${${PROJECT_NAME}_LOOKASIDE_DIR}/win/
                -I${${PROJECT_NAME}_LOOKASIDE_DIR}/ )

    if ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "windows_i386_[^_]+" )
        set ( SIMD_BASENAMES 
                jsimdcpu
                jccolmmx
                jcgrammx
                jdcolmmx
                jcsammmx
                jdsammmx
                jdmermmx
                jcqntmmx
                jfmmxfst
                jfmmxint
                jimmxred
                jimmxint
                jimmxfst
                jcqnt3dn
                jf3dnflt
                ji3dnflt
                jcqntsse
                jfsseflt
                jisseflt
                jccolss2
                jcgrass2
                jdcolss2
                jcsamss2
                jdsamss2
                jdmerss2
                jcqnts2i
                jfss2fst
                jfss2int
                jiss2red
                jiss2int
                jiss2fst
                jcqnts2f
                jiss2flt )
        list ( APPEND ${PROJECT_NAME}_SRC
                ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/jsimd_i386.c )
    elseif ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "windows_amd64_[^_]+" )
        set ( SIMD_BASENAMES 
                jfsseflt-64
                jccolss2-64
                jdcolss2-64
                jcgrass2-64
                jcsamss2-64
                jdsamss2-64
                jdmerss2-64
                jcqnts2i-64
                jfss2fst-64
                jfss2int-64
                jiss2red-64
                jiss2int-64
                jiss2fst-64
                jcqnts2f-64
                jiss2flt-64 )
        list ( APPEND ${PROJECT_NAME}_SRC
                ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/jsimd_x86_64.c )
    endif ( )

    set ( OBJDIR ${CMAKE_CURRENT_BINARY_DIR} )
    file ( GLOB INC_FILES ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd *.inc )

    foreach ( file ${SIMD_BASENAMES} )
      set ( DEPFILE "" )
      set ( SIMD_SRC ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/${file}.asm )
      if ( ${file} MATCHES col )
        set ( DEPFILE ${file} )
        string ( REGEX REPLACE "col" "clr" DEPFILE ${DEPFILE} )
        set ( DEPFILE ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/${DEPFILE}.asm )
      endif ( )
      if ( ${file} MATCHES mer )
        set ( DEPFILE ${file} )
        string ( REGEX REPLACE "mer" "mrg" DEPFILE ${DEPFILE} )
        set ( DEPFILE ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/${DEPFILE}.asm )
      endif ( )
      if ( ${file} MATCHES gra )
        set ( DEPFILE ${file} )
        string ( REGEX REPLACE "gra" "gry" DEPFILE ${DEPFILE} )
        set ( DEPFILE ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd/${DEPFILE}.asm )
      endif ( )
      set ( SIMD_OBJ ${OBJDIR}/${file}.obj )
      add_custom_command ( OUTPUT ${SIMD_OBJ}
        DEPENDS ${SIMD_SRC} ${DEPFILE} ${INC_FILES}
        COMMAND ${NASM} ${NAFLAGS} ${SIMD_SRC} -o${SIMD_OBJ}
        WORKING_DIRECTORY ${${PROJECT_NAME}_LOOKASIDE_DIR}/simd )
      set ( SIMD_OBJS ${SIMD_OBJS} ${SIMD_OBJ} )
    endforeach ( )

    set_source_files_properties ( ${SIMD_OBJS} PROPERTIES GENERATED 1 )
    add_custom_target ( simd DEPENDS ${SIMD_OBJS} )

    list ( APPEND ${PROJECT_NAME}_SRC ${SIMD_OBJS} )
    list ( APPEND ${PROJECT_NAME}_INCLUDE ${${PROJECT_NAME}_LOOKASIDE_DIR}/win )
else ( )
    list ( APPEND ${PROJECT_NAME}_SRC
            ${${PROJECT_NAME}_LOOKASIDE_DIR}/jsimd_none.c )
    list ( APPEND ${PROJECT_NAME}_DEFINITIONS -DHAVE_CONFIG_H )
    list ( APPEND ${PROJECT_NAME}_OPTIONS -Wno-attributes )
endif ( )

add_library ( ${PROJECT_NAME} STATIC ${${PROJECT_NAME}_SRC} )
target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE ${${PROJECT_NAME}_INCLUDE} )
target_compile_definitions ( ${PROJECT_NAME} PRIVATE ${${PROJECT_NAME}_DEFINITIONS} )
target_compile_options ( ${PROJECT_NAME} PRIVATE ${${PROJECT_NAME}_OPTIONS} )

if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    if ( DEFINED ${PROJECT_NAME}_MSVC_ALT_CRUNTIME )
        target_compile_options ( ${PROJECT_NAME} PRIVATE
                ${${PROJECT_NAME}_MSVC_ALT_CRUNTIME} )
    endif ( )
endif ( )

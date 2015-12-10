# Copyright © 2015, Prosoft Engineering, Inc. (A.K.A "Prosoft")
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of Prosoft nor the names of its contributors may be
#       used to endorse or promote products derived from this software without
#       specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL PROSOFT ENGINEERING, INC. BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required(VERSION 3.1)
project(coretests)

set(CORETESTS ${CMAKE_CURRENT_LIST_DIR})

include("${CMAKE_CURRENT_LIST_DIR}/../core.cmake")
include("${CORETESTS}/test_utils.cmake")

set(SRCTESTS
    ${CORETESTS}/src/byteorder_tests.cpp
    ${CORETESTS}/src/case_convert_tests.cpp
    ${CORETESTS}/src/regex_string_tests.cpp
    ${CORETESTS}/src/regex_u8string_tests.cpp
    ${CORETESTS}/src/regex_u16string_tests.cpp
    ${CORETESTS}/src/string_convert_tests.cpp
    ${CORETESTS}/src/u8string_tests.cpp
)

add_executable(${PROJECT_NAME}
    ${CORETESTS}/src/test_main.cpp
    ${SRCTESTS}
)

if(APPLE)
    target_sources(${PROJECT_NAME} PRIVATE
        ${CORETESTS}/src/apple_convert_tests.mm
    )
    set_source_files_properties(
        ${CORETESTS}/src/apple_convert_tests.mm
        PROPERTIES COMPILE_FLAGS -fobjc-arc
    )
endif()

target_include_directories(${PROJECT_NAME} PRIVATE ${CORETESTS})

ps_core_configure(${PROJECT_NAME})
ps_core_use_regex(${PROJECT_NAME})
ps_core_use_u8string(${PROJECT_NAME})

enable_testing()
ps_add_ctests_from_catch_tests(${PROJECT_NAME})
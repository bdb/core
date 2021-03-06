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
project(regex)

set(PSREGEX ${CMAKE_CURRENT_LIST_DIR})

add_subdirectory(${PSREGEX}/vendor/onig)

include("${CMAKE_CURRENT_LIST_DIR}/../config_module.cmake")

set(regex_lib ${PROJECT_NAME})
add_library(${regex_lib}
    ${PSREGEX}/regex.cpp
)

ps_core_module_config(${regex_lib})

target_include_directories(${regex_lib}
	PRIVATE
    ${PSREGEX}/vendor/onig/onig
    PUBLIC
    ${PSREGEX}
)

if(WIN32)
    target_compile_definitions(${regex_lib} PRIVATE ONIG_EXTERN=extern)
endif()

target_link_libraries(${regex_lib} PRIVATE onig)

ps_core_enable_code_coverage(${PSREGEX}/regex.cpp)

############################################################################
#
#   Copyright (C) 2015 Mark Charlebois. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name PX4 nor the names of its contributors may be
#    used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

set(TOOLS_ERROR_MSG
		"The FC_ADDON must be installed and the environment variable FC_ADDON must be set"
		"(e.g. export FC_ADDON=${HOME}/Qualcomm/addon)")

if ("$ENV{FC_ADDON}" STREQUAL "")
	message(FATAL_ERROR ${TOOLS_ERROR_MSG})
else()
	set(FC_ADDON $ENV{FC_ADDON})
endif()

add_library(uart_esc SHARED IMPORTED GLOBAL)
set_target_properties(uart_esc PROPERTIES IMPORTED_LOCATION ${FC_ADDON}/flight_controller/hexagon/libs/libuart_esc.so)

px4_add_module(
	MODULE platforms__qurt__fc_addon__uart_esc
	MAIN uart_esc
	INCLUDES
		${FC_ADDON}/flight_controller/hexagon/inc
	SRCS
		uart_esc_main.cpp
	DEPENDS
	)
target_link_libraries(platforms__qurt__fc_addon__uart_esc PRIVATE uart_esc)

# -*- mode: Python; indent-tabs-mode: t; tab-width: 4; python-indent: 4 -*-
# Copyright (C) 2013  Olga Yakovleva <yakovleva.o.v@gmail.com>

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

file(GLOB_RECURSE SRCFILES1 "${CMAKE_CURRENT_SOURCE_DIR}/HTS_*.c")

if(ENABLE_MAGE)
	file(GLOB_RECURSE SRCFILES2 "${CMAKE_CURRENT_SOURCE_DIR}/HTS106_*.c")
	set(CPACK_DEBIAN_LIBHTS_ENGINE_PACKAGE_DEPENDS "${CPACK_DEBIAN_LIBMAGE_PACKAGE_NAME}" PARENT_SCOPE)
else()
	set(SRCFILES2 "")
endif()

add_library(libhts_engine SHARED "${SRCFILES1}" "${SRCFILES2}")

set(LIBHTS_VERSION_MAJOR 1)
set(LIBHTS_VERSION_MINOR 06)
set(LIBHTS_VERSION_PATCH 0)
set(LIBHTS_VERSION "${LIBHTS_VERSION_MAJOR}.${LIBHTS_VERSION_MINOR}-RHVoice")
set_target_properties(libhts_engine PROPERTIES VERSION "${LIBHTS_VERSION}" SOVERSION "${LIBHTS_VERSION_MAJOR}")

set(CPACK_COMPONENT_LIBHTS_ENGINE_VERSION_MAJOR "${LIBHTS_VERSION_MAJOR}" PARENT_SCOPE)
set(CPACK_COMPONENT_LIBHTS_ENGINE_VERSION_MINOR "${LIBHTS_VERSION_MINOR}" PARENT_SCOPE)
set(CPACK_COMPONENT_LIBHTS_ENGINE_VERSION_PATCH "${LIBHTS_VERSION_PATCH}" PARENT_SCOPE)
set(CPACK_COMPONENT_LIBHTS_ENGINE_VERSION "${LIBHTS_VERSION}" PARENT_SCOPE)

target_include_directories(libhts_engine PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}" "${INCLUDE_DIR}")
harden(libhts_engine)
add_sanitizers(libhts_engine)
set_target_properties("libhts_engine" PROPERTIES PREFIX "")

install(TARGETS "libhts_engine"
	LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	COMPONENT "libhts_engine"
)
set("CPACK_DEBIAN_LIBHTS_ENGINE_PACKAGE_NAME" "libhtsengine" PARENT_SCOPE)

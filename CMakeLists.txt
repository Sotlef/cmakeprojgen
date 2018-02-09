cmake_minimum_required (VERSION 3.0)

project (CMakeProjectGenerator)

set (CMAKEPROJGEN_PROJECTNAME CACHE STRING "Project Name")
set (CMAKEPROJGEN_AUTHORNAME CACHE STRING "Project Author")
set (CMAKEPROJGEN_CONTACTS CACHE STRING "Project Contacts")
set (CMAKEPROJGEN_LICENSE CACHE STRING "Project License")
set (CMAKEPROJGEN_ROOTDIR CACHE PATH "Root Directory")
set (CMAKEPROJGEN_OUTDIR CACHE STRING "Output Directory")

if ("x${CMAKEPROJGEN_PROJECTNAME}" STREQUAL "x")
	message (FATAL_ERROR "Enter 'Project Name'")
elseif ("x${CMAKEPROJGEN_ROOTDIR}" STREQUAL "x")
	message (FATAL_ERROR "Enter 'Project Directory'")
endif ()

string (MAKE_C_IDENTIFIER ${CMAKEPROJGEN_PROJECTNAME} TEMPPROJNAME)
if (${CMAKEPROJGEN_PROJECTNAME} STREQUAL ${TEMPPROJNAME})
	string (TOUPPER ${TEMPPROJNAME} CMAKEPROJGEN_PROJECTPREFIX)
else ()
	message (FATAL_ERROR "Invalid Project Name: ${CMAKEPROJGEN_PROJECTNAME}")
endif ()

file (MAKE_DIRECTORY ${CMAKEPROJGEN_ROOTDIR})
configure_file (
	"source/CMakeLib.cmake.in" 
	"${CMAKEPROJGEN_ROOTDIR}/cmake/CMakeLib.cmake" @ONLY)
configure_file (
	"source/CMakeLists.txt.in"
	"${CMAKEPROJGEN_ROOTDIR}/CMakeLists.txt" @ONLY)


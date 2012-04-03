cmake_minimum_required(VERSION 2.8)

project(swganh-docs CXX)

# current version
if(NOT DEFINED swganh_VERSION)
    set(swganh_VERSION_MAJOR 0)
    set(swganh_VERSION_MINOR 4)
    set(swganh_VERSION_PATCH 0)
    set(swganh_VERSION
    	${swganh_VERSION_MAJOR}.${swganh_VERSION_MINOR}.${swganh_VERSION_PATCH})
endif()

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

find_package(Sphinx REQUIRED)

if(NOT DEFINED SPHINX_THEME)
	set(SPHINX_THEME default)
endif()

if(NOT DEFINED SPHINX_THEME_DIR)
	set(SPHINX_THEME_DIR)
endif()
	
# configured documentation tools and intermediate build results
set(BINARY_BUILD_DIR "${CMAKE_CURRENT_BINARY_DIR}/_build")
	
# Sphinx cache with pickled ReST documents
set(SPHINX_CACHE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_doctrees")

# HTML output directory
set(SPHINX_HTML_DIR "${CMAKE_CURRENT_BINARY_DIR}/html")

if(NOT DEFINED SWGPY_DIR)
    set(SWGPY_DIR ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
endif()
		
if(WIN32)
    foreach(configuration ${CMAKE_CONFIGURATION_TYPES})    
        configure_file(
	        "${CMAKE_CURRENT_SOURCE_DIR}/conf.py.in"
	        "${BINARY_BUILD_DIR}/${configuration}/conf.py"
	        @ONLY)
	endforeach()
	
	set(BINARY_BUILD_DIR "${BINARY_BUILD_DIR}/$<CONFIGURATION>")
    set(SPHINX_CACHE_DIR "${SPHINX_CACHE_DIR}/$<CONFIGURATION>")
    set(SPHINX_HTML_DIR "${SPHINX_HTML_DIR}/$<CONFIGURATION>")
else()
    configure_file(
	    "${CMAKE_CURRENT_SOURCE_DIR}/conf.py.in"
	    "${BINARY_BUILD_DIR}/conf.py"
	    @ONLY) 		
endif()

file(GLOB_RECURSE DOCUMENTS *.rst *.in)

# Group the source files based on the directory structure.
FOREACH(__source_file ${DOCUMENTS})
    STRING(REGEX REPLACE "(${CMAKE_CURRENT_SOURCE_DIR}/)((.*/)*)(.*)" "\\2" __source_dir "${__source_file}")
    STRING(REGEX REPLACE "(${CMAKE_CURRENT_SOURCE_DIR}/${__source_dir})(.*)" "\\2" __source_filename "${__source_file}")

    STRING(REPLACE "/" "\\\\" __source_group "${__source_dir}")
    SOURCE_GROUP("${__source_group}" FILES ${__source_file})
ENDFOREACH()

if(WIN32)
    set(WORKING_DIR "${SWGPY_DIR}/$\(Configuration\)")
    set(DEPENDENCIES swgpy)
else()
    set(WORKING_DIR "${SWGPY_DIR}")
    set(DEPENDENCIES)
endif()

add_custom_target(swganh_doc_html ALL
    ${SPHINX_EXECUTABLE}
	-q -b html
	-c "${BINARY_BUILD_DIR}"
	-d "${SPHINX_CACHE_DIR}"
	"${CMAKE_CURRENT_SOURCE_DIR}"
	"${SPHINX_HTML_DIR}"
	COMMENT "Building HTML documentation with Sphinx"
	DEPENDS ${DEPENDENCIES}
	WORKING_DIRECTORY "${WORKING_DIR}"
	VERBATIM
	SOURCES ${DOCUMENTS}
)

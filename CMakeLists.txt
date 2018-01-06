
cmake_minimum_required(VERSION 3.9)

if (NOT TARGET rapidjson-project)

	include(ExternalProject)
	
	ExternalProject_Add(rapidjson-project
		GIT_REPOSITORY https://github.com/Tencent/rapidjson.git
		GIT_TAG v1.1.0
		CMAKE_ARGS
			# This does not work on MSVC, but may be useful on Linux.
			-DCMAKE_BUILD_TYPE=Release
			-DRAPIDJSON_BUILD_DOC=OFF
			-DRAPIDJSON_BUILD_EXAMPLES=OFF
			-DRAPIDJSON_BUILD_TESTS=OFF
		# Disable the build step.
		BUILD_COMMAND ""
		# Disable the update step.
		UPDATE_COMMAND ""
		# Disable the install step.
		INSTALL_COMMAND ""
		LOG_DOWNLOAD ON
		LOG_CONFIGURE ON
		LOG_BUILD ON
	)
	
	# Set include directories.
	ExternalProject_Get_Property(rapidjson-project source_dir)
	set(RAPIDJSON_INCLUDE_DIR ${source_dir}/include)
	
	# Add libraries.
	add_library(rapidjson INTERFACE)
	target_include_directories(rapidjson SYSTEM INTERFACE ${RAPIDJSON_INCLUDE_DIR})
	add_dependencies(rapidjson rapidjson-project)

endif()

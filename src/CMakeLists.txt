add_executable(teapot)
target_sources(teapot PUBLIC
	                    "${CMAKE_CURRENT_SOURCE_DIR}/teapot.c"
	                    "${CMAKE_CURRENT_SOURCE_DIR}/mesh_data.h")

target_link_libraries(teapot -ldraw -lgraph -lmath3d -lpacket -ldma)

if (CMAKE_BUILD_TYPE STREQUAL "Release")
	add_custom_command(TARGET teapot POST_BUILD COMMAND ${EE_STRIP} ARGS --strip-all teapot)
endif()

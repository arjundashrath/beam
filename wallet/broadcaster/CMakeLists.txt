cmake_minimum_required(VERSION 3.13)

set(TARGET_NAME broadcaster${BEAM_TARGET_SUFFIX})

add_executable(${TARGET_NAME} broadcaster.cpp)

if(LINUX)
    target_link_libraries(${TARGET_NAME}
        PRIVATE 
            -static-libstdc++ 
            -static-libgcc)
endif()

target_link_libraries(${TARGET_NAME}
    PRIVATE
        broadcast_gateway
        news_channels
        cli
        mnemonic
)

if (BEAM_SIGN_PACKAGE AND WIN32)
    add_custom_command(
        TARGET ${TARGET_NAME} POST_BUILD
        COMMAND ${SIGNTOOL_EXECUTABLE} sign ${SIGNTOOL_OPTIONS} "${CMAKE_CURRENT_BINARY_DIR}\\{TARGET_NAME}.exe"
    )
endif()

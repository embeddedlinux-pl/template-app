add_subdirectory(utils)

add_executable(template-app
    appMain.cpp
)

target_link_libraries(template-app
    PRIVATE osal::cpp platform::main utils
)

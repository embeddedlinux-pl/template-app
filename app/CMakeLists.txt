add_subdirectory(utils)

add_library(apptemplate-app EXCLUDE_FROM_ALL
    appMain.cpp
)

target_include_directories(apptemplate-app
    PRIVATE .
)

target_link_libraries(apptemplate-app
    PRIVATE osal::cpp utils
)

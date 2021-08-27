add_subdirectory(utils)

add_library(apptemplate-app EXCLUDE_FROM_ALL
    appMain.cpp
)

target_include_directories(apptemplate-app
    PRIVATE .
)

conan_get(REQUIRES cli11/2.0.0)
find_package(CLI11 REQUIRED)

target_link_libraries(apptemplate-app
    PRIVATE CLI11::CLI11 osal::cpp utils
)

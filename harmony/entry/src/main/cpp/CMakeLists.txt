project(rnapp)
cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_SKIP_BUILD_RPATH TRUE)
set(OH_MODULE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../../oh_modules")
set(RNOH_APP_DIR "${CMAKE_CURRENT_SOURCE_DIR}")
set(RNOH_CPP_DIR "${OH_MODULE_DIR}/@rnoh/react-native-openharmony/src/main/cpp")
set(RNOH_GENERATED_DIR "${CMAKE_CURRENT_SOURCE_DIR}/generated")
set(SAFE_AREA_CPP_DIR "${OH_MODULE_DIR}/@react-native-ohos/react-native-safe-area-context/src/main/cpp")
set(CMAKE_ASM_FLAGS "-Wno-error=unused-command-line-argument -Qunused-arguments")
set(CMAKE_CXX_FLAGS "-fstack-protector-strong -Wl,-z,relro,-z,now,-z,noexecstack -s -fPIE -pie")
add_compile_definitions(WITH_HITRACE_SYSTRACE)
set(WITH_HITRACE_SYSTRACE 1)

add_subdirectory("${RNOH_CPP_DIR}" ./rn)
add_subdirectory("${SAFE_AREA_CPP_DIR}" ./safe_area)

add_library(rnoh_app SHARED
    "./PackageProvider.cpp"
    "${RNOH_CPP_DIR}/RNOHAppNapiBridge.cpp"
)

target_include_directories(rnoh_app PUBLIC "${SAFE_AREA_CPP_DIR}")
target_link_libraries(rnoh_app PUBLIC rnoh rnoh_safe_area)
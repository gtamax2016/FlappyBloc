# Configure with [fxsdk build-fx] or [fxsdk build-cg], which provide the
# toolchain file and module path of the fxSDK

cmake_minimum_required(VERSION 3.15)
project(FlappyBloc)

include(GenerateG1A)
include(GenerateG3A)
include(Fxconv)
find_package(Gint 2.9 REQUIRED)

set(SOURCES
  src/main.c
  # ...
)
# Shared assets, fx-9860G-only assets and fx-CG-50-only assets
set(ASSETS
  # ...
)
set(ASSETS_fx
  assets-fx/example.png
  # ...
)
set(ASSETS_cg
  assets-cg/example.png
  # ...
)

fxconv_declare_assets(${ASSETS} ${ASSETS_fx} ${ASSETS_cg} WITH_METADATA)

add_executable(flappybloc ${SOURCES} ${ASSETS} ${ASSETS_${FXSDK_PLATFORM}})
target_compile_options(flappybloc PRIVATE -Wall -Wextra -Os)
target_link_libraries(flappybloc Gint::Gint)

if("${FXSDK_PLATFORM_LONG}" STREQUAL fx9860G)
  generate_g1a(TARGET flappybloc OUTPUT "FlappyBloc.g1a"
    NAME "FlappyBloc" ICON assets-fx/icon.png)
elseif("${FXSDK_PLATFORM_LONG}" STREQUAL fxCG50)
  generate_g3a(TARGET flappybloc OUTPUT "FlappyBloc.g3a"
    NAME "FlappyBloc" ICONS assets-cg/icon-uns.png assets-cg/icon-sel.png)
endif()

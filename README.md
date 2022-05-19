# Compile-IntelXED
Compile-IntelXED

## Manual
https://intelxed.github.io/build-manual/index.html

## X86
```
python mfile.py --host-cpu=ia32 examples install
python mfile.py --host-cpu=ia32 examples install zip
```

## X86_64
```
python mfile.py examples install
python mfile.py examples install zip
```

## Usage
```cmake
kits/xed-install-base-xxx/lib/cmake/XED/XEDConfig.cmake
```
```cmake
set(XED_LIBRARY_DIR "${CMAKE_CURRENT_LIST_DIR}/../../../lib")
set(XED_LIBRARIES "${XED_LIBRARY_DIR}/xed${CMAKE_STATIC_LIBRARY_SUFFIX}" "${XED_LIBRARY_DIR}/xed-ild${CMAKE_STATIC_LIBRARY_SUFFIX}")
set(XED_INCLUDE_DIRS "${CMAKE_CURRENT_LIST_DIR}/../../../include")

# https://stackoverflow.com/a/48397346
add_library(XED::Main STATIC IMPORTED)
set_target_properties(XED::Main PROPERTIES
    IMPORTED_LOCATION "${XED_LIBRARY_DIR}/xed${CMAKE_STATIC_LIBRARY_SUFFIX}"
)

add_library(XED::ILD STATIC IMPORTED)
set_target_properties(XED::ILD PROPERTIES
    IMPORTED_LOCATION "${XED_LIBRARY_DIR}/xed-ild${CMAKE_STATIC_LIBRARY_SUFFIX}"
)

add_library(XED::XED INTERFACE IMPORTED)
set_target_properties(XED::XED PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${XED_INCLUDE_DIRS}"
    INTERFACE_LINK_LIBRARIES "XED::Main;XED::ILD"
)
```

CMakeLists.txt
```cmake
find_package(XED REQUIRED)
target_link_libraries(MyProject PRIVATE XED::XED)
``

Add the absolute path to ``kits/xed-install-base-xxx` to your `CMAKE_PREFIX_PATH` so CMake can find this package. I built this based on https://www.youtube.com/watch?v=eC9-iRN2b04 and it should be modern CMake.
```


# MITRE Geodetic Library

`Geodetic library` (or `geolib`) is a library for performing WGS-84 calculations with high precision. We think it's very handy and use it regularly in our internal software. Enjoy!

## Build

Use `cmake` in the normal way.

```bash
mkdir build
cd build
cmake ..
make
```

You'll get two artifacts:

* the geodetic library `libgeolib.a` in /lib. 
* a test binary `testGeolib` in /bin.

This library & sample binary builds and runs on **CentOs7** and **MacOsX**.

**NOTE 1**: Several c-code source files have been intentionally ignored in the build of the library. The goal in removing unneeded code from the library was to keep callers from using code that has not been recently tested. The ignored source code may be added back into the library build as needed, but it should be tested as it is added back in. See [CMakeLists.txt](/geolib/src/main/c/CMakeLists.txt) for more information.

**NOTE 2**: The `geolib` library can be compiled to run one of three solvers: Vincenty, Karney, or NGSVincenty. The solver decision is required to occur at compile-time. Only one solver may be compiled into the library. See [CMakeLists.txt](/CMakeLists.txt) for more details.

## Automatic Builds

We've been using CI internally for this library for years. We'll try to get a public builder working soon.

## Issues

Please use standard GitHub Issue and Discussion tools for interaction with the developers.

## Tests

Yup, we have 'em. See /test.

We currently use a custom test harness for our testing. The goal is to switch over to using `gtest` at some point in the future.

## CPM Snippet

`geolib` is a library that you'll want to link against, not an application to run. A recommended way to do this with your software is through the [C++ Package Manager](https://github.com/cpm-cmake/CPM.cmake) (or CPM). Add this into your CMake profile and you should be good-to-go.

Drop this in the appropriate place for your application:
```cmake
CPMAddPackage(
        NAME geolib_library
        GITHUB_REPOSITORY mitre/geodetic_library
        VERSION 3.2.7-SNAPSHOT
        GIT_TAG main
)
```
When using this library, we strongly recommend using a tagged, versioned build of geolib whenever possible.

Then, where you do your linking commands in cmake, also add this:
```cmake
target_include_directories(my-application PUBLIC ${geolib_library_SOURCE_DIR}/include)
target_link_libraries(my-application geolib)
```

Once these are done, your application (`my-application`) will be linked against `geolib_library` and you can call it from your code. Like this:

```c++
#include "geolib/cppmanifest.h"
int main(int argc, char *argv[]) {
  std::cout << "geolib-library build version: " << geolib_idealab::cppmanifest::getVersion() << std::endl;
}
```

## Example Binary

See also the `examples` binary built by this repository in /src/example. It uses CPM to retrieve, build, and link against `geolib`. Then it makes some sample calls to show `direct()` and `inverse()` operations on the ellipsoid.

To build, perform this `cmake` operation:

```bash
mkdir build
cd build
cmake ../src/example
make
```

This will produce a binary in /src/example/bin called `examples` that links against `libgeolib.a` and calls it in the `main` routine. 

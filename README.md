
# How to build

### Linux

```
make
```

### Windows (with msys2)

1. Install mingw packages

    ```
    pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-make mingw-w64-x86_64-dlfcn
    ```

2. Install pexports

    a. Goto https://winlibs.com/

    b. Download MSVCRT runtime GCC 14.1.0 (with POSIX threads) + LLVM/Clang/LLD/LLDB 18.1.5 + MinGW-w64 11.0.1 (MSVCRT)

    c. Extract pexports to anywhere that added to `$PATH`

3. Build

    ```
    export VCToolsInstallDir="C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.38.33130"
    mingw32-make LDEXPORT="-static -s" CONFIG_WIN32=y
   #strip -g
   gcc -shared -o quickjs.dll -static -s -Wl,--whole-archive libquickjs.a -lm -Wl,--no-whole-archive
   pexports quickjs.dll > quickjs.def
   "$VCToolsInstallDir/bin/Hostx64/x64/lib" /def:quickjs.def /machine:x64 /out:quickjs.lib
    ```

### Android

Install android sdk, android ndk and cmake.

```
export ANDROID_SDK_HOME=/path/to/android_sdk_home
export ANDROID_NDK_HOME=/path/to/android_ndk_home
mkdir -p build/android
cd build/android
cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK_HOME/build/cmake/android.toolchain.cmake \
   -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-29 ../..
```
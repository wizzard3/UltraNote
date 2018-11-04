## THIS IS AN OLD CODE, DO NOT FORK OR BUILD
## Building UltraNote

On Apple or Linux and can't get readline installed? Run cmake with `cmake .. -DFORCE_READLINE=FALSE` to disable readline support.

### On Linux:

Dependencies: GCC 4.7.3 or later, CMake 2.8.6 or later, GNU Readline, and Boost 1.55 or later.

You may download them from:

- http://gcc.gnu.org/
- http://www.cmake.org/
- http://www.boost.org/
- https://tiswww.case.edu/php/chet/readline/rltop.html (Mac + Linux only)

Alternatively, it may be possible to install them using a package manager.

To build:
Run these commands:
```
cd ~
sudo apt-get install build-essential git cmake libboost-all-dev libreadline-dev
git clone https://github.com/xun-project/UltraNote.git 
cd UltraNote
mkdir build
cd build
cmake ..
cd ..
make
```

The resulting executables can be found in `UltraNote/build/release/src`.

#### Advanced options:

Parallel build: run `make -j<number of threads>` instead of `make`.

Debug build: run `make build-debug`.

Test suite: run `make test-release` to run tests in addition to building. Running `make test-debug` will do the same to the debug version.

Building with Clang: it may be possible to use Clang instead of GCC, but this may not work everywhere. To build, run `export CC=clang CXX=clang++` before running `make`.

### On Windows

##### Prerequisites
- Install [Visual Studio 2017 Community Edition](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&page=inlineinstall)
- When installing Visual Studio, it is **required** that you install **Desktop development with C++** and the **VC++ v140 toolchain** when selecting features. The option to install the v140 toolchain can be found by expanding the "Desktop development with C++" node on the right. You will need this for the project to build correctly.
- Install [Boost 1.59.0](https://sourceforge.net/projects/boost/files/boost-binaries/1.59.0/), ensuring you download the installer for MSVC 14.

##### Building

- From the start menu, open 'x64 Native Tools Command Prompt for vs2017'.
- `cd <your_ultranote_directory>`
- `mkdir build`
- `cd build`
- Set the PATH variable for cmake: ie. `set PATH="C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin";%PATH%`
- `cmake -G "Visual Studio 15 2017 Win64" .. -DBOOST_ROOT=C:/local/boost_1_59_0` (Or your boost installed dir.)
- `MSBuild UltraNote.sln /p:Configuration=Release /m`
- If all went well, it will complete successfully, and you will find all your binaries in the '..\build\src\Release' directory.
- Additionally, a `.sln` file will have been created in the `build` directory. If you wish to open the project in Visual Studio with this, you can.

### On Apple:

```
brew install git cmake boost rocksdb readline
brew link --force readline
git clone https://github.com/xun-project/UltraNote.git
cd UltraNote
mkdir build
cd build
cmake ..
make
```

The resulting executables can be found in `UltraNote/build/release/src`.

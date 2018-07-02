## Building UltraNote

### On *nix:

On Apple or Linux and can't get readline installed? Run cmake with `cmake .. -DFORCE_READLINE=FALSE` to disable readline support.

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

### On Windows:
Dependencies: MSVC 2013 or later, CMake 2.8.6 or later, and Boost 1.55 or later. You may download them from:

- http://www.microsoft.com/
- http://www.cmake.org/
- http://www.boost.org/

To build, change to a directory where this file is located, and run this commands:
```
mkdir build
cd build
cmake -G "Visual Studio 12 Win64" ..
```

And then do Build.

Good luck!

### On Apple:

`brew install git cmake boost rocksdb readline`
`brew link --force readline`
`git clone https://github.com/xun-project/UltraNote.git`
`cd UltraNote`
`mkdir build`
`cd build`
`cmake ..`
`make`

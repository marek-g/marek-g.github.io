---
layout: page
title: Rust Crosscompiling
parent: Tips & Tricks
---

# Rust Crosscompiling on Linux

Tested on `Manjaro Linux 5.6.12` (2020-05-26).

## Rust

Install native Linux `rust` and `i686-pc-windows-gnu` target.

Currently (as of 2020-05) Windows version of `cargo` doesn't run on Wine (NtLockFile from `ntdll` is not implemented). `Rustc` alone seems to be working. Fortunately, you can use linux version of `rustc` and `cargo` with `i686-pc-windows-gnu` target installed.

## GCC (option 1 - Linux binaries)

You can install `mingw-w64-gcc` natively on Linux. Make sure that the `i686` version is configured with `dwarf2` exception handling. Otherwise, it won't link the rust code. Also install `i686-w64-mingw32-wine` package.

Look at the `PKGBUILD` (you need to also verify it for every library). Make sure there are flags:

```sh
--disable-sjlj-exceptions --with-dwarf2
```

However, the `sjlj` version may also work (not tested) if you add `rustflags` to the `.cargo/config`:

```toml
[target.i686-pc-windows-gnu]
rustflags = "-C panic=abort"
```

Add to your `.cargo/config`:

```toml
[target.i686-pc-windows-gnu]
linker = "/usr/bin/i686-w64-mingw32-gcc"
ar = "/usr/i686-w64-mingw32/bin/ar"
runner = "/usr/bin/i686-w64-mingw32-wine"
rustflags = [
            ]

[target.x86_64-pc-windows-gnu]
linker = "/usr/bin/x86_64-w64-mingw32-gcc"
ar = "/usr/x86_64-w64-mingw32/bin/ar"
runner = "/usr/bin/x86_64-w64-mingw32-wine"
rustflags = [
            ]
```

## GCC (option 2 - Windows binaries)

It works through Wine.

You can either download gcc from: <https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/>. Tested with `i686-8.1.0-release-posix-dwarf-rt_v6-rev0.7z`.

Or you can install msys2-i686 from: <https://sourceforge.net/projects/msys2/files/Base/i686/>. The only problem with it is that `bash` and `pacman` commands are crashing on `Wine`, but the gcc compiler works great. And it is very easy to install libraries. So this is a great option if you can update and install libraries from real Windows.

Let's assume you've installed `msys2-i686` somewhere in `msys2-x32` folder. Now you can create following scripts at the same level:

- `wine-mingw-command.sh` - base script to call GCC tools:

```sh
#!/bin/bash

# folders
BASE_DIR=$(dirname $0)
MSYS_DIR=$BASE_DIR/msys2-x32
MINGW_DIR=$MSYS_DIR/mingw32

# GCC
export WINEPATH=$MINGW_DIR/bin
export CC=i686-w64-mingw32-gcc
export CXX=i686-w64-mingw32-g++
export AR=i686-w64-mingw32-ar

# make sure that crt2.o come from MINGW_DIR
# cargo is trying to use it's own version which won't work
declare -a new_args=()
for var in "${@:2}"
do
	if [[ "$var" == */lib/crt2.o ]]
	then
		new_args+=("$MINGW_DIR/i686-w64-mingw32/lib/crt2.o")
	elif [[ "$var" == */lib/dllcrt2.o ]]
	then
		new_args+=("$MINGW_DIR/i686-w64-mingw32/lib/dllcrt2.o")
	else
		new_args+=($var)
	fi
done

# call command provided as the first parameter
$MINGW_DIR/bin/$1 "${new_args[@]}"
```

- `wine-runner.sh` - allows to run final .exe files with access to all required dlls:

```sh
#!/bin/bash

# folders
BASE_DIR=$(dirname $0)
MSYS_DIR=$BASE_DIR/msys2-x32
MINGW_DIR=$MSYS_DIR/mingw32

# GCC
export WINEPATH=$MINGW_DIR/bin

/usr/bin/wine "$@"
```

Now you are ready to create following shortcuts. Put them somewhere that they are available in one of the PATH environment's folder. For example in `~/.cargo/bin". The scripts are:

- `i686-w64-mingw32-ar`:

```sh
/mnt/aplikacje/Uzytkowe/dev/compilers/native-x32/wine-mingw-command.sh ar.exe "$@"
```

- `i686-w64-mingw32-g++`:

```sh
/mnt/aplikacje/Uzytkowe/dev/compilers/native-x32/wine-mingw-command.sh g++.exe "$@"
```

- `i686-w64-mingw32-gcc`:

```sh
/mnt/aplikacje/Uzytkowe/dev/compilers/native-x32/wine-mingw-command.sh gcc.exe "$@"
```

- `i686-w64-mingw32-pkg-config`:

```sh
/mnt/aplikacje/Uzytkowe/dev/compilers/native-x32/wine-mingw-command.sh pkg-config.exe "$@"
```

Note! I had problems with building freetype for rust when I rename `i686-w64-mingw32-ar` to `i686-w64-mingw32-gcc-ar`. The reason is not clear to me and it is related with `cmake` that cannot find `ar` command. 

Finally, you can update `cargo` configuration in `~/.cargo/config` file:


```toml
[target.i686-pc-windows-gnu]
linker = "i686-w64-mingw32-gcc"
ar = "i686-w64-mingw32-ar"
runner = "/mnt/aplikacje/Uzytkowe/dev/compilers/native-x32/wine-runner.sh"
rustflags = [
            ]
```

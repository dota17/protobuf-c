[![Build Status](https://travis-ci.org/protobuf-c/protobuf-c.png?branch=master)](https://travis-ci.org/protobuf-c/protobuf-c) [![Coverage Status](https://coveralls.io/repos/protobuf-c/protobuf-c/badge.png)](https://coveralls.io/r/protobuf-c/protobuf-c)

## Overview

This is `protobuf-c`, a C implementation of the [Google Protocol Buffers](https://developers.google.com/protocol-buffers/) data serialization format. It includes `libprotobuf-c`, a pure C library that implements protobuf encoding and decoding, and `protoc-c`, a code generator that converts Protocol Buffer `.proto` files to C descriptor code, based on the original `protoc`. `protobuf-c` formerly included an RPC implementation; that code has been split out into the [protobuf-c-rpc](https://github.com/protobuf-c/protobuf-c-rpc) project.

`protobuf-c` was originally written by Dave Benson and maintained by him through version 0.15 but is now being maintained by a new team. Thanks, Dave!

## Mailing list

`protobuf-c`'s mailing list is hosted on a [Google Groups forum](https://groups.google.com/forum/#!forum/protobuf-c). Subscribe by sending an email to [protobuf-c+subscribe@googlegroups.com](mailto:protobuf-c+subscribe@googlegroups.com).

## Building

`protobuf-c` requires a C compiler, a C++ compiler, [protobuf](https://github.com/google/protobuf), and `pkg-config` to be installed.

    ./configure && make && make install

If building from a git checkout, the `autotools` (`autoconf`, `automake`, `libtool`) must also be installed, and the build system must be generated by running the `autogen.sh` script.

    ./autogen.sh && ./configure && make && make install

## Test

If you want to execute test cases individually, please run the following command after running `./configure` once.

     make check
	 
## Documentation

See the [online Doxygen documentation here](http://lib.protobuf-c.io) or [the Wiki](https://github.com/protobuf-c/protobuf-c/wiki) for a detailed reference. The Doxygen documentation can be built from the source tree by running:

    make html

## Synopsis

Use the `protoc` command to generate `.pb-c.c` and `.pb-c.h` output files from your `.proto` input file. The `--c_out` options instructs `protoc` to use the protobuf-c plugin.

    protoc --c_out=. example.proto

Include the `.pb-c.h` file from your C source code.

    #include "example.pb-c.h"

Compile your C source code together with the `.pb-c.c` file. Add the output of the following command to your compile flags.

    pkg-config --cflags 'libprotobuf-c >= 1.0.0'

Link against the `libprotobuf-c` support library. Add the output of the following command to your link flags.

    pkg-config --libs 'libprotobuf-c >= 1.0.0'

If using autotools, the `PKG_CHECK_MODULES` macro can be used to detect the presence of `libprotobuf-c`. Add the following line to your `configure.ac` file:

    PKG_CHECK_MODULES([PROTOBUF_C], [libprotobuf-c >= 1.0.0])

This will place compiler flags in the `PROTOBUF_C_CFLAGS` variable and linker flags in the `PROTOBUF_C_LDFLAGS` variable. Read [more information here](https://autotools.io/pkgconfig/pkg_check_modules.html) about the `PKG_CHECK_MODULES` macro.

## Versioning

`protobuf-c` follows the [Semantic Versioning Specification](http://semver.org/) as of version 1.0.0.

Note that as of version of 1.0.0, the header files generated by the `protoc-c` compiler contain version guards to prevent incompatibilities due to version skew between the `.pb-c.h` files generated by `protoc-c` and the public `protobuf-c.h` include file supplied by the `libprotobuf-c` support library. While we will try not to make changes to `protobuf-c` that will require triggering the version guard often, such as releasing a new major version of `protobuf-c`, this cannot be guaranteed. Thus, it's a good idea to recompile your `.pb-c.c` and `.pb-c.h` files from their source `.proto` files with `protoc-c` as part of your build system, with proper source file dependency tracking, rather than shipping potentially stale `.pb-c.c` and `.pb-c.h` files that may not be compatible with the `libprotobuf-c` headers installed on the system in project artifacts like repositories and release tarballs. (Note that the output of the `protoc-c` code generator is not standalone, as the output of some other tools that generate C code is, such as `flex` and `bison`.)

Major API/ABI changes may occur between major version releases, by definition. It is not recommended to export the symbols in the code generated by `protoc-c` in a stable library interface, as this will embed the `protobuf-c` ABI into your library's ABI. Nor is it recommended to install generated `.pb-c.h` files into a public header file include path as part of a library API, as this will tie clients of your library's API to particular versions of `libprotobuf-c`.

## Contributing

Please send patches to the [protobuf-c mailing list](https://groups.google.com/forum/#!forum/protobuf-c) or by opening a GitHub pull request.

The most recently released `protobuf-c` version is kept on the `master` branch, while the `next` branch is used for commits targeted at the next release. Please base patches and pull requests against the `next` branch, not the `master` branch.

Copyright to all contributions are retained by the original author, but must be licensed under the terms of the [BSD-2-Clause](http://opensource.org/licenses/BSD-2-Clause) license. Please add a `Signed-off-by` header to your commit message (`git commit -s`) to indicate that you are licensing your contribution under these terms.

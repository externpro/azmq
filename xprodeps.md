# azmq dependencies

|project|license [^_l]|description [dependencies]|version|source|diff [^_d]|
|-------|-------------|--------------------------|-------|------|----------|
|<a id='azmq' />[azmq](https://zeromq.org/)|[BSL-1.0](https://github.com/zeromq/azmq/blob/master/LICENSE-BOOST_1_0 'Boost Software License 1.0')|provides Boost Asio style bindings for ZeroMQ [deps: _libzmq, boost_]| |[upstream](https://github.com/zeromq/azmq 'github.com/zeromq/azmq')|  [patch]|
|<a id='libzmq' />[libzmq](https://zeromq.org/)|[MPL-2.0](http://wiki.zeromq.org/area:licensing 'Mozilla Public License 2.0')|high-performance asynchronous messaging library [deps: _libsodium_]|[xpv4.3.5.2](https://github.com/externpro/libzmq/releases/tag/xpv4.3.5.2 'release')|[repo](https://github.com/externpro/libzmq 'github.com/externpro/libzmq') [upstream](https://github.com/zeromq/libzmq 'github.com/zeromq/libzmq')|[diff](https://github.com/externpro/libzmq/compare/v4.3.5...xpv4.3.5.2 'github.com/externpro/libzmq/compare/v4.3.5...xpv4.3.5.2') [patch]|
|<a id='boost' />[boost](http://www.boost.org/ 'Boost website')|[BSL-1.0](http://www.boost.org/users/license.html 'Boost Software License')|libraries that give C++ a boost [deps: _bzip2, zlib_]|[xpv1.91.0.1](https://github.com/externpro/boost/releases/tag/xpv1.91.0.1 'release')|[repo](https://github.com/externpro/boost 'github.com/externpro/boost') [upstream](https://github.com/boostorg/boost 'github.com/boostorg/boost')|[diff](https://github.com/externpro/boost/compare/boost-1.91.0...xpv1.91.0.1 'github.com/externpro/boost/compare/boost-1.91.0...xpv1.91.0.1') [native]|
|<a id='libsodium' />[libsodium](https://doc.libsodium.org/)|[ISC](https://doc.libsodium.org/#license 'Internet Systems Consortium License, functionally equivalent to simplified BSD and MIT licenses')|library for encryption, decryption, signatures, password hashing and more [deps: _Threads_]|[xpv1.0.22.2](https://github.com/externpro/libsodium/releases/tag/xpv1.0.22.2 'release')|[repo](https://github.com/externpro/libsodium 'github.com/externpro/libsodium') [upstream](https://github.com/jedisct1/libsodium 'github.com/jedisct1/libsodium')|[diff](https://github.com/externpro/libsodium/compare/1.0.22...xpv1.0.22.2 'github.com/externpro/libsodium/compare/1.0.22...xpv1.0.22.2') [auto]|
|<a id='bzip2' />[bzip2](https://sourceware.org/bzip2/)|[bzip2-1.0.6](https://spdx.org/licenses/bzip2-1.0.6.html 'BSD-like, modified zlib license')|lossless block-sorting data compression library|[xpv1.0.8.5](https://github.com/externpro/bzip2/releases/tag/xpv1.0.8.5 'release')|[repo](https://github.com/externpro/bzip2 'github.com/externpro/bzip2') [upstream](https://github.com/opencor/bzip2 'github.com/opencor/bzip2')|[diff](https://github.com/externpro/bzip2/compare/bzip2-1.0.8...xpv1.0.8.5 'github.com/externpro/bzip2/compare/bzip2-1.0.8...xpv1.0.8.5') [intro]|
|<a id='zlib' />[zlib](https://zlib.net/ 'zlib website')|[Zlib](https://zlib.net/zlib_license.html 'zlib/libpng license, see https://en.wikipedia.org/wiki/Zlib_License')|a general-purpose lossless data-compression library|[xpv1.3.2.1](https://github.com/externpro/zlib/releases/tag/xpv1.3.2.1 'release')|[repo](https://github.com/externpro/zlib 'github.com/externpro/zlib') [upstream](https://github.com/madler/zlib 'github.com/madler/zlib')|[diff](https://github.com/externpro/zlib/compare/v1.3.2...xpv1.3.2.1 'github.com/externpro/zlib/compare/v1.3.2...xpv1.3.2.1') [patch]|
|<a id='Threads' />[Threads](https://cmake.org/cmake/help/latest/module/FindThreads.html)|[LGPL-2.1-or-later](https://spdx.org/licenses/LGPL-2.1-or-later.html 'GNU Lesser General Public License v2.1 or later')|Finds and determines the thread library of the system for multithreading support|[xpv1.0.4](https://github.com/externpro/Threads/releases/tag/xpv1.0.4 'release')|[repo](https://github.com/externpro/Threads 'github.com/externpro/Threads')|[diff](https://github.com/externpro/Threads/compare/v0...xpv1.0.4 'github.com/externpro/Threads/compare/v0...xpv1.0.4') [bin]|

![deps](xprodeps.svg 'dependencies')

Dependency version check: all 6 parent-manifest versions match pinned versions.

|diff  |description|
|------|-----------|
|patch |diff modifies/patches existing cmake|
|intro |diff introduces cmake|
|auto  |diff adds cmake to replace autotools/configure/make|
|native|diff adds cmake but uses existing build system|
|bin   |diff adds cmake to repackage binaries built elsewhere|
|fetch |diff adds cmake and utilizes FetchContent|

[^_l]: see [SPDX License List](https://spdx.org/licenses/ '') for a list of commonly found licenses
[^_d]: see table above with description of diff

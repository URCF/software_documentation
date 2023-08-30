
**Boost** provides free peer-reviewed portable C++ libraries.[1]

Installed Versions
------------------

There are several installed versions on Proteus. Use **module list
boost** to see a list.

Boost Build Documentation
-------------------------

See [Getting Started on Unix Variants](http://www.boost.org/doc/libs/1_57_0/more/getting_started/unix-variants.html)
in the Boost documentation.

b2
--

Build **b2** first, following documentation.

Build
-----

Intermediate compilation products are put in the build directory, which
should be separate from the source directory. The following builds using
the Intel compilers, by specifying the "toolset".

`    ./b2 -j 4 --prefix=$PREFIX --without-mpi --build-dir=../boost-build address-model=64 toolset=intel variant=release threading=multi cxxflags="-xHost" install > & Build.install.out`

It will take a couple of hours.

N.B. the toolset string changes with version of Boost. It may be
"intel-linux".

### boost.python

-   See
    <http://www.boost.org/doc/libs/1_57_0/libs/python/doc/building.html>

See Also
--------

-   [Intel® Developer Zone: Building Boost with Intel® C++ Compiler 15.0](https://software.intel.com/en-us/articles/building-boost-with-intel-c-compiler-150)

References
----------

<references/>

[1] [Boost Official Web Site](http://www.boost.org/)

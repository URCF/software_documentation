
**AmberTools** is a suite of software that is useful for molecular
dynamics.[1]

This guide covers building **AmberTools 18**.

Overview
--------

AmberTools uses the miniconda package manager[2] to handle its various
dependencies. It does this by installing a private version of Python
2.7.13.

1.  The zeroth step is to build Boost, and optionally, Parallel NetCDF
2.  The first step is to configure, which will also install miniconda
    and all the other packages AmberTools needs
3.  Then, you build AmberTools itself (and install, in one go)

Except, you have to interrupt the configure step after it has gotten far
enough, i.e. after all the Python/miniconda stuff is done. Go and build
Parallel NetCDF, install it, and then start the configure again.

Download
--------

You should have a copy of the tarball:

`   AmberTools18.tar.bz2`

which will expand into a directory named "`amber18`".

Environment
-----------

We will use the Intel compilers, with the OpenMPI that was built with
the Intel compilers

`    module load intel/composerxe/2015.1.133`
`    module load proteus-openmpi/intel/2015/1.8.1-mlnx-ofed`

AmberTools configure script looks for the wrong environment variable to
determine location of MKL. Set this to fix that:

`    export MKL_HOME=$MKLROOT`

Boost Libraries
---------------

AmberTools18, or one of its dependencies, needs the Boost C++
Libraries,[3] but the Amber documentation makes not mention of Boost.

The version of Boost installed on the system by default is not new
enough. So, will need to get an appropriate version. Without guidance
from the Amber manual, we will have to guess at the version required.

Parallel NetCDF (Optional)
--------------------------

For parallel i/o for one of the AmberTools modules, parallel NetCDF[4]
is needed.

`    ./configure --prefix=$AMBERHOME --enable-largefile --enable-shared --enable-fortran --enable-cxx`

Configure and Build
-------------------

Nothing in configure script allows for different boost. Need to set this
in environment:

`   export CXXFLAGS="-I$AMBERHOME/include -L$AMBERHOME/lib -Wl,-rpath,$AMBERHOME/lib"`

**NOTE** DO NOT set the above before building Parallel NetCDF. Do it
after the PNetCDF build.

**NOTE** hybrid MPI-OpenMP does not seem to be buildable

`    cd amber18`
`    ./configure -mpi --with-pnetcdf /mnt/HA/opt/amber18 intel`

You will be prompted for a location for the installation, and some other
questions.

Once that completes, follow the instructions.

`    source amber.sh`
`    make -j 12 >& Make.out &`
`    make install`

References
----------

<references/>

[1] [AmberTools website](http://ambermd.org/AmberTools.php)

[2] [miniconda website](https://conda.io/miniconda.html)

[3] [Boost C++ Libraries website](https://www.boost.org/)

[4] [Parallel NetCDF website](https://parallel-netcdf.github.io)

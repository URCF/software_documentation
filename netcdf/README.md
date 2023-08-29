---
title: NetCDF
permalink: /NetCDF/
---

**NetCDF** (Network Common Data Form) is a set of software libraries and
self-describing, machine-independent data formats that support the
creation, access, and sharing of array-oriented scientific data.[1]

Installed Versions
------------------

All nodes on Proteus have a runtime-only copy of NetCDF 4.1.1 -- this
does not allow for compiling code which uses NetCDF.

### Intel-optimized Versions

There are two versions installed:

-   NetCDF 4.4.1, with a separate NetCDF Fortran 4.4.4 module
-   NetCDF 4.5.0 + NetCDF Fortran 4.4.4

An Intel-optimized version **NetCDF 4.4.1** is installed, using Intel
Composer XE 2015.1.133, which also includes **[HDF5](/HDF5 "wikilink")
1.8.14**. This does not include HDF4 compatibility, nor does it include
any parallel i/o capability.

To use the modules listed in this section, the modulefile for the Intel
compiler must be loaded first:

`    intel/composerxe/2015.1.133`

Use the modulefile

`    proteus-netcdf/intel/2015/4.4.1`

This relies on [HDF5](/HDF5 "wikilink"), [Zlib](/Zlib "wikilink"), and
[Szip](/Szip "wikilink"). These other modules are automatically loaded
with the **proteus-netcdf** module.

For the Fortran bindings, use:

`    proteus-netcdf-fortran/intel/2015/4.4.4`

this will load other necessary modulefiles.

An Intel-optimized **NetCDF 4.5.0** is installed, which pulls in the
**[HDF5](/HDF5 "wikilink") 1.10.1** module. This NetCDF 4.5.0
installation includes NetCDF Fortran 4.4.4, so the separate NetCDF
Fortan modulefile is not needed.

Use:

`    proteus-netcdf/intel/2015/4.5.0`

Using
-----

Since there are also default system versions of NetCDF, one has to
perform some extra steps in order to be sure the appropriate library is
found at compilation/link time as well as run time.

The installation root is given by these two environment variables (NCDIR
is the one referenced in NetCDF's official documentation):

`    NCDIR`
`    NETCDFDIR`

For convenience, the include and library directories have their own
environment variables defined.

To get the appropriate include files for a C application, add the
following to the CPPFLAGS of the application you are building

`    -I$NETCDFINCDIR`

To set the proper library link path, add the following to the LDFLAGS of
the application you are building

`    -L$NETCDFLIBDIR -Wl,-rpath,$NETCDFLIBDIR`

Documentation
-------------

Official documentation is available online.[2]

Compilation Instructions
------------------------

See [Compiling NetCDF](/Compiling_NetCDF "wikilink")

References
----------

<references/>

[1] [NetCDF official website](http://www.unidata.ucar.edu/software/netcdf/)

[2] [NetCDF official documentation (latest version)](http://www.unidata.ucar.edu/software/netcdf/docs/index.html)
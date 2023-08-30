**AutoDock Vina** is an open-source program for doing molecular
docking.[1]

Installed Versions
------------------

**AutoDock Vina 1.2.3** is installed on Picotte. Use the modulefile:

`   vina/intel/2020/1.2.3`

N.B. this is distinct from [VinaLC](/VinaLC "wikilink")

Documentation
-------------

Please consult the official documentation.[2]

Python bindings
---------------

The official documentation shows how to use Anaconda[3] to have Python
bindings.

Generally, Anaconda (or Mini Conda) is not well-suited for multi-user
systems like Picotte. We recommend, instead, using Python virtual
environments (venv) which provide similar capabilities to Conda
environments. For an example, see the article [Installing TensorFlow 2.11.0 using pip and venv](/Installing_TensorFlow_2.11.0_using_pip_and_venv "wikilink").

See Also
--------

-   [Compiling AutoDock Vina](/Compiling_AutoDock_Vina "wikilink")

References
----------

<references/>

[1] [AutoDock Vina website](https://vina.scripps.edu/)

[2] [AutoDock Vina Documentation](https://autodock-vina.readthedocs.io/en/latest/index.html)

[3] [AutoDock Vina Documentation - Installation - Python bindings](https://autodock-vina.readthedocs.io/en/latest/installation.html#python-bindings)


**[AutoDock Vina](/AutoDock_Vina "wikilink")** is an open-source program
for doing molecular docking.[1]

Documentation
-------------

Official documentation:
<https://autodock-vina.readthedocs.io/en/latest/introduction.html>

Source Code
-----------

GitHub repository: <https://github.com/ccsb-scripps/AutoDock-Vina>

Compilation
-----------

See [official build instructions.](https://autodock-vina.readthedocs.io/en/latest/installation.html#building-from-source)

We will use:

-   Intel compilers via Intel Composer XE
-   Boost 1.75.0
-   Python 3.7.7 provided by Intel Composer XE (optional)

Load the appropriate modulefiles:

-   intel/composerxe/2020u4
-   boost/intel/2020/1.75.0

Get source from:
<https://github.com/ccsb-scripps/AutoDock-Vina/releases>

Expand archive:

``` text
[juser@picotte001 ~]$ cd /ifs/groups/myrsrchGrp/src
[juser@picotte001 ~]$ tar -xf v1.2.3.tar.gz
```

which creates the directory `AutoDock-Vina-1.2.3`

### Install Location

We will install the executables to a subdirectory of your group
directory. First, create the directory structure:

``` text
[juser@picotte001 ~]$ cd /ifs/groups/myrsrchGrp
[juser@picotte001 ~]$ mkdir -p software/bin
```

### Modify Makefile

We will modify the Makefile to use the Intel Compiler, and set
optimization options. We also set installation location.

``` text
[juser@picotte001 ~]$ cd AutoDock-Vina-1.2.3/build/linux/release
[juser@picotte001 release]$
```

Edit the `Makefile` to look like this:

``` makefile
BASE=/ifs/groups/myrsrchGrp/software/
BOOST_VERSION=
BOOST_INCLUDE = $(BASE)/include
C_PLATFORM=-pthread
GPP=icpc
C_OPTIONS= -O3 -DNDEBUG -std=c++11 -xHost
BOOST_LIB_VERSION=

include ../../makefile_common
```

### Compile

We compile with 12 concurrent processes, and redirect output to a file
so we can inspect it later.

``` text
[juser@picotte001 release]$ make -j 12 | tee Make.out 2>&1
```

This generates a bunch of `.o` files and the two executables `vina` and
`vina_split`.

There is no installer, so copy the executables to an appropriate
location.

``` text
[juser@picotte001 release]$ cp vina vina_split /ifs/groups/myrsrchGrp/software/bin
```

### Using Vina

In order to use Vina, you will need to add
`/ifs/groups/myrsrchGrp/software/bin` to your `PATH` environment
variable.

References
----------

<references/>

[1] [AutoDock Vina website](https://vina.scripps.edu/)

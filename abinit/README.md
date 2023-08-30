Sources
-------

Available from:
<http://www.abinit.org/downloads/source-packages/abinit-1>

Using GCC + MVAPICH
-------------------

### Environment

Currently Loaded Modulefiles:

` 1) shared                                  4) gcc/4.8.1`
` 2) proteus                                 5) proteus-mvapich2/gcc/64/1.9-mlnx-ofed`
` 3) sge/univa                               6) proteus-fftw3/intel/gcc/64/3.3.3`

NB This uses FFTW3 optimized for Intel CPUs.

Set the following environment variables:

`    CPP="gcc -E"`

### Modify `configure` script

In line `9312` add a line containing "`unset MPI_RUNNER`" so that those
few lines look like:

``` bash
      $as_echo "$as_me: WARNING: ${CPP} might not be fully compatible with MPI" >&2;}
      fi
      unset MPI_RUNNER
      if test "${MPI_RUNNER}" != ""; then
        as_fn_error $? "use --with-mpi-prefix or set MPI_RUNNER, not both" "$LINENO" 5
      fi
```

### Configure

`    [juser@proteusi01 abinit-7.6.4]$ ./configure --prefix=$MYGROUPDIR --with-mpi-prefix=$MPI_HOME \`
`        --with-fft-flavor=fftw3 \`
`        --with-fft-incs=-I$FFTW3INCLUDE \`
`        --with-fft-libs="-L$FFTW3DIR -lfftw3 -lfftw3f"`

### Edit `config.status`

-   Line 901:

`     S["fcflags_opt_41_xc_lowlevel"]="-O1 -extend-source"`

Then,

`     [juser@proteusi01 abinit-7.6.4]$ ./config.status`

### Build

`    [juser@proteusi01 abinit-7.6.4]$ make -j 8 >& Make.out &`

### Test

`    [juser@proteusi01 abinit-7.6.4]$ cd tests ; ./runtests.py`
`    ...`
`    Suite        failed  passed  succeeded  skipped  disabled  run_etime  tot_etime`
`    atompaw           0       1          1        0         0      17.22      17.41`
`    bigdft            1       1         21        0         0     234.04     235.21`
`    built-in          0       0          7        0         0       5.91       5.97`
`    etsf_io           0       0          7        0         0      16.92      17.30`
`    fast              0       1         10        0         0      30.54      32.00`
`    fox               0       1          1        0         0      32.29      32.43`
`    gpu               0       0          0        4         0       0.00       0.00`
`    libxc             0       8         13        0         0     202.80     206.34`
`    mpiio             0       0          1       13         0       1.28       1.35`
`    paral             0       6         15       64         0     327.47     331.41`
`    seq               0       0          0       18         0       0.00       0.01`
`    tutoparal         0       0          1        1         0       0.62       0.64`
`    tutoplugs         0       4          0        0         0      18.33      18.62`
`    tutorespfn        0      10         12        0         0     815.04     822.84`
`    tutorial          0       8         40        0         0     735.31     747.79`
`    unitary           0       0         17       13         0      41.01      41.35`
`    v1                0       4         72        0         0     170.35     177.06`
`    v2                0      11         69        0         0     227.11     236.53`
`    v3                0      14         68        0         0     329.46     344.20`
`    v4                0      17         46        0         0     369.51     381.65`
`    v5                0      17         57        0         0     825.48     846.29`
`    v6                0      11         49        0         0     563.24     579.66`
`    v67mbpt           0       4         13        0         0     273.72     276.91`
`    v7                0       7         14        0         0     472.23     477.66`
`    vdwxc             0       0          1        0         0      11.77      11.84`
`    wannier90         0       6          0        0         0      35.63      36.17`

### Install

To run abinit, you must complete the installation. It will probably not
work right trying to run from within the source hierarchy:

`    [juser@proteusi01 abinit-7.6.4]$ make install`

### Job Script

This is a bare outline of a job script to run abinit, which has been
installed with "prefix=$HOME":

``` bash
     . /etc/profile
     module load shared
     module load gcc sge/univa
     module load proteus
     module load gcc/4.8.1
     module load proteus-mvapich2/gcc/64/1.9-mlnx-ofed
     module load proteus-fftw3/intel/gcc/64/3.3.3
     module list

     export ABINIT=$HOME/bin/abinit

     mpirun $ABINIT < t.files > abinit.log 2>&1
```

Intel Compiler 2013 + MVAPICH2
------------------------------

**NOTE** Use FFTW from MKL.

### Environment

`    Currently Loaded Modulefiles:`
`      1) shared                                    6) intel/ipp/64/8.1/2013_sp1.3.174`
`      2) proteus                                   7) intel/mkl/64/11.1/2013_sp1.3.174`
`      3) gcc/4.8.1                                 8) intel/tbb/64/4.2/2013_sp1.3.174`
`      4) sge/univa                                 9) intel-tbb-oss/intel64/42_20140601oss`
`      5) intel/compiler/64/14.0/2013_sp1.3.174    10) proteus-mvapich2/intel/64/1.9-mlnx-ofed`

Environment variables:

`    CC = mpicc`
`    CXX = mpicxx`
`    FC = mpif90`

### Configure

`    $ ./configure --prefix=/mnt/HA/opt/abinit/intel/mvapich2/7.10.2 --enable-mpi --enable-optim=aggressive \`
`          --with-fft-flavor=fftw3-mkl --with-linalg-flavor=mkl`

Intel Compiler 2015 + OpenMPI
-----------------------------

IN PROGRESS

### Configure

"-ipo" may or may not work

`    ./configure CC=mpicc CXX=mpicxx FC=mpif90 FCFLAGS="-O2 -xHost -mkl" \`
`        CFLAGS="-O2 -xHost -mkl" CXXFLAGS="-O2 -xHost -mkl" \`
`        --prefix=/mnt/HA/groups/mygroup/opt \`
`        --with-fc-vendor=intel \`
`        --with-fft-flavor=fftw3-mkl \`
`        --with-fft-libs="-lmkl_intel_lp64 -lmkl_sequential -lmkl_core" \`
`        --with-linalg-flavor=mkl \`
`        --enable-mpi --enable-mpi-inplace --enable-mpi-io`

Using Intel Compiler + Intel MPI
--------------------------------

**INCOMPLETE**

### Environment

`    Currently Loaded Modulefiles:`
`      1) shared                                  4) intel/compiler/64/14.0/2013_sp1.1.106`
`      2) proteus                                 5) intel-mpi/64/4.1.1/036`
`      3) sge/univa                               6) intel/mkl/64/11.1/2013_sp1.1.106`

### Modify `configure` script

In line `9312` add a line containing "`unset MPI_RUNNER`" so that those
few lines look like:

``` make
      $as_echo "$as_me: WARNING: ${CPP} might not be fully compatible with MPI" >&2;}
      fi
      unset MPI_RUNNER
      if test "${MPI_RUNNER}" != ""; then
        as_fn_error $? "use --with-mpi-prefix or set MPI_RUNNER, not both" "$LINENO" 5
      fi
```

### Configure

`    [juser@proteusi01 abinit-7.6.4]$ ./configure --prefix=$HOME --with-mpi-prefix=$I_MPI_ROOT/intel64`

### Make

`    [juser@proteusi01 abinit-7.6.4]$ make -j 8 >& Make.out &`

Caution
-------

### Optimization Flags

The default build will use these optimization settings:
`-mtune=native -march=native` This means that the generated executable
may not run properly on both Intel and AMD. The "native" setting means
to set all possible optimization flags based on the machine being used
to compile.


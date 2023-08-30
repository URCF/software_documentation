
**BLASR** is [Pacific Biosciences'](http://pacificbiosciences.com/) long
read aligner.[1]

Installed Version
-----------------

Version 1.3.1 is installed on Proteus. Use the module:

`    pacbio/blasr/gcc/20150513`

Check the version by doing:

`    [juser@proteusa01 ~]$ blasr -version`

Also installed are the various utilities, e.g. alchemy, evolve,
samFilter, etc.

Known Bug
---------

There is an existing bug with multithreaded blasr[2]: running
multithreaded blasr causes it to crash and dump core. There is no
comment by the authors on that bug, though it has been assigned. The
workaround is to run it single-threaded.

General Notes
-------------

-   We use the refactored code, currently (2015-05-13) hosted at
    <https://github.com/ylipacbio/refactored_blasr_II> but which will
    soon replace the main git repository
-   BLASR depends on [HDF5 1.8](/Compiling_HDF5_1.8 "wikilink")

Environment
-----------

`Currently Loaded Modulefiles:`
`  1) shared                      3) gcc/4.8.1                   5) szip/gcc/2.1`
`  2) proteus                     4) sge/univa                   6) hdf5_18/gcc/1.8.14-serial`

Check Out Source
----------------

Clone the repository:

`    [juser@proteusa01 myresearchGrp]$ mkdir src ; cd src`
`    [juser@proteusa01 myresearchGrp]$ git clone git@github.com:ylipacbio/refactored_blasr_II.git --recursive`

### Modify top level Makefile

Edit the Makefile at the top level to remove the "static" flag.

``` make
ifneq ($(OS), Darwin)
    LIBS += -lrt
    ### do not use "-static"
    #STATIC := -static
    STATIC :=
else
    STATIC :=
endif
```

### Modify tools/Makefile

The Makefile in the `tools` subdirectory contains rules which use the
"-static" flag directly, rather than via the $(STATIC) macro. Delete
every instance of "-static".

See Also
--------

-   [Compiling HDF5 1.8](/Compiling_HDF5_1.8 "wikilink")

References
----------

<references/>

[1] [BLASR GitHub project page](https://github.com/PacificBiosciences/blasr)

[2] [BLASR issue #158](https://github.com/PacificBiosciences/blasr/issues/158)

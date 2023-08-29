---
title: SuiteSparse
permalink: /SuiteSparse/
---

**SuiteSparse** is a suite of sparse matrix algorithms.[1] Note that
these are libraries to be linked with your application. They are not
directly executable programs/applications.

Installed Version
-----------------

**SuiteSparse 4.5.3** compiled with **GCC 4.8.1** is available on
Proteus. Use the modulefile:

`    suitesparse/gcc/4.5.3`

The installation includes METIS.[2]

Using SuiteSparse
-----------------

Compile against the installed SuiteSparse by setting your include and
library paths appropriately. The following environment variables are set
by the modulefile:

`    SUITESPARSEDIR=/mnt/HA/opt/suitesparse/gcc/4.5.3`
`    SUITESPARSEINCLUDEDIR=/mnt/HA/opt/suitesparse/gcc/4.5.3/include`
`    SUITESPARSELIBDIR=/mnt/HA/opt/suitesparse/gcc/4.5.3/lib`

References
----------

<references/>

[1] [SuiteSparse website](http://faculty.cse.tamu.edu/davis/suitesparse.html)

[2] [METIS website](http://glaros.dtc.umn.edu/gkhome/metis/metis/overview)
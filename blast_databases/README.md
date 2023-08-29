---
title: BLAST Databases
permalink: /BLAST_Databases/
---

A subset of the BLAST Databases[1] has been downloaded.

Using NCBI BLAST software
-------------------------

The modulefiles:

`   ncbi-blast/`*`version`*

will define two environment variables:

-   BLASTDB - location of BLAST databases
-   BLASTMAT - location of BLAST matrices

Picotte
-------

On Picotte, this is at:

`    /beegfs/blastdb`

### Updates

The databases are automatically updated every weekend.

Proteus
-------

On Proteus, this is at:

`    /lustre/blastdb`

General
-------

You may set your `BLASTDB` environment variable to this directory if you
will be using one of the downloaded databases. The databases are
read-only.

If you use one of the BLAST software installations provided by Proteus,
BLASTDB is already set to this value. Also, BLASTMAT is set to
`/lustre/blastmat` on Proteus, and `/beegfs/blastmat` on Picotte.

Please see the `README.proteus` file in that directory for an up-to-date
list of what is available. Please request database updates or other
databases by sending email to [URCF
Support](mailto:urcf-support@drexel.edu).

See Also
--------

-   [Compiling NCBI BLAST](/Compiling_NCBI_BLAST "wikilink")
-   [Compiling NCBI C++ Toolkit](/Compiling_NCBI_C++_Toolkit "wikilink")

References
----------

<references/>

[1] [BLAST Databases README](https://ftp.ncbi.nlm.nih.gov/blast/documents/blastdb.html)
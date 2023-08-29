---
title: Samtools
permalink: /Samtools/
---

**Samtools** is a suite of programs for interacting with high-throughput
sequencing data. It consists of three separate bundles:

-   **Samtools** is for reading/writing/editing/indexing/viewing
    SAM/BAM/CRAM format.[1]
-   **BCFtools** is for reading/writing BCF2/VCF/gVCF files and
    calling/filtering/summarising SNP and short indel sequence
    variants[2]
-   **HTSlib** is a C library for reading/writing high-throughput
    sequencing data.[3] It is installed as part of SAMtools.

Picotte
=======

Installed Version
-----------------

**HTSlib + BCFtools + Samtools 1.12** is installed. Use the modulefile:

`   samtools/1.12`

Loading a samtools module will automatically load the appropriate
bcftools and htslib modules. (Plus other modules, e.g. Perl.)

You can also load HTSlib and BCFtools separately:

`   htslib/1.12`
`   bcftools/1.12`

Proteus
=======

Installed Versions
------------------

Multiple versions of Samtools + BCFtools + HTSlib are installed. Pick
one:

`    samtools/1.2`
`    samtools/1.3.1`
`    samtools/1.8`
`    samtools/1.10`

Loading a samtools module will automatically load the appropriate
bcftools and htslib modules. (Plus other modules, e.g. Perl.)

References
----------

<references/>

[1] [SAMtools official website](http://www.htslib.org/)

[2]

[3]

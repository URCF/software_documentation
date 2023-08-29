---
title: GroopM
permalink: /GroopM/
---

**GroopM** is an automated tool for the recovery of population genomes
from related metagenomes.[1][2][3] It depends on **BamM**,[4][5] which
is also installed.

**UPDATE Aug 2018** The original authors of GroopM seem to have stopped
developing it. A new author, Tim Lamberton, has taken over and seems to
be doing further development.[6] He has also taken over BamM
development.[7] This GroopM is version 2.0.0, which is accessible using
the command **groopm2**. Please use this version, instead.

Installed Versions
------------------

**GroopM 0.3.5** with **BamM 1.7.3**[8] and **CheckM 1.0.7**[9] is
installed on Proteus. Because it depends on some obsolete Python
modules, it comes with its own installation of Python 2.7.13. Use the
modulefile:

`    groopm/0.3.5 `

The newer version **GroopM 2.0.0** by [10] uses its own Python 2.7.15
and is available with the modulefile:

`    groopm/2.0.0`

This module also loads, among others:

-   `perl/5.20.0`
-   `oracle/jdk/1.7.0_current`
-   `samtools/1.2`
-   `bwa/master`
-   `pplacer/1.1.alpha19`
-   `prodigal/2.6.3`
-   `hmmer/3.1b2`

### Caveat

-   The [QIIME](/QIIME "wikilink") modules provide different versions of
    GroopM and BamM because QIIME's scripts depend on specific versions
    of underlying tools; QIIME may not work properly using non-canonical
    versions. Undetermined behavior may result if both GroopM and QIIME
    modules are loaded together.
-   The same is true of [Python](/Python "wikilink") -- the **groopm**
    module includes its own version of Python 2.7.13 because some of its
    dependencies are not the latest installed on Proteus.

### Using

If you need to use another module which conflicts with the GroopM
module, you will need to unload groopm before loading the other module,
and vice versa:

`    module load othermod/1.2.3`
`    dosomething`
`    module unload othermod`
`    module load groopm`
`    groopm something`

Please be sure to cite the authors of GroopM and BamM appropriately in
any publications. See the official websites for these software for
details.

Compiling
---------

-   The recommended installation method is to use "pip"

### Prerequisites

-   Python 2.7.x
-   BamM: [this one](http://ecogenomics.github.io/BamM/), not [that one](http://bamm-project.org/); which requires
    -   [SAMtools](/SAMtools "wikilink") 1.2
    -   [BWA](/BWA "wikilink") 0.7.12
-   numpy (for CheckM)
-   PyTables 2.x
    -   PyTables 2.x is obsolete and no longer supported. There is an
        error in its setup.py which causes it to incorrectly check the
        numpy version number. The fix (diff file) is here:
        <https://gist.github.com/prehensilecode/2eb790476c38299e520ce5ea40896e08>

### CheckM

-   Installation is done using pip[11]
-   CheckM requires data files to be downloaded and installed once the
    pip install is complete

See Also
--------

-   [QIIME](/QIIME "wikilink")

References
----------

<references/>

[1] [Imelfort M, Parks D, Woodcroft BJ, Dennis P, Hugenholtz P, Tyson GW. (2014) GroopM: an automated tool for the recovery of population genomes from related metagenomes. PeerJ 2:e603](http://dx.doi.org/10.7717/peerj.603)

[2] [GroopM website](http://ecogenomics.github.io/GroopM/)

[3] [GroopM GitHub repository](https://github.com/ecogenomics/GroopM)

[4] [BamM website](http://ecogenomics.github.io/BamM/)

[5] [BamM GitHub repository](https://github.com/ecogenomics/BamM)

[6] [GroopM from Tim Lamberton](https://github.com/timbalam/GroopM)

[7] [BamM from Tim Lamberton](https://github.com/timbalam/BamM)

[8]

[9] [CheckM website](https://ecogenomics.github.io/CheckM/)

[10]

[11] [CheckM wiki - Installation](https://github.com/Ecogenomics/CheckM/wiki/Installation)
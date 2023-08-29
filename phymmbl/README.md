---
title: PhymmBL
permalink: /PhymmBL/
---

**PhymmBL** is software for taxonomic classification of metagenomic
short reads.[1][2]

Prerequisites
-------------

Uses NCBI BLAST

`    module load ncbi-blast`

Installation
------------

PhymmBL installs only in its own directory, downloading all data into
"hidden" directories named:

`    .blastData`
`    .genomeData`
`    .taxonomyData`

To install:

`    [juser@proteusa01 ~]$ mkdir phymmbl`
`    [juser@proteusa01 ~]$ cd phymmbl`
`    [juser@proteusa01 phymmbl]$ wget `[`http://www.cbcb.umd.edu/software/phymm/phymmbl_installer.tar.gz`](http://www.cbcb.umd.edu/software/phymm/phymmbl_installer.tar.gz)
`    [juser@proteusa01 phymmbl]$ tar xf phymmbl_installer.tar.gz`

This creates 4 Perl scripts, and several "hidden" directories -- do "ls
-la" to see them.

To install:

`    [juser@proteusa01 phymmbl]$ ./phymmblSetup.pl`

and follow the prompts.

References
----------

<references/>

[1] [PhymmBL web site](http://www.cbcb.umd.edu/software/phymm/)

[2] [*Phymm and PhymmBL: metagenomic phylogenetic classification with interpolated Markov models*, Brady A., Salzberg, S.L., Nat Methods. 2009 Sep;6(9):673-6, <DOI:10.1038/nmeth.1358>](http://www.ncbi.nlm.nih.gov/pubmed/19648916)
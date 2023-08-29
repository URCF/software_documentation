---
title: Bowtie
permalink: /Bowtie/
---

**Bowtie** is an ultrafast, memory-efficient short read aligner. It
aligns short DNA sequences (reads) to the human genome at a rate of over
25 million 35-bp reads per hour. Bowtie indexes the genome with a
Burrows-Wheeler index to keep its memory footprint small: typically
about 2.2 GB for the human genome (2.9 GB for paired-end).[1]

**Bowtie 2** is an ultrafast and memory-efficient tool for aligning
sequencing reads to long reference sequences. It is particularly good at
aligning reads of about 50 up to 100s or 1,000s of characters, and
particularly good at aligning to relatively long (e.g. mammalian)
genomes. Bowtie 2 indexes the genome with an FM Index to keep its memory
footprint small: for the human genome, its memory footprint is typically
around 3.2 GB. Bowtie 2 supports gapped, local, and paired-end alignment
modes.[2]

Installed Versions
------------------

Both modulefiles for both versions also load a threaded Perl modulefile.

### Bowtie 1

**Bowtie 1.3.1** is installed on Picotte. Use the modulefile:

`    bowtie/1.3.1`

### Bowtie 2

**Bowtie2 2.5.0** is installed. Use the modulefile:

`    bowtie2/2.5.0`

Documentation
-------------

Please refer to the online manuals.[3][4]

Running
-------

t.b.a.

References
----------

<references/>

[1] [Bowtie 1 official website](http://bowtie-bio.sourceforge.net/index.shtml)

[2] [Bowtie 2 official website](https://bowtie-bio.sourceforge.net/bowtie2/index.shtml)

[3] [Bowtie 1 Manual](https://bowtie-bio.sourceforge.net/manual.shtml)

[4] [Bowtie 2 Manual](https://bowtie-bio.sourceforge.net/bowtie2/manual.shtml)
---
title: MMseqs2
permalink: /MMseqs2/
---

**MMseqs2** (Many-against-Many sequence searching) is a software suite
to search and cluster huge protein and nucleotide sequence sets.[1]

Installed Versions
------------------

**MMseqs2 14-7e284** is installed on Picotte. Use the modulefile:

`    mmseqs2/14-7e284`

Using
-----

**MMseqs2** is multithreaded, and will use as many cores as are
available by default.

### Example Usage

``` text
[juser@picotte001 testing]$ module load mmseqs2
[juser@picotte001 testing]$ mmseqs easy-search examples/QUERY.fasta examples/DB.fasta alnRes.m8 tmp
easy-search examples/QUERY.fasta examples/DB.fasta alnRes.m8 tmp

MMseqs Version:                         14-7e284-MPI
Substitution matrix                     aa:blosum62.out,nucl:nucleotide.out
Add backtrace                           false
Alignment mode                          3
Alignment mode                          0
...
MPI Init
Rank: 0 Size: 1
align tmp/3703829652469307901/query tmp/3703829652469307901/target tmp/3703829652469307901/search_tmp/15893234211161012075/pref_0 tmp/3703829652469307901/result --sub-mat 'aa:blosum62.out,nucl:nucleotide.out' -a 0 --alignment-mode 3 --alignment-output-mode 0 --wrapped-scoring 0 -e 0.001 --min-seq-id 0 --min-aln-len 0 --seq-id-mode 0 --alt-ali 0 -c 0 --cov-mode 0 --max-seq-len 65535 --comp-bias-corr 1 --comp-bias-corr-scale 1 --max-rejected 2147483647 --max-accept 2147483647 --add-self-matches 0 --db-load-mode 0 --pca substitution:1.100,context:1.400 --pcb substitution:4.100,context:5.800 --score-bias 0 --realign 0 --realign-score-bias -0.2 --realign-max-seqs 2147483647 --corr-score-weight 0 --gap-open aa:11,nucl:5 --gap-extend aa:1,nucl:2 --zdrop 40 --threads 96 --compressed 0 -v 3

Compute score, coverage and sequence identity
Query database size: 500 type: Aminoacid
Target database size: 20000 type: Aminoacid
Calculation of alignments
Compute split from 0 to 500
[=================================================================] 500 0s 552ms
Time for merging to result_tmp_0: 0h 0m 0s 4ms
68875 alignments calculated
12897 sequence pairs passed the thresholds (0.187252 of overall calculated)
25.794001 hits per query sequence
Time for merging to result: 0h 0m 0s 0ms
Time for processing: 0h 0m 0s 878ms
rmdb tmp/3703829652469307901/search_tmp/15893234211161012075/pref_0 -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/search_tmp/15893234211161012075/aln_0 -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/search_tmp/15893234211161012075/input_0 -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/search_tmp/15893234211161012075/aln_merge -v 3

Time for processing: 0h 0m 0s 0ms
convertalis tmp/3703829652469307901/query tmp/3703829652469307901/target tmp/3703829652469307901/result alnRes.m8 --sub-mat 'aa:blosum62.out,nucl:nucleotide.out' --format-mode 0 --format-output query,target,fident,alnlen,mismatch,gapopen,qstart,qend,tstart,tend,evalue,bits --translation-table 1 --gap-open aa:11,nucl:5 --gap-extend aa:1,nucl:2 --db-output 0 --db-load-mode 0 --search-type 0 --threads 96 --compressed 0 -v 3

[=================================================================] 500 0s 22ms
Time for merging to alnRes.m8: 0h 0m 0s 4ms
Time for processing: 0h 0m 0s 38ms
rmdb tmp/3703829652469307901/result -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/target -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/target_h -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/query -v 3

Time for processing: 0h 0m 0s 0ms
rmdb tmp/3703829652469307901/query_h -v 3

Time for processing: 0h 0m 0s 0ms
```

### Example Job Script

N.B. this does not use multi-node MPI.

Using the examples distributed with the MMseqs2 source code.

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --cpus-per-task=24
#SBATCH --mem-per-cpu=3G
#SBATCH --time=24:00:00

module load mmseqs2

mmseqs easy-search examples/QUERY.fasta examples/DB.fasta alnRes.m8 tmp
```

OpenMP + MPI (multithreading + multi-node parallel)
---------------------------------------------------

The version on Picotte is hybrid-MPI/OpenMP enabled. Please consult the
official MMseqs2 documentation for instructions on using this
capability.

References
----------

<references/>

[1] [MMseqs2 GitHub repository](https://github.com/soedinglab/MMseqs2)
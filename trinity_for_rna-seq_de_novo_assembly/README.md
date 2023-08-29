---
title: Trinity for RNA-Seq De novo Assembly
permalink: /Trinity_for_RNA-Seq_De_novo_Assembly/
---

**Trinity** is software for RNA-Seq De novo Assembly.[1]

Installed Version
-----------------

A [Singularity](/Singularity "wikilink") container for Trinity is
installed on Picotte.[2]

Load the appropriate modulefile:

`    trinity/2.14.0`

Using
-----

The Singularity image is installed in the path

`   $TRINITYDIR/Trinity.simg`

Example command:

``` bash
singularity exec --bind=`pwd`:/tmp -e $TRINITYDIR/Trinity.simg \
          Trinity \
          --seqType fq \
          --left /tmp/reads.left.fq.gz  \
          --right /tmp/reads.right.fq.gz \
          --max_memory 1G --CPU 4 \
          --output /tmp/trinity_out_dir
```

Example job script using the sample data distributed with the Trinity
source code, `trinityrnaseq/sample_data/test_Trinity_Assembly`:

``` bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --cpus-per-task=8
#SBATCH --time=1:00:00
#SBATCH --mem=4G

module load trinity

# NOTE
# this example assumes
# 1) the data files reads.left.fq.gz and reads.right.fq.gz are in the same directory as this job script
# 2) there is an output directory "trinity_out_dir" in the same directory as this job script

singularity exec --bind=`pwd`:/tmp -e $TRINITYDIR/Trinity.simg \
          Trinity \
          --seqType fq \
          --left /tmp/reads.left.fq.gz  \
          --right /tmp/reads.right.fq.gz \
          --max_memory 4G \
          --CPU $SLURM_CPUS_PER_TASK \
          --output /tmp/trinity_out_dir
```

Truncated output from the example:

``` text
/ifs/opt/src/trinityrnaseq/sample_data/test_Trinity_Assembly

     ______  ____   ____  ____   ____  ______  __ __
    |      ||    \ |    ||    \ |    ||      ||  |  |
    |      ||  D  ) |  | |  _  | |  | |      ||  |  |
    |_|  |_||    /  |  | |  |  | |  | |_|  |_||  ~  |
      |  |  |    \  |  | |  |  | |  |   |  |  |___, |
      |  |  |  .  \ |  | |  |  | |  |   |  |  |     |
      |__|  |__|_||____||__|__||____|  |__|  |____/

    Trinity-v2.14.0

Left read files: $VAR1 = [
          '/tmp/reads.left.fq.gz'
        ];
Right read files: $VAR1 = [
          '/tmp/reads.right.fq.gz'
        ];
Trinity version: Trinity-v2.14.0
-currently using the latest production release of Trinity.

Tuesday, November 8, 2022: 12:18:09 CMD: java -Xmx64m -XX:ParallelGCThreads=2  -jar /usr/local/bin/util/support_scripts/ExitTester.jar 0
Tuesday, November 8, 2022: 12:18:09 CMD: java -Xmx4g -XX:ParallelGCThreads=2  -jar /usr/local/bin/util/support_scripts/ExitTester.jar 1
Tuesday, November 8, 2022: 12:18:09 CMD: mkdir -p /tmp/trinity_out_dir/chrysalis

----------------------------------------------------------------------------------
-------------- Trinity Phase 1: Clustering of RNA-Seq Reads  ---------------------
----------------------------------------------------------------------------------

---------------------------------------------------------------
------------ In silico Read Normalization ---------------------
-- (Removing Excess Reads Beyond 200 Coverage --
---------------------------------------------------------------

# running normalization on reads: $VAR1 = [
          [
            '/tmp/reads.left.fq.gz'
          ],
          [
            '/tmp/reads.right.fq.gz'
          ]
        ];

Tuesday, November 8, 2022: 12:18:09 CMD: /usr/local/bin/util/insilico_read_normalization.pl --seqType fq --JM 4G  --max_cov 200 --min_cov 1 --CPU 8 --output /tmp/trinity_out_dir/insilico_read_normalization --max_CV 10000  --left /tmp/reads.left.fq.gz --right /tmp/reads.right.fq.gz --pairs_together  --PARALLEL_STATS
-prepping seqs
Converting input files. (both directions in parallel)CMD: seqtk-trinity seq -A -R 1  <(gunzip -c /tmp/reads.left.fq.gz) >> left.fa

... [other output clipped] ...

All commands completed successfully. :-)

** Harvesting all assembled transcripts into a single multi-fasta file...

Tuesday, November 8, 2022: 12:18:38 CMD: find /tmp/trinity_out_dir/read_partitions/ -name '*inity.fasta'  | /usr/local/bin/util/support_scripts/partitioned_trinity_aggregator.pl --token_prefix TRINITY_DN --output_prefix /tmp/trinity_out_dir/Trinity.tmp
* [Tue Nov  8 12:18:39 2022] Running CMD: /usr/local/bin/util/support_scripts/salmon_runner.pl Trinity.tmp.fasta /tmp/trinity_out_dir/both.fa 8
* [Tue Nov  8 12:18:40 2022] Running CMD: /usr/local/bin/util/support_scripts/filter_transcripts_require_min_cov.pl Trinity.tmp.fasta /tmp/trinity_out_dir/both.fa salmon_outdir/quant.sf 2 > /tmp/trinity_out_dir.Trinity.fasta
Tuesday, November 8, 2022: 12:18:40 CMD: /usr/local/bin/util/support_scripts/get_Trinity_gene_to_trans_map.pl /tmp/trinity_out_dir.Trinity.fasta > /tmp/trinity_out_dir.Trinity.fasta.gene_trans_map

#############################################################################
Finished.  Final Trinity assemblies are written to /tmp/trinity_out_dir.Trinity.fasta
#############################################################################
```

Grid Config
-----------

Currently unavailable on Picotte.

OBSOLETE - Grid Config
----------------------

There is a default grid config file given by the environment variable
`TRINITYGRIDCONF`, which may or may not work for your application. See
the Trinity documentation for details. The default grid config file is
as follows:

``` bash
#--------------------------------------------------------------------------------------------
# grid type:
grid=SGE
# template for a grid submission
cmd=qsub -cwd -j y -l h_rt=2:00:00
# number of grid submissions to be maintained at steady state by the Trinity submission system
max_nodes=8
# number of commands that are batched into a single grid submission job.
cmds_per_node=1
#--------------------------------------------------------------------------------------------
```

References
----------

<references/>

[1] [Trinity RNA-Seq](http://trinityrnaseq.github.io/)

[2] [Trinity Documentation - Running Trinity using Singularity](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Trinity-in-Docker#running-trinity-using-singularity)
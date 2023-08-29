---
title: Nf-core-mag
permalink: /Nf-core-mag/
---

**nf-core/mag** is a bioinformatics best-practice analysis pipeline for
assembly, binning and annotation of metagenomes.[1][2]

Using
-----

**nf-core/mag** uses [Nextflow](/Nextflow "wikilink") and
[Singularity](/Singularity "wikilink"). No additional packages need to
be installed by the user.

### Storage Requirements

nf-core/mag workflows may consume large amounts of disk space, and may
exceed the amount in the local disk on all nodes. Use the [BeeGFS Scratch File System](/BeeGFS_Scratch_File_System "wikilink"), instead.

### Self-Test

**CAUTION** nf-core/mag's self-test requires a lot of storage space
(&gt;750 GiB), and may take several hours to complete. Use `/scratch`
(i.e. BeeGFS); local scratch (i.e. `/local/scratch`) on all nodes is not
sufficient.

Job script to run a Nextflow local executor within a Slurm job:

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --time=48:00:00
#SBATCH --mem=180G
#SBATCH --cpus-per-task=48

module load nextflow

export NXF_SINGULARITY_CACHEDIR=${BEEGFS_TMPDIR}/singularity
mkdir $NXF_SINGULARITY_CACHEDIR

# N.B. $BEEGFS_TMPDIR will be deleted after the job ends
# so, set output directory to somewhere in the group directory structure
cd $BEEGFS_TMPDIR
nextflow run nf-core/mag -profile test_full,singularity --outdir /ifs/groups/somethingGrp/nf-core-mag-test-results
```

References
----------

<references/>

[1] [nf-core/mag GitHub repository](https://github.com/nf-core/mag)

[2] [nf-core/mag nf-core/mag analysis pipeline at nf-core](https://nf-co.re/mag)
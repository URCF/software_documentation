---
title: AlphaFold2
permalink: /AlphaFold2/
---

**AlphaFold2**[1][2][3] predicts protein structures using machine
learning.

In the rest of this document, AlphaFold2 will be referred to as just
"AlphaFold".

Installed Versions
------------------

**AlphaFold 2.2.4** and **2.3.1** are installed on Picotte as a
[Singularity](/Singularity "wikilink") container. Use the appropriate
modulefile:

`   alphafold/2.2.4`
`   alphafold/2.3.1`

The Singularity versions match the released AlphaFold versions:

-   [AlphaFold 2.2.4](https://github.com/deepmind/alphafold/tree/v2.2.4)
-   [AlphaFold 2.3.1](https://github.com/deepmind/alphafold/tree/v2.3.1)

AlphaFold Databases
-------------------

AlphaFold requires 2.3 TB of data to run. That data has been downloaded
to Picotte and is available to all. The data is stored on the
[BeeGFS](/BeeGFS "wikilink") filesystem. The database directory is given
by the environment variable `ALPHAFOLD_DATADIR` once the modulefile is
loaded. The `run_singularity.py` script is aware of the existing
database paths.

Please DO NOT re-download the AlphaFold databases. If updates are
needed, please contact urcf-support@drexel.edu

Running AlphaFold
-----------------

The Singularity container which holds AlphaFold is run by a wrapper
Python script, `run_singularity.py`. Output from AlphaFold will go to
the job's local scratch directory, which must then be copied to a
permanent location (group or home directory).

Note:

1.  AlphaFold can run only on a single node.
2.  AlphaFold should be able to use multiple GPU devices. BUT please
    confirm for yourself that your specific workload will use multiple
    GPUs.
3.  AlphaFold is actually a workflow which runs separate programs. Some
    of those programs are CPU-only.
4.  The AlphaFold modulefile loads the specific version of Python that
    it requires. This may or may not conflict with the version of Python
    you may have set up by default.

To run either of the examples below, first download a FASTA file:

`    [juser@picotte001 ~]$ wget "`[`https://predictioncenter.org/casp14/target.cgi?target=T1050&view=sequence`](https://predictioncenter.org/casp14/target.cgi?target=T1050&view=sequence)`" -O T1050.fasta`

### Running AlphaFold 2.x.x

Create a job script named "alphafoldtest.sh":

``` bash
#!/bin/bash
#SBATCH -p gpu
#SBATCH --time=18:00:00
#SBATCH --gpus=4
#SBATCH --cpus-per-gpu=12
#SBATCH --mem-per-gpu=45G

### FIXME change to appropriate version string
module load alphafold/2.3.1

### Check values of some environment variables
echo ALPHAFOLD_DIR=$ALPHAFOLD_DIR
echo ALPHAFOLD_DATADIR=$ALPHAFOLD_DATADIR

###
### README This runs AlphaFold 2.3.1 on the T1050.fasta file
###

# AlphaFold should use all GPU devices available to the job by default.
#
# To run the CASP14 evaluation, use:
#   --model_preset=monomer_casp14
#
# To benchmark, running multiple JAX model evaluations (NB this
# significantly increases run time):
#   --benchmark

# Run AlphaFold; default is to use GPUs, i.e. "--use_gpu" can be omitted.
python3 ${ALPHAFOLD_DIR}/singularity/run_singularity.py \
    --use_gpu \
    --data_dir=${ALPHAFOLD_DATADIR} \
    --fasta_paths=T1050.fasta \
    --max_template_date=2020-05-14 \
    --db_preset=reduced_dbs \
    --model_preset=monomer

echo INFO: AlphaFold returned $?

### Copy Alphafold output back to directory where "sbatch" command was issued.
mkdir $SLURM_SUBMIT_DIR/Output-$SLURM_JOB_ID
cp -R $TMPDIR $SLURM_SUBMIT_DIR/Output-$SLURM_JOB_ID
```

Submit the job script with: `sbatch alphafoldtest.sh`. N.B. This may
take several hours to complete.

Documentation
-------------

Consult the official documentation for details.[4]

Citing AlphaFold
----------------

If you use AlphaFold in your work, please cite:

Jumper, J., Evans, R., Pritzel, A. *et al.* Highly accurate protein
structure prediction with AlphaFold. *Nature* **596**, 583–589 (2021).
<https://doi.org/10.1038/s41586-021-03819-2>

``` bibtex
@Article{AlphaFold2021,
  author  = {Jumper, John and Evans, Richard and Pritzel, Alexander and Green, Tim and Figurnov, Michael and Ronneberger, Olaf and Tunyasuvunakool, Kathryn and Bates, Russ and {\v{Z}}{\'\i}dek, Augustin and Potapenko, Anna and Bridgland, Alex and Meyer, Clemens and Kohl, Simon A A and Ballard, Andrew J and Cowie, Andrew and Romera-Paredes, Bernardino and Nikolov, Stanislav and Jain, Rishub and Adler, Jonas and Back, Trevor and Petersen, Stig and Reiman, David and Clancy, Ellen and Zielinski, Michal and Steinegger, Martin and Pacholska, Michalina and Berghammer, Tamas and Bodenstein, Sebastian and Silver, David and Vinyals, Oriol and Senior, Andrew W and Kavukcuoglu, Koray and Kohli, Pushmeet and Hassabis, Demis},
  journal = {Nature},
  title   = {Highly accurate protein structure prediction with {AlphaFold}},
  year    = {2021},
  doi     = {10.1038/s41586-021-03819-2},
  note    = {(Accelerated article preview)},
}
```

Source Code
-----------

The source code, particularly the Singularity build recipe and the
`run_singularity.py` script, has been modified for use on Picotte. (The
original source only provides code to use Docker.) The [source code with modification to use Singularity is on GitHub](https://github.com/prehensilecode/alphafold_singularity).

See Also
--------

-   [AlphaFold Databases](/AlphaFold_Databases "wikilink")

References
----------

<references/>

[1] [DeepMind: AlphaFold official website](https://deepmind.com/research/case-studies/alphafold)

[2] [Official AlphaFold repository at Github](https://github.com/deepmind/alphafold)

[3] [Jumper, J., Evans, R., Pritzel, A. et al. Highly accurate protein structure prediction with AlphaFold. Nature 596, 583–589 (2021).](https://dx.doi.org/10.1038/s41586-021-03819-2)

[4]
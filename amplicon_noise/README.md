
<https://code.google.com/p/ampliconnoise/>

Modules
-------

gsl, blas, lapack, proteus-openmpi/gcc

Edit Makefiles
--------------

All files which have

`    CC      = cc`

must be changed to

`    CC      = gcc`

Optimization flags should be:

`    CFLAGS  = -O3 -mavx -mfpmath=sse`

Build
-----

Type "make" and then "make install".

Test
----

Example job script:

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -cwd
#$ -j y
#$ -M myname@drexel.edu
#$ -m bea
#$ -pe fixed16 32
#$ -l h_rt=1:00:00
#$ -l h_vmem=2G
#$ -l vendor=intel

. /etc/profile.d/modules.sh
module load shared
module load proteus
module load sge/univa
module load gcc
module load use.own
module load ampliconnoise

### C005.dat comes with the AmpliconNoise source code distribution
file=C005.dat

defaultPrimer="ATTAGATACCC[ACTG]GGTAG"
primer=${2:-$defaultPrimer}
stub=${file%.dat}

echo "Calculating .fdist file"
mpirun PyroDist -in $file -out ${stub} > ${stub}.fout

echo "Clustering .fdist file"
FCluster -in ${stub}.fdist -out ${stub}_X > ${stub}.fout

rm ${stub}.fdist
rm ${stub}_X.otu ${stub}_X.tree

echo "Running PyroNoise"
mpirun PyroNoiseM -din ${file} -out ${stub}_s60_c01 -lin ${stub}_X.list -s 60.0 -c 0.01 > ${stub}_s60_c01.pout

Truncate.pl 220 < ${stub}_s60_c01_cd.fa > ${stub}_s60_c01_T220.fa

echo "Running SeqDist"
mpirun SeqDist -in ${stub}_s60_c01_T220.fa > ${stub}_s60_c01_T220.seqdist
FCluster -in ${stub}_s60_c01_T220.seqdist -out ${stub}_s60_c01_T220_S > ${stub}_s60_c01_T220.fcout

echo "Running SeqNoise"
mpirun SeqNoise -in ${stub}_s60_c01_T220.fa -din ${stub}_s60_c01_T220.seqdist -lin ${stub}_s60_c01_T220_S.list -out ${stub}_s60_c01_T220_s30_c08 -s 30.0 -c 0.08 -min ${stub}_s60_c01.mapping > ${stub}_s60_c01_T220.snout

rm ${stub}_s60_c01_T220_S.otu ${stub}_s60_c01_T220_S.tree ${stub}_s60_c01_T220.seqdist

echo "Remove degenerate primers"
sed 's/^${primer}//' ${stub}_s60_c01_T220_s30_c08_cd.fa > ${stub}_s60_c01_T220_s30_c08_P.fa
echo "Clustering OTUs"
mpirun NDist -i -in ${stub}_s60_c01_T220_s30_c08_P.fa > ${stub}_s60_c01_T220_s30_c08_P.ndist

FCluster -i -in ${stub}_s60_c01_T220_s30_c08_P.ndist -out ${stub}_s60_c01_T220_s30_c08_P > ${stub}_s60_c01_T220_s30_c08_P.fcout

rm ${stub}_s60_c01_T220_s30_c08_P.ndist

echo "Removing intermediate files"
rm *out
```

References
----------


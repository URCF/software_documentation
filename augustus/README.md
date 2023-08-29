---
title: Augustus
permalink: /Augustus/
---

**Augustus** is a program to find genes and their structures in one or
more genomes.[1][2]

Installed Version
-----------------

**Augustus 3.4.0** is installed on Picotte as a
[Singularity](/Singularity "wikilink") container. Use the modulefile:

`    augustus/3.4.0`

### Running

To run the example, first clone the Augustus repo:

``` text
[juser@picotte001 myrsrchGrp]$ git clone https://github.com/Gaius-Augustus/Augustus.git
[juser@picotte001 myrsrchGrp]$ cd Augustus
```

Then, create this example job script, naming it "`testaugustus.sh`":

``` bash
#!/bin/bash
#SBATCH --time=00:30:00
#SBATCH --mem=64GB

module load augustus/3.4.0

singularity run --bind /ifs/groups/myrsrchGrp/Augustus:/temp $AUGUSTUSDIR/augustus.sif augustus --species=human --UTR=on /temp/examples/example.fa
```

And submit it as usual:

``` text
[juser@picotte001 Augustus]$ sbatch testaugustus.sh
```

This run should take several seconds. The expected output is:

``` text
# This output was generated with AUGUSTUS (version 3.4.0).
# AUGUSTUS is a gene prediction tool written by M. Stanke (mario.stanke@uni-greifswald.de),
# O. Keller, S. König, L. Gerischer, L. Romoth and Katharina Hoff.
# Please cite: Mario Stanke, Mark Diekhans, Robert Baertsch, David Haussler (2008),
# Using native and syntenically mapped cDNA alignments to improve de novo gene finding
# Bioinformatics 24: 637-644, doi 10.1093/bioinformatics/btn013
# No extrinsic information on sequences given.
# Sources of extrinsic information: M RM
# Initializing the parameters using config directory /opt/augustus-3.4.0/config/ ...
# human version. Using species specific transition matrix: /opt/augustus-3.4.0/config/species/human/human_trans_shadow_partial_utr.pbl
# Looks like examples/example.fa is in fasta format.
# We have hints for 0 sequences and for 0 of the sequences in the input set.
#
# ----- prediction on sequence number 1 (length = 9453, name = HS04636) -----
#
# Predicted genes for sequence number 1 on both strands
# start gene g1
HS04636 AUGUSTUS    gene    836 8857    1   +   .   g1
HS04636 AUGUSTUS    transcript  836 8857    .   +   .   g1.t1
HS04636 AUGUSTUS    tss 836 836 .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    836 1017    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    start_codon 966 968 .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 966 1017    .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 1818    1934    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    1818    1934    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 2055    2198    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    2055    2198    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 2852    2995    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    2852    2995    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 3426    3607    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    3426    3607    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 4340    4423    .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    4340    4423    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 4543    4789    .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    4543    4789    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 5072    5358    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    5072    5358    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 5860    6007    .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    5860    6007    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    CDS 6494    6903    .   +   2   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    exon    6494    8857    .   +   .   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    stop_codon  6901    6903    .   +   0   transcript_id "g1.t1"; gene_id "g1";
HS04636 AUGUSTUS    tts 8857    8857    .   +   .   transcript_id "g1.t1"; gene_id "g1";
# protein sequence = [MLARALLLCAVLALSHTANPCCSHPCQNRGVCMSVGFDQYKCDCTRTGFYGENCSTPEFLTRIKLFLKPTPNTVHYIL
# THFKGFWNVVNNIPFLRNAIMSYVLTSRSHLIDSPPTYNADYGYKSWEAFSNLSYYTRALPPVPDDCPTPLGVKGKKQLPDSNEIVEKLLLRRKFIPD
# PQGSNMMFAFFAQHFTHQFFKTDHKRGPAFTNGLGHGVDLNHIYGETLARQRKLRLFKDGKMKYQIIDGEMYPPTVKDTQAEMIYPPQVPEHLRFAVG
# QEVFGLVPGLMMYATIWLREHNRVCDVLKQEHPEWGDEQLFQTSRLILIGETIKIVIEDYVQHLSGYHFKLKFDPELLFNKQFQYQNRIAAEFNTLYH
# WHPLLPDTFQIHDQKYNYQQFIYNNSILLEHGITQFVESFTRQIAGRVAGGRNVPPAVQKVSQASIDQSRQMKYQSFNEYRKRFMLKPYESFEELTGE
# KEMSAELEALYGDIDAVELYPALLVEKPRPDAIFGETMVEVGAPFSLKGLMGNVICSPAYWKPSTFGGEVGFQIINTASIQSLICNNVKGCPFTSFSV
# PDPELIKTVTINASSSRSGLDDINPTVLLKERSTEL]
# Evidence for and against this transcript:
# % of transcript supported by hints (any source): 0
# CDS exons: 0/10
# CDS introns: 0/9
# 5'UTR exons and introns: 0/1
# 3'UTR exons and introns: 0/1
# hint groups fully obeyed: 0
# incompatible hint groups: 0
# end gene g1
###
#
# ----- prediction on sequence number 2 (length = 2344, name = HS08198) -----
#
# Predicted genes for sequence number 2 on both strands
# start gene g2
HS08198 AUGUSTUS    gene    86  2344    1   +   .   g2
HS08198 AUGUSTUS    transcript  86  2344    .   +   .   g2.t1
HS08198 AUGUSTUS    tss 86  86  .   +   .   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    exon    86  582 .   +   .   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    start_codon 445 447 .   +   0   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    CDS 445 582 .   +   0   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    CDS 812 894 .   +   0   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    exon    812 894 .   +   .   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    CDS 1053    1123    .   +   1   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    exon    1053    1123    .   +   .   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    CDS 1208    1315    .   +   2   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    exon    1208    1315    .   +   .   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    CDS 1587    1688    .   +   2   transcript_id "g2.t1"; gene_id "g2";
HS08198 AUGUSTUS    exon    1587    1688    .   +   .   transcript_id "g2.t1"; gene_id "g2";
# protein sequence = [MLPPGTATLLTLLLAAGSLGQKPQRPRRPASPISTIQPKANFDAQQEQGHRAEATTLHVAPQGTAMAVSTFRKLDGIC
# WQVRQLYGDTGVLGRFLLQARGARGAVHVVVAETDYQSFAVLYLERAGQLSVKLYARSLPVSDSVLSGFEQRVQEAHLTEDQIFYFPKY]
# Evidence for and against this transcript:
# % of transcript supported by hints (any source): 0
# CDS exons: 0/5
# CDS introns: 0/5
# 5'UTR exons and introns: 0/1
# 3'UTR exons and introns: 0/0
# hint groups fully obeyed: 0
# incompatible hint groups: 0
# end gene g2
###
# command line:
# augustus --species=human --UTR=on examples/example.fa
```

Reference
---------

<references/>

[1] [Augustus GitHub repository](https://github.com/Gaius-Augustus/Augustus)

[2] Mario Stanke, Mark Diekhans, Robert Baertsch, David Haussler (2008).
Using native and syntenically mapped cDNA alignments to improve de novo
gene finding. Bioinformatics, 24(5), pages 637–644, doi:
[10.1093/bioinformatics/btn013](https://dx.doi.org/10.1093/bioinformatics/btn013)
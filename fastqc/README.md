---
title: FastQC
permalink: /FastQC/
---

**FastQC** aims to provide a simple way to do some quality control
checks on raw sequence data coming from high throughput sequencing
pipelines.[1][2]

Installed Versions
------------------

**FastQC 0.11.9** is installed on Picotte. Use the modulefile:

`   fastqc/0.11.9`

Using
-----

### Remote Display

FastQC has a Graphical User Interface (GUI). To run it, appropriate X11
display software must be installed on your PC (Windows or macOS; Linux
has it installed by default). See:

-   [Tips for Windows Users\#Graphical Display to Windows](/Tips_for_Windows_Users#Graphical_Display_to_Windows "wikilink")
-   [Tips for macOS Users\#Graphical Display](/Tips_for_macOS_Users#Graphical_Display "wikilink")

Then, FastQC can be run interactively on a compute node. See: [Running GUI Applications on Compute Nodes](/Running_GUI_Applications_on_Compute_Nodes "wikilink")

### Multithreading

FastQC is multithreaded. You can request up to 48 CPU cores (1 core per
thread) but there is no guarantee that performance scales linearly with
number of threads.

Specify number of threads by using the `SLURM_NPROCS` environment
variable[3] rather than a literal integer. For example, requesting 16
(up to 48 is possible):

``` text
[juser@picotte001 ~]$ srun --x11 --nodes=1 --ntasks=1 --cpus-per-task=16 --mem=60G --pty /bin/bash
[juser@node042 ~]$ module load fastqc
[juser@node042 ~]$ fastqc --threads $SLURM_NPROCS
```

N.B. the window color may be reversed, i.e. black background.

[Image:FastQC application window.png](/FastQC_application_window.png "wikilink")

References
----------

<references/>

[1] [FastQC web page](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

[2] [FastQC GitHub repository](https://github.com/s-andrews/FastQC)

[3] [Writing Slurm Job Scripts\#Environment Variables](/Writing_Slurm_Job_Scripts#Environment_Variables "wikilink")
---
title: R
permalink: /R/
---

**R**[1] is a language and environment for statistical computing and
graphics. It is also known as GNU S.

Installed Versions
------------------

-   Default **R** is Microsoft R Open (MRO)[2] **4.0.2**. No modulefile
    needed.
-   **4.1.3** via modulefile `R/4.1.3` This version has been built with
    Intel MKL (Math Kernel Library)[3] To use:

`    module load R/4.1.3`

-   **4.2.2** via modulefile `R/4.2.2` This version has been built with
    Intel MKL (Math Kernel Library)[4] To use:

`    module load R/4.2.2`

Interactive R
-------------

R Studio is not available on Picotte. As an alternative,
[JupyterLab](/JupyterLab "wikilink") can be run with an R kernel.

See: [R in Jupyter](/R_in_Jupyter "wikilink")

Third-Party R Packages
----------------------

NOTE if you have packages from another computer, whether Windows, macOS,
or Linux, the packages you installed there cannot be copied and used on
Picotte or Proteus. R packages are often compiled, and must be compiled
(and linked) on the platform which will be running those packages.

You may install third-party R packages privately into your home
directory. From the R prompt (the "repos" and "Ncpus" options are
optional):

``` text
[juser@picotte001 ~]$ module load R
[juser@picotte001 ~]$ R

R version 4.1.3 (2022-03-10) -- "One Push-Up"
Copyright (C) 2022 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)
...

> install.packages("package_name", Ncpus=8)
Warning in install.packages("package_name", Ncpus = 8) :
  'lib = "/ifs/opt/R/4.1.3/lib64/R/library"' is not writable
Would you like to use a personal library instead? (yes/No/cancel) yes
Would you like to create a personal library
‘~/R/x86_64-pc-linux-gnu-library/4.1’
to install packages into? (yes/No/cancel) yes
... [compilation output] ...
* DONE (package_name)

The download source packages are in
     '/local/scratch/somethinghere/downloaded_packages'
>
```

DO NOT use the same library installation location for different versions
of R. Just accept the defaults.

The default installation location should be:
`~/R/x86_64-pc-linux-gnu-library/$R_VERSION`

### Configure a default repository

To avoid being asked for a CRAN repository location every time, add the
following to `~/.Rprofile`:

``` R
## Default repo - cloud.r-project.org will get a server near you
local({r <- getOption("repos")
       r["CRAN"] <- "https://cloud.r-project.org"
       options(repos=r)
})
```

### Useful Packages

A couple of useful utility R packages:

-   [littler](http://dirk.eddelbuettel.com/code/littler.html)
-   [easypackages](https://CRAN.R-project.org/package=easypackages)

### minqa

The `minqa` package has a small bug which prevents it from building
successfully on Picotte. A fixed version is available here:
<https://github.com/prehensilecode/minqa> It only modifies the options
for compiling the package, and makes no changes to the code which does
the computations.

To install, download the `.tar.gz file`. Then, in the command line:

``` text
[juser@picotte001 ~]$ R CMD INSTALL minqa-x.y.z.tar.gz
```

### More Information

Please see R documentation on `install.packages`[5]

doParallel
----------

It is NOT RECOMMENDED to use `doParallel`. Instead, use a job array to
launch multiple computations on multiple nodes with a single job script.
See:

-   Picotte: [Writing Slurm Job Scripts\#Job Arrays](/Writing_Slurm_Job_Scripts#Job_Arrays "wikilink")

Example Job
-----------

-   See: [R vs. Stata benchmark](/R_vs._Stata_benchmark "wikilink")

Example R script -- name this file `testjob.R`

``` R
### Name this file: testjob.R
print(sample(1:3))
print(sample(1:3, size=3, replace=FALSE))  # same as previous line
print(sample(c(2,5,3), size=4, replace=TRUE))
print(sample(1:2, size=10, prob=c(1,3), replace=TRUE))
```

Example job script -- name this file "test_r.sh"

``` bash
#!/bin/bash
#SBATCH -p def
#SBATCH -n 1
#SBATCH --cpus-per-task=8
#SBATCH --mem=64G
#SBATCH --time=2:00:00

### THIS IS FOR PICOTTE

# To use R 4.1.3, uncomment the following line
# module load R/4.1.3

R CMD BATCH testjob.R
```

Submit by:

`   sbatch test_r.sh`

Installing Your Own Copy of R
-----------------------------

See the R section of the [Anaconda](/Anaconda "wikilink") article. Note:
the version of R distributed by Anaconda may be out of date.

See Also
--------

-   For information on migrating from [Stata](/Stata "wikilink") to R,
    please see:
    [<File:GettingStartedInR-Stata.pdf>](/GettingStartedInR-Stata.pdf "wikilink")
-   All versions of [Microsoft R Open](https://mran.microsoft.com/release-history)
-   [Compiling R](/Compiling_R "wikilink")
-   [R Benchmarks](/R_Benchmarks "wikilink")
-   [R vs. Stata benchmark](/R_vs._Stata_benchmark "wikilink")

References
----------

<references/>

[1] [The R Project for Statistical Computing official website](http://www.r-project.org/)

[2] [Microsoft R Open (formerly Revolution Analytics R)](https://mran.microsoft.com/open/)

[3] [Get Started with Intel® oneAPI Math Kernel Library](https://www.intel.com/content/www/us/en/develop/documentation/get-started-with-mkl-for-dpcpp/top.html)

[4] [Get Started with Intel® oneAPI Math Kernel Library](https://www.intel.com/content/www/us/en/develop/documentation/get-started-with-mkl-for-dpcpp/top.html)

[5] [R Documentation - install.packages {utils}](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/install.packages.html)
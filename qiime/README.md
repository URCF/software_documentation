---
title: QIIME
permalink: /QIIME/
---

**QIIME** stands for Quantitative Insights Into Microbial Ecology.[1]

Installed Versions
------------------

QIIME 1 has now been deprecated. QIIME 2™ is the current supported
version.[2]

### QIIME 1

**QIIME 1.8.0** and **1.9.1** are installed on Proteus. To use, load the
appropriate module:

`    qiime/gcc/64/1.8.0`
`    qiime/gcc/64/1.9.1`

Note that if you have been using QIIME 1.9.0, you should update to
1.9.1.[3] Neither of these versions have USEARCH or rtax (which depends
on USEARCH). Please contact the [author of USEARCH directly](http://drive5.com/usearch/) for licensing.

The installed version has its own private version of Python. We do this
because qiime requires outdated versions of several Python modules. The
current list of Python modules installed under this environment is
below. This may be out of date; do "`pip list`" to see what is actually
installed.

    antiorm (1.1.1)
    backports.ssl-match-hostname (3.4.0.2)
    BamM (1.4.2)
    biom-format (1.3.1)
    brokit (0.0.0.dev0)
    certifi (14.5.14)
    cffi (0.9.2)
    click (2.4)
    cogent (1.5.3)
    cryptography (0.8.2)
    Cython (0.22)
    DateTime (4.0.1)
    db (0.0.15)
    db-sqlite3 (0.0.1)
    docutils (0.12)
    emperor (0.9.3)
    enum34 (1.0.4)
    future (0.12.4)
    gdata (2.0.18)
    GroopM (0.3.4)
    h5py (2.3.1)
    ipython (2.1.0)
    Jinja2 (2.7.3)
    MarkupSafe (0.23)
    matplotlib (1.3.1)
    MySQL-python (1.2.5)
    natsort (3.4.0)
    ndg-httpsclient (0.3.3)
    nose (1.3.3)
    numexpr (2.4.3)
    numpy (1.7.1)
    pandas (0.14.1)
    PIL (1.1.7)
    pip (6.1.1)
    pyasn1 (0.1.7)
    pycparser (2.12)
    Pygments (1.6)
    pynast (1.2.2)
    pyOpenSSL (0.15.1)
    pyparsing (2.0.2)
    pyqi (0.3.1)
    pysqlite (2.6.3)
    python-dateutil (2.4.2)
    pytz (2015.2)
    qcli (0.1.0)
    qiime (1.8.0)
    requests (2.6.2)
    scikit-bio (0.1.4)
    scikit-learn (0.15.0)
    scipy (0.14.0)
    setuptools (15.2)
    six (1.9.0)
    Sphinx (1.2.2)
    SQLAlchemy (0.9.7)
    tables (3.1.1)
    tax2tree (1.0.dev0)
    timeseries (0.5.0)
    tornado (4.0)
    urllib3 (1.10.3)
    zope.interface (4.1.1)

If you have settings in your login scripts (.bashrc, .bash_profile,
.bash_login) and a private qiime_config from a prior installation,
disable them before using the system-installed version of QIIME.

### QIIME Parallel

The default script for starting cluster jobs has been set in
`$QIIMEDIR/etc/qiime_config`:

`    cluster_jobs_fp     start_parallel_jobs_proteus.py`

Note that this does not set the project required for qsub. If you work
only in one research group, this should not be an issue since you will
have a default project set.

QIIME Parallel has not been well-tested on Proteus, so expect to do some
experimentation to get settings right for your particular situation.

### Newer biom Format

If you have a file in a newer biom format, and need to convert it back
for it to work with QIIME 1.8.0, you can unload the qiime module, load a
newer Python 2.7 module, do your conversion, and then go back to qiime.

`    [juser@proteusa01 ~]$ module unload qiime`
`    [juser@proteusa01 ~]$ module load python/2.7-current`
`    [juser@proteusa01 ~]$ biom convert -i myfile.biom -o myfile_json.biom --to-json --table-type="OTU table"`
`    [juser@proteusa01 ~]$ module unload python`
`    [juser@proteusa01 ~]$ module load qiime`

### QIIME 2 2018.8 and 2019.7

**QIIME 2** is installed differently from QIIME 1. It uses a Python
"conda environment".[4]

To use QIIME 2, first load the modulefile:

`    qiime2/2018.8.0`

Then, set up your shell environment. If you use bash (the default on
Proteus):

`    . $CONDA_HOME/etc/profile.d/conda.sh`

and if you use csh:

`    source $CONDA_HOME/etc/profile.d/conda.csh`

Finally, activate the conda environment for QIIME 2, and you will see
your prompt change:

`    [juser@proteusa01 ~]$ conda activate qiime2-2018.8`
`    (qiime2-2018.8) [juser@proteusa01 ~]$ `

Once you are done working in qiime2, deactivate the env:

`    (qiime2-2018.8) [juser@proteusa01 ~]$ conda deactivate`

In a job script, you would do something like:

``` bash
#$ -pe shm 64
#$ -l h_rt=...
. /etc/profile.d/modules.sh
module load shared
module load proteus
module load gcc/4.8.1
module load sge/univa
module load qiime2/2018.8.0

. $CONDA_HOME/etc/profile.d/conda.sh
conda activate qiime2-2018.8

...  DO YOUR WORK HERE. ...

### no need to "conda deactivate" at the end since the job is over
```

**qiime2 2019.7** is also available as a conda env using this module.
Follow the instructions for `qiime2-2018.8` above, but replace the env
name with `qiime2-2019.7`

Installing Your Own Version
---------------------------

**NOTE** The following instructions are for QIIME 1.8.0, and are
therefore out of date. QIIME 1.9.1 now installs properly with `pip`.

### Prerequisites

QIIME requires Python 2.7 and numpy 1.7.1. Since the required version of
numpy is older than the current version, you should install Python 2.7.x
(where x is whatever the latest release is) into your group's directory,
and install numpy using that installation. Please see the article on
[Compiling Python](/Compiling_Python "wikilink"). Alternatively, you may
install the [Canopy Express](https://www.enthought.com/canopy-express/)
package, which is a complete Python-based scientific analysis
environment.

These are the prerequisite packages listed in the QIIME documentation

-   cogent &gt;= 1.5.3
-   matplotlib = 1.1.0
-   numpy &gt;= 1.5.1

In addition, the following are required:

-   numpy &lt;= 1.7.1
-   scipy
-   biom-format &lt; 2.0
-   scikit-bio
-   Sphinx

There are other prerequisites which should be picked up by pip.

For a more complete listing: <http://qiime.org/install/install.html>

The instructions here are very likely incomplete. Please refer to the
official documentation[5]

`    [juser@proteusa01 ~]$ module load myresearch-python`
`    [juser@proteusa01 ~]$ pip install numpy-1.7.1`[6]
`    [juser@proteusa01 ~]$ pip install scipy Sphinx matplotlib biom-format`
`    [juser@proteusa01 ~]$ pip install qiime`

### If pip Does Not Work

Download a release source package from here:
<https://github.com/biocore/qiime/releases>

Then:

`   [juser@proteusa01 ~]$ tar xf 1.8.0.tar.gz`
`   [juser@proteusa01 ~]$ cd 1.8.0`
`   [juser@proteusa01 1.8.0]$ python setup.py install | tee install.out`

References
----------

<references/>

[1] [QIIME official website](http://qiime.org/)

[2] [QIIME 2 website](https://qiime2.org)

[3] [QIIME 1.9.1 release notes](https://github.com/biocore/qiime/releases/tag/1.9.1)

[4] [Conda User Guide - Managing Environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)

[5] [QIIME Installation Guide](http://qiime.org/install/install.html)

[6] [`Installing`` ``QIIME`` ``via`` ``pip`](http://qiime.org/install/install.html#installing-qiime-via-pip)
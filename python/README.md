---
title: Python
permalink: /Python/
---

**Python** is a scripting language.[1]

Python 2 End of Life
--------------------

Python 2 reached its end of life on January 1st, 2020[2][3] The
statement from [Guido van Rossum](/Wikipedia:Guido_van_Rossum "wikilink") is reproduced here:

> Let's not play games with semantics. The way I see the situation for
> 2.7 is that EOL is January 1st, 2020, and there will be no updates,
> not even source-only security patches, after that date. Support (from
> the core devs, the PSF, and python.org) stops completely on that date.
> If you want support for 2.7 beyond that day you will have to pay a
> commercial vendor. Of course it's open source so people are also
> welcome to fork it. But the core devs have toiled long enough, and the
> 2020 EOL date (an extension from the originally annouced 2015 EOL!)
> was announced with sufficient lead time and fanfare that I don't feel
> bad about stopping to support it at all.

Picotte
-------

RHEL 8 comes with **Python 3.6** providing the commands `python` and
`python3`.

### Recommendation

We recommend using one of the locally-compiled Pythons, e.g.
`python/gcc/3.10`, and then creating virtual environments (or
venvs)[4][5] Venvs also have the advantage that they can be easily used
by multiple people, e.g. all members of a research group. For an
example, see [Installing TensorFlow 2.11.0 using pip and venv](/Installing_TensorFlow_2.11.0_using_pip_and_venv "wikilink")

We do not recommend Anaconda as it distributes various libraries. Some
of these libraries, specifically GPU-related CUDA libraries, may be
incompatible with the drivers installed on Picotte’s GPU nodes, and
result in either job crashes or even node crashes. Additionally, trying
to share a single Anaconda installation between multiple people can be
error-prone.

### RHEL 8 Python

In addition to Python 3.6, RHEL8 also provides

-   Python 2.7.16, with the command `python2`

### Bright-Provided Python

Bright Cluster Manager provides

-   Python 3.7

with the modulefiles `python3` and `python37` (these refer to the same
module).

### Locally-Installed Versions

There are several versions installed:

-   Anaconda Python - modulefile `python/anaconda3`
-   Python 3.8- modulefile `python/gcc/3.8.6`
-   Python 3.9 - modulefile `python/gcc/3.9.1`
-   Python 3.10 - modulefile `python/gcc/3.10`
-   Python 3.11 - modulefile `python/gcc/3.11`
-   Intel Python 3.7.7 - modulefile `python/intel/2020.4.912`

#### Anaconda Python

To use Anaconda Python, it is not enough to load the modulefile.
Additional setup needs to be done, which will modify your login script
and provide Anaconda Python for all future login sessions. See:
[Anaconda\#URCF-Installed Anaconda](/Anaconda#URCF-Installed_Anaconda "wikilink")

#### Other Versions

For the others, just load the listed modulefile.

Intel® Distribution for Python[6] is a distribution of Python including
various packages which are optimized for performance on Intel
processors.

### Jupyter

See: [Jupyter](/Jupyter "wikilink")

Proteus
-------

**PROTEUS HAS BEEN DECOMMISSIONED**

Locally-Compiled Versions
-------------------------

### Proteus

Proteus provides the following versions -- use the command
**`module avail python`** to see a current list:

-   Python 2.7.9 -- modulefile python/2.7.9
-   Python 2.7.13 -- modulefile python/2.7.13
-   Python 3.4.3 -- modulefile python/3.4.3
-   Python 3.5.0 -- modulefile python/3.5.0
-   Python 3.5.2 -- modulefile python/3.5.2
-   Python 3.6.1 -- modulefile python/3.6.1 or python/intel/2015/3.6.1
-   Python 3.7.4 -- modulefile python/3.7.4 or python/intel/2015/3.7.4

The latest versions have aliased modulefiles:

-   python/2.7-current
-   python/3.5-current
-   python/3.6-current
-   python/3.7-current

These locally-compiled versions are probably faster than the default
versions for linear algebra computations.

If you need to compile your own version, because you need an older
version of numpy or some other module, please see [Compiling Python](/Compiling_Python "wikilink").

#### Installed Modules

All locally-compiled versions have the following add-on modules
installed:

-   numpy[7]
-   scipy[8]
-   matplotlib[9]
-   pandas[10]
-   biopython[11]
-   scikit-bio[12] (Python &gt;= 3.4 only)
-   scikit-learn[13]

#### QIIME

-   See [QIIME](/QIIME "wikilink")

### Intel-Optimized Versions

Intel has Intel-optimized (using MKL etc) versions of Python 2.7, 3.5,
and 3.6 available. There is no charge for the "community-supported"
version. Download at:

`    `[`https://registrationcenter.intel.com/en/forms/?productid=2810`](https://registrationcenter.intel.com/en/forms/?productid=2810)

#### Installed on Proteus

Intel's distribution of Python 3.5 and 3.6 (based on Anaconda[14]) is
installed on Proteus. Use one of the modulefiles:

`   python/intelpython/3.5.3`
`   python/intelpython/3.6.2`

Loading **python/intelpython/3** will give the latest version in the 3.x
series. These versions include [TensorFlow](/TensorFlow "wikilink").

As of Mar 7, 2019, this uses Python 3.7.1

There are a few conda environments installed for specific Python modules
and/or applications. Do "`conda env list`" to see a list of available
environments. Installed environments include the following. Environments
may also use versions of Python different from the Python in the base
Anaconda installation.

-   pystan
-   biopython (uses Python 3.5.5)
-   qiime (uses Python 2.7.15, and QIIME 1.9.1)

To switch to a new conda environment, do:

`   conda activate `*`environment_name`*

For example:

`   [juser@proteusi01 ~]$ conda activate biopython`
`   (biopython) [juser@proteusi01]$`

Note that your prompt will change to include "`(environment_name)`"
prepended to it.

To exit the environment, do:

`    conda deactivate`

For information on conda environments, see:
<https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>

You can also create your own (private) environments for your own
purposes.

### Python Virtualenvs (venv)

You can make use of an existing version of Python on Picotte to install
a virtualenv (venv) containing some set of packages/modules that you
require. This can be shared with other members of your group. For an
example, see: [TensorFlow\#Installing a Private TensorFlow for Your Research Group with venv](/TensorFlow#Installing_a_Private_TensorFlow_for_Your_Research_Group_with_venv "wikilink")

### Pipenv Environments

You can leverage any existing Python installation (via modulefiles, your
own copy, from Canopy), to create a new "clean" virtual installation
with a set of packages that you require for a project using
**pipenv**.[15] You can create multiple environments, each with their
own package requirements.

### Conda Environments

This applies mostly to both the normal Proteus nodes, and the GPU nodes.
Conda environments[16] are a similar idea to virtual environments above,
or pip environments. They allow easy switching between sets of Python
modules to make up one "application", avoiding the possibility that one
module may have a dependency that clashes with another module.

#### On the GPU Nodes

The GPU nodes have an `anaconda3` module which provides several
different conda environments with different versions of Python. Use the
modulefile:

`    python/anaconda3`

This gives Python 3.6

There are some Python frameworks which have requirements which clash:
these frameworks are separated into their own conda environments:

-   caffe
-   caffe2
-   python27
-   python37 -- this contains PyCUDA and PyOpenCL; see [NVIDIA CUDA\#PyCUDA](/NVIDIA_CUDA#PyCUDA "wikilink")
-   pytorch
-   tensorflow -- see [Tensorflow](/Tensorflow "wikilink")

To activate an environment named "`caffe`":

`    conda activate caffe`

Once you have completed working in that environment, you can deactivate
it to return to the default Anaconda Python with its modules:

`    conda deactivate`

#### Your own Anaconda installations

If you have your own Anaconda installation, you must set it up in your
job scripts before running any conda commands.

In your job script, you must have the line:

`   export PATH=/my/path/anaconda3/bin:$PATH`

before you have any "conda activate ..." commands, or even anything that
uses the Python provided by your installation of Anaconda.

### Jupyter

Please see: [JupyterLab](/JupyterLab "wikilink")

### Installing Your Own Python

This should be your **last resort**.

The first two below are popular Python distributions. Both use Intel
Math Kernel Library (MKL)[17] for accelerated performance on Intel CPUs
(but not AMD). Both have their own Python module management system; both
also have pip for installing modules not provided by their repositories.
The third, Intel Distribution for Python, is an Anaconda-based
distribution that also provides optimized libraries for Intel hardware.

-   [Continuum Analytics' Anaconda](https://www.anaconda.com/download/#linux) - This may break
    modulefile on Proteus due to differing versions of the Tcl scripting
    language. Note also that the Anaconda distribution is large, and may
    exceed the 15 GB quota set on Proteus home directories. (Picotte
    home directories have a larger quota.)
-   [Enthought Canopy](https://www.enthought.com/products/canopy/)
-   [Intel Distribution for Python](https://software.intel.com/en-us/distribution-for-python)

You may also compile your own copy. See: [Compiling Python](/Compiling_Python "wikilink")

### Notes about Anaconda

#### General

Anaconda assumes, by default, that is installed for a single user.
Modifications must be made to the user's login scripts (using
`conda init`), which make that installation of Anaconda active on every
login. This makes it awkward to use more than one Anaconda installation
(switching between installations on demand).

#### Picotte

With the increased default storage allocation in home directories (64
GiB), you should be able to install Anaconda to your home directory
rather than using Miniconda.

However, note that each conda environment you create will consume space,
the amount depending on exactly what you install in each conda env.

#### Proteus Only

PLEASE DO NOT ATTEMPT TO INSTALL ANACONDA INTO YOUR HOME DIRECTORY.

If you insist on an Anaconda installation different from the one already
available (see above), please install it in the group directory so that
the installation may be shared among all members of your research group.

The default full installation of Anaconda 3 Python will exceed the 15 GB
quota on home directories. When the quota is reached, load on the
fileserver maxes out from generating all the quota warnings, and will
cause the fileserver to "block" (i.e. stop responding). This affects the
entire cluster. The symptoms for an interactive user is that normal
operations like "ls" and editing a file take a long time (several
seconds to several minutes).

### Setup for Job Scripts

If you do install your own Python, you will likely have to modify your
job script. Add the following line after the module load commands:

`   . ~/.bashrc`

Installing Your Own Python Modules
----------------------------------

**NOTE** before installing your own Python modules, check to see if the
modules you need are already installed in one of the system-wide
Pythons. Some modules need modifications to the standard installation
procedure, and a simple "pip install" will not result in a correct
installation.

Without using the Software Collections, you can install python modules
for yourself using pip. E.g.

`   [juser@proteusi01 ~]$ python3 -m pip install --user scikit-learn`

If you use one of the locally-compiled Pythons, i.e. you did "module
load python/2.7-current", the command is still the same. All
locally-compiled Pythons provide the **pip** command.

You may also want to investigate [Python Virtual Environments (virtualenv)](http://docs.python-guide.org/en/latest/dev/virtualenvs/).

If you get warnings like:

`    InsecurePlatformWarning: A true SSLContext object is not available.`

just install the following:

`    [juser@proteusa01 ~]$ python3 -m pip install --user "requests[security]"`

If the package you want is not listed on PyPI, you can do the following
in the directory containing all the package files that you downloaded
and expanded -- it looks for "`setup.py`":

`    [juser@proteusa01 somepkg]$ python3 -m pip install --user .`

Python in the Cloud
-------------------

### Google Colaboratory

Google Colaboratory is a cloud-hosted Python environment based on
Jupyter. Jupyter Notebooks are stored on your existing Google Drive.
Both Python 2 and Python 3 are available. The hosted instances run on
GPUs.

-   [Google Colaboratory - Welcome Notebook](https://colab.research.google.com/notebooks/welcome.ipynb)
-   [Towards Data Science blog - Getting Started With Google Colab](https://towardsdatascience.com/getting-started-with-google-colab-f2fff97f594c)

Speeding Up Python
------------------

-   [Cython](http://cython.org/) provides a relatively easy way of
    generating fast compiled modules for Python.

Accelerating Python with Numba
------------------------------

Numba is an open source Just-In-Time (JIT) compiler that translates a
subset of Python and NumPy code into fast machine code.[18] It also
allows use of GPUs.

For a brief introduction, see this Youtube video: [Make Python code 1000x Faster with Numba](https://www.youtube.com/watch?v=x58W9A2lnQc)

Parallel Execution
------------------

Python, in general, does not do parallelism across physical nodes. There
are ways to do it, but your software must specifically state that it can
do this. This requires one or more Python packages which provide this
facility, e.g. MPI for Python.

If the software you use does not specifically state that it can do
parallelism across physical nodes, it is at best multithreaded. Please
consult your software documentation for details.

For multithreaded code, use the **shm** PE. See [Writing Job Scripts](/Writing_Job_Scripts "wikilink") for details.

### joblib.Parallel

It is NOT RECOMMENDED to use `joblib.Parallel`. Instead, write a job
array. See: [Writing Job Scripts\#Array Jobs](/Writing_Job_Scripts#Array_Jobs "wikilink")

Or, use `concurrent.futures.ProcessPoolExecutor`[19] (to bypass the
Global Interpreter Lock) or the `ThreadPoolExecutor`

### Python Multiprocessing

It is NOT RECOMMENDED to use `multiprocessing`. Instead, write a job
array: [Writing Job Scripts\#Array Jobs](/Writing_Job_Scripts#Array_Jobs "wikilink")

Or, use `concurrent.futures.ProcessPoolExecutor`[20] (to bypass the
Global Interpreter Lock) or the `ThreadPoolExecutor`

Python has a [multiprocessing module](https://docs.python.org/3.6/library/multiprocessing.html) (also
available in Python 2.7), which provides threaded execution capability.

However, multiprocessing ignores the
[cgroups](/wikipedia:Cgroups "wikilink") system which restricts jobs to
using only the number of slots requested in the jobs. The
multiprocessing module always uses all available CPU cores on the node.
This happens even if there are other jobs running on that node, leading
to an overload condition, where there are more threads/processes running
than there are CPU cores.

The workaround is this -- to request all "full" nodes, by requesting all
cores.

-   If you want to run on AMDs with 64 cores:

`    #$ -pe shm 64`

-   If you want to run on 16-core Intels (the "ua=sandybridge" request
    is to avoid the 20-core Haswell nodes)

`    #$ -pe shm 16`
`    #$ -l ua=sandybridge`

-   If you want to run on 20-core Intels (n.b. there are only 4 of these
    in Proteus, so the wait time may be much longer)

`    #$ -pe shm 20`
`    #$ -l ua=haswell`

In particular, the **[gensim](https://radimrehurek.com/gensim/)**
`models.ldamulticore` object uses multiprocessing and suffers this
issue. See also: [Gensim](/Gensim "wikilink")

Another workaround, but which depends on your being able to control the
use of the multiprocessing package, is to use the following to count the
number of CPU cores actually available to your program:

``` python
import multiprocessing as mp

def f(x):
    return x*x

ncores = len(os.sched_getaffinity(0))

with mp.Pool(ncores) as p:
    print(p.map(f, [1, 2, 3, 4, 5]))
```

References
----------

<references/>

[1] [Python official website](http://www.python.org/)

[2] [Python.org - Sunsetting Python 2](https://www.python.org/doc/sunset-python-2/)

[3] [Python 2 End-of-Life Statement by Guido van Rossum](https://mail.python.org/pipermail/python-dev/2018-March/152348.html)

[4] [Python Documentation » The Python Standard Library » Software Packaging and Distribution » venv](https://docs.python.org/3.10/library/venv.html)

[5] [Real Python - Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)

[6] [Intel® Distribution for Python](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/distribution-for-python.html)

[7] [Numpy website](http://www.numpy.org/)

[8] [Scipy website](http://www.scipy.org/)

[9] [Matplotlib website](http://matplotlib.org/)

[10] [Pandas website](http://pandas.pydata.org/)

[11] [BioPython website](http://biopython.org)

[12] [SciKit-bio website](http://scikit-bio.org/)

[13] [SciKit-Learn website](http://scikit-learn.org/)

[14] [What is Anaconda?](https://www.continuum.io/what-is-anaconda)

[15] [Pipenv: Python Dev Workflow for Humans](https://pipenv.pypa.io/en/latest/)

[16] [Conda User Guide: Managing Environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)

[17] [Intel MKL product website](https://software.intel.com/en-us/mkl)

[18] [Numba website](http://numba.pydata.org/)

[19] \[<https://docs.python.org/3/library/concurrent.futures.html#processpoolexecutor>
The Python Standard Library - concurrent.futures - ProcessPoolExecutor

[20] \[<https://docs.python.org/3/library/concurrent.futures.html#processpoolexecutor>
The Python Standard Library - concurrent.futures - ProcessPoolExecutor
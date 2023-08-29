---
title: Python virtual environments
permalink: /Python_virtual_environments/
---

**Python virtual environments (venv)**[1] are a standard Python
alternative to Conda/Anaconda environments. Multiple venvs may be
created for using multiple Python-based software with differing
requirements.

We suggest reading the Real Python primer[2] on Python venvs for the
rationale, and some examples.

Creating venvs for your research group
--------------------------------------

It can be desirable for all members of a research group to share the
same software installation so that everyone is using the same versions
of some particular Python stack.

In this example, we create two different venvs in a subdirectory of the
(shared) group directory. **RECOMMENDATION** it is best to assign a
single member of the group to be a manager of the software so that
unintended changes are not made to the venvs.

This example can be modified for personal use by putting the venv
directory in your home directory rather than the group directory.

### Load appropriate Python

We will use Python 3.10 for this example. You may use any version.

``` text
[juser@picotte001 ~]$ module load python/gcc/3.10
[juser@picotte001 ~]$ which python3
/ifs/opt/python/gcc/3.10/bin/python3
[juser@picotte001 ~]$ python3 --version
Python 3.10.7
```

### Create a directory to hold all venvs

First, create a directory to hold all venvs:

``` text
[juser@picotte001 ~]$ cd /ifs/groups/myrsrchGrp
[juser@picotte001 myrsrchGrp]$ mkdir Venvs
```

### Create a new venv

We create a new venv called “`py310-analysis`”:

``` text
[juser@picotte001 myrsrchGrp]$ python3 -m venv ./Venvs/py310-analysis
```

N.B. you can also specify the full path to the venv directory, i.e.
`/ifs/groups/myrsrchGrp/Venvs/py310-analysis`

### Activate the venv

Similar to "`conda activate`", we activate the newly-created venv:

``` text
[juser@picotte001 myrsrchGrp]$ cd
[juser@picotte001 ~]$ source /ifs/groups/myrsrchGrp/Venvs/py310-analysis/bin/activate
(py310-analysis) [juser@picotte001 ~]$
```

Note that the command line prompt now has a prefix with the venv name:
“`(py310-analysis)`”

### Install packages

From the venv, use pip[3] to install packages.

N.B. rather than using the “`pip`” command directly, we use
“`python3 -m pip`” to avoid mistakenly using pip from another location
in the system. We must make sure to use pip from the venv.

We first update pip and setuptools, and install the wheel package.

``` text
(py310-analysis) [juser@picotte001 ~]$ python3 -m pip install --upgrade pip setuptools wheel
Requirement already satisfied: pip in .../Venvs/py310-analysis/lib/python3.10/site-packages (22.2.2)
Collecting pip
  Downloading pip-23.0.1-py3-none-any.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 39.3 MB/s eta 0:00:00
Requirement already satisfied: setuptools in .../Venvs/py310-analysis/lib/python3.10/site-packages (63.2.0)
Collecting setuptools
  Downloading setuptools-67.4.0-py3-none-any.whl (1.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.1/1.1 MB 51.5 MB/s eta 0:00:00
Collecting wheel
  Using cached wheel-0.38.4-py3-none-any.whl (36 kB)
Installing collected packages: wheel, setuptools, pip
  Attempting uninstall: setuptools
    Found existing installation: setuptools 63.2.0
    Uninstalling setuptools-63.2.0:
      Successfully uninstalled setuptools-63.2.0
  Attempting uninstall: pip
    Found existing installation: pip 22.2.2
    Uninstalling pip-22.2.2:
      Successfully uninstalled pip-22.2.2
Successfully installed pip-23.0.1 setuptools-67.4.0 wheel-0.38.4
```

Then we install other wanted packages:

``` text
(py310-analysis) [rbatty@picotte001 ~]$ python3 -m pip install numpy scipy pandas
...
Installing collected packages: pytz, six, numpy, scipy, python-dateutil, pandas
Successfully installed numpy-1.24.2 pandas-1.5.3 python-dateutil-2.8.2 pytz-2022.7.1 scipy-1.10.1 six-1.16.0
```

### Try out one of the packages

Quick test to see if one of the packages installed correctly.

``` text
(py310-analysis) [juser@picotte001 ~]$ python3
Python 3.10.7 (main, Sep 20 2022, 11:50:57) [GCC 9.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy as np
>>> print(np.__version__)
1.24.2
>>> Ctrl-D
```

### Deactivate the environment

Once you are done with the environment, deactivate it. Note that the
prompt loses the venv name prefix.

``` text
(py310-analysis) [rbatty@picotte001 ~]$ deactivate
[juser@picotte001 ~]$
```

### Create another venv

We create another venv.

``` text
[juser@picotte001 ~]$ python3 -m venv /ifs/groups/myrsrchGrp/Venvs/py310-sklearn
[juser@picotte001 ~]$ source /ifs/groups/myrsrchGrp/Venvs/py310-sklearn/bin/activate
(py310-sklearn) [juser@picotte001 ~]$ python3 -m pip install -U pip setuptools wheel
...
(py310-sklearn) [juser@picotte001 ~]$ python3 -m pip install scikit-learn
```

Use in job scripts
------------------

To use the venv in a job script, activate the venv as above. You can
also switch venvs in the middle.

``` bash
#!/bin/bash
#SBATCH --time=00:20:00
#SBATCH --mem=4G

### WARNING this is not a complete job script and will not run as-is

module load python/gcc/3.10

source /ifs/groups/myrsrchGrp/Venvs/py310-sklearn/bin/activate

python3 my_machine_learning_script.py

deactivate

source /ifs/groups/myrsrchGrp/Venvs/py310-analysis/bin/activate

python3 my_analysis_script.py
```

See Also
--------

-   For a more detailed example, see: [Installing TensorFlow 2.11.0 using pip and venv](/Installing_TensorFlow_2.11.0_using_pip_and_venv "wikilink")

References
----------

<references/>

[1] [3.11.2 Documentation » The Python Standard Library » Software Packaging and Distribution » venv](https://docs.python.org/3/library/venv.html)

[2] [Real Python - Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)

[3] [Pip documentation](https://pip.pypa.io/en/stable/)
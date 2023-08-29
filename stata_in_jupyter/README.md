---
title: Stata in Jupyter
permalink: /Stata_in_Jupyter/
---

**Jupyter** allows for language kernels other than Python.
[Stata](/Stata "wikilink") 17 allows running Stata within Python using
`pystata`.[1]

Stata Version
-------------

This works for [Stata](/Stata "wikilink") 17. This makes use of Stata’s
[pystata](https://www.stata.com/python/pystata/) Python package.[2] N.B.
DO NOT install `pystata` using pip. The `pystata` package in PyPI
(Python Package Index) is not from StataCorp.

The `pystata` package provides a few ["magic commands"](https://www.stata.com/python/pystata/notebook/Magic%20Commands0.html)
which allow Python to call Stata. This is how Stata runs within a Python
notebook.

Prerequisites
-------------

### VS Code Jupyter setup

This requires that you have VS Code set up to use remote Jupyter
kernels. See: [Jupyter via VS Code](/Jupyter_via_VS_Code "wikilink")

We will use the following Python version (Jupyter is a Python
application; the Jupyter kernel can be other languages):

``` text
[juser@picotte001 ~]$ module load python/gcc/3.10
```

### Stata installation location

You will need to make a note of the installation location of Stata,
given by the environment variable `STATA_SYSDIR`.

``` text
[juser@picotte001 ~]$ module load stata
[juser@picotte001 ~]$ echo $STATA_SYSDIR
/ifs/opt/stata/17
```

Run Jupyter Lab server and create notebook
------------------------------------------

At the shell prompt in the same terminal:

``` text
[juser@picotte001 ~]$ jupyter-lab
... lots of messages ...
    To access the server, open this file in a browser:
        file:///home/rbatty/.local/share/jupyter/runtime/jpserver-14571-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/lab?token=random_characters
     or http://127.0.0.1:8888/lab?token=random_characters
```

Copy one of the URLs shown, and paste into a browser on your PC. You
should see a new Jupyter Lab launcher:

![756px](/Jupyter_Lab_launcher_with_Julia.png "wikilink")

VS Code may also show a popup asking if you want to open a browser using
the forwarded port: click "Open in Browser", and you should see Jupyter
Lab.

Click the "Python" button to create a new Python notebook. In the
notebook, set up for Stata using the `STATA_SYSDIR` value:

``` text
[1]: import stata_setup
[2]: stata_setup.conf('/ifs/opt/stata/mp48/17', 'mp')
```

![756px](/Python_notebook_running_Stata_in_Jupyter_Lab_-_setup.png "wikilink")

![756px](/Python_notebook_running_Stata_in_Jupyter_Lab.png "wikilink")

The notebook file created resides on Picotte, not on your PC.

Installation Instructions
-------------------------

These instructions are if you are using a different Python, i.e. not the
`python/gcc/3.10` modulefile.

All that is needed is the `stata_setup` package available via pip.[3][4]

``` text
[juser@picotte001 ~]$ pip install --user stata_setup
```

References
----------

<references/>

[1] [Official Stata Website - New in Stata 17 - Jupyter Notebook with Stata](https://www.stata.com/new-in-stata/jupyter-notebooks/)

[2] [Stata Documentation - pystata](https://www.stata.com/python/pystata/)

[3] [pystata Documentation - Configuration](https://www.stata.com/python/pystata/install.html)

[4] [PyPI - stata-setup](https://pypi.org/project/stata-setup/)
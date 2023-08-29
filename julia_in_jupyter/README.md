---
title: Julia in Jupyter
permalink: /Julia_in_Jupyter/
---

**Jupyter** allows for language kernels other than Python.

Julia Version
-------------

There are multiple versions of [Julia](/Julia "wikilink") installed on
Picotte. We will be using `julia/1.7.3` in this example.

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

### Install IJulia package

This example uses Julia 1.7.3 but will work with any Julia version.

There is only one package to install: IJulia.[1]

In the terminal in VS Code, run Julia and install the IJulia package --
type the "\]" (right square bracket) and the prompt will change to the
"pkg&gt;" prompt:

``` text
[juser@picotte001 ~]$ module load python/gcc/3.10
[juser@picotte001 ~]$ module load julia/1.7.3
[juser@picotte001 ~]$ julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.7.3 (2022-05-06)
 _/ |__'_|_|_|__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> ]
(@v1.7) pkg> add IJulia
... [lots of output] ...
```

Hit "Ctrl-D" to exit.

Run Jupyter Lab server and create notebook
------------------------------------------

At the shell prompt in the same terminal:

``` text
[juser@picotte001 ~]$ jupyter-lab
... lots of messages ...
    To access the server, open this file in a browser:
        file:///home/juser/.local/share/jupyter/runtime/jpserver-14571-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/lab?token=random_characters
     or http://127.0.0.1:8888/lab?token=random_characters
```

Copy one of the URLs shown, and paste into a browser on your PC. You
should see a new Jupyter Lab launcher:

![756px](/Jupyter_Lab_launcher_with_Julia.png "wikilink")

Click the "Julia 1.7.3" button to create a new Julia notebook.

![756px](/Julia_notebook_in_Jupyter_Lab_with_plot.png "wikilink")

The notebook file created resides on Picotte, not on your PC.

Finishing up
------------

When done, from the Jupyter Lab "File" menu, select "Shut Down".

![512px](/Shut_down_Jupyter_Lab.png "wikilink")

See Also
--------

-   [Jupyter](/Jupyter "wikilink")
-   [Julia](/Julia "wikilink")
-   [DJ Passey - It Only Takes 10 Minutes to add a Julia Kernel to Jupyter](https://medium.com/@djpasseyjr/it-only-takes-10-minutes-to-add-a-julia-kernel-to-jupyter-739490456a2b)

References
----------

<references/>

[1] [IJulia GitHub repository](https://github.com/JuliaLang/IJulia.jl)
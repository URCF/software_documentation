---
title: Jupyter
permalink: /Jupyter/
---

**Jupyter** provides a notebook interface/format for interactive
calculation.[1]

Methods of running Jupyter
--------------------------

There are three methods to run Jupyter:

1.  as a job on a compute node (including GPUs and bigmem nodes) using
    Slurm, presenting in a browser running on your PC. See: [Jupyter Hub](/Jupyter_Hub "wikilink")
2.  using the web browser on your PC, with the Jupyter kernel running on
    `picotte001`. See: [Jupyter via VS Code](/Jupyter_via_VS_Code "wikilink")
3.  using remote display, running a browser on a compute node displaying
    to your PC. This is very laggy. See [this demonstration video on Drexel Streams.](https://1513041.mediaspace.kaltura.com/media/Using%20Jupyter%20Notebook%20in%20a%20web%20browser%20via%20an%20interactive%20job%20on%20URCF%20Picotte/1_bm7yvrt6)

The second method is recommended unless you have a large computation to
do.

Installing Packages Inside a Jupyter Notebook
---------------------------------------------

**DO NOT DO THE FOLLOWING**

-   `!conda install --yes some_package`
-   `!pip install some_package`
-   `%pip install some_package`

For details, please see [this blog post](https://jakevdp.github.io/blog/2017/12/05/installing-python-packages-from-jupyter/).

Instead, create a virtualenv in a normal terminal session. The
virtualenv is like a Conda env.

See Also
--------

-   [Python](/Python "wikilink")
-   [R in Jupyter](/R_in_Jupyter "wikilink")
-   [Julia in Jupyter](/Julia_in_Jupyter "wikilink")
-   [Stata in Jupyter](/Stata_in_Jupyter "wikilink")

References
----------

<references/>

[1] [Project Jupyter web page](https://jupyter.org/)
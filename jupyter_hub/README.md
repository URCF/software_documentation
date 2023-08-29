---
title: Jupyter Hub
permalink: /Jupyter_Hub/
---

**Jupyter Hub**[1] allows for running Jupyter notebooks as Slurm jobs on
compute nodes.

**Jupyter Hub** runs only on Picotte.

Jupyter Hub vs. Jupyter via VS Code
-----------------------------------

The method described in this article runs a Jupyter kernel as a Slurm
job on a compute node. It allows running on the GPU nodes, as well.

The method described in [Jupyter via VS Code](/Jupyter_via_VS_Code "wikilink") runs the Jupyter kernel on the
login node `picotte001`, where other users will be working.

Access
------

Due to the use of a self-signed HTTPS certificate, you must use Firefox
in order to bypass the security restriction of the browser.

**UPDATE 2021-06-09** Some versions of Google Chrome may allow you to
bypass the security restriction, as well.

Access via the URL below; login with your Picotte credentials:

`    `[`https://picottemgmt.urcf.drexel.edu:8000/`](https://picottemgmt.urcf.drexel.edu:8000/)

![thumb\|Jupyter Hub main
interface](/Jupyter_Hub_Main.png "wikilink")

### Per-User Jupyter Server

When you first login to Jupyter Hub, a Jupyter server virtual machine is
created for you. You must be sure to shut this down when you are done.
See below for instructions.

Available Kernels
-----------------

There is only one kernel which runs on the compute nodes as a Slurm job:

-   **Python 3.7 via Slurm** (runs only in the `def` partition, i.e. no
    GPU, no bigmem)

Using Jupyter Hub
-----------------

### Create Notebook

Create a notebook by clicking the "Python 3.7 via Slurm" button in the
Notebook section.

### Open Existing Notebook

In the left sidebar, which is a listing of files in the current
directory, double click on the notebook.

Select kernel by clicking in the upper right corner of the notebook.

![thumb\|Select notebook kernel](/Jupyter_Hub_-_Select_Notebook_Kernel.png "wikilink")

### Close Notebook

In order for the Slurm job running the Python kernel to terminate,
Select “File → Close and Shutdown Notebook”.

[thumb\|Close and Shutdown Notebook](/File:Jupyter_Hub_-_Close_Notebook.png "wikilink")

### Stop Server

In the main Jupyter Hub interface, select “File → Hub Control Panel“
(the URL should be <https://picottemgmt.urcf.drexel.edu:8000/hub/home>)

A new browser tab will open for the Hub Control Panel. Click the red
“Stop My Server” button.

![thumb\|Select File→Hub Control Panel](/Jupyter_Hub_-_Select_Hub_Control_Panel.png "wikilink")

![thumb\|Stop My Server](/Jupyter_Hub_-_Stop_My_Server.png "wikilink")

### Logout

Finally, logout of the Jupyter Hub.

![thumb\|Logout](/Jupyter_Hub_-_Logout.png "wikilink")

See Also
--------

-   [Jupyter](/Jupyter "wikilink")

References
----------

<references/>

[1] [Jupyter Hub documentation](https://jupyterhub.readthedocs.io/en/stable/)
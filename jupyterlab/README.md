---
title: JupyterLab
permalink: /JupyterLab/
---

**JupyterLab** is a successor to Jupyter Notebook.[1] It bypasses the
initial separate filesystem view of the Jupyter Server, and provides
other features.

Kernels
-------

Jupyter Server can provide kernels using different scripting languages.
(Jupyter Server itself is a Python application.) On Picotte, available
kernels are:

-   [Python](/Python "wikilink") 3.10
-   [R](/R "wikilink") 4.1

Prerequisites
-------------

In order to use JupyterLab (or Jupyter Notebook) on Picotte, you will
need an X11 display server installed on your PC.[2][3]

Running JupyterLab via VS Code
------------------------------

This is the preferred method for running Jupyter.

See: [Jupyter via VS Code](/Jupyter_via_VS_Code "wikilink")

Running JupyterLab using Remote Display (X11)
---------------------------------------------

View the [screen recording](https://1513041.mediaspace.kaltura.com/media/Using+JupyterLab+with+Python+and+R+in+a+web+browser+via+an+interactive+job+on+URCF+Picotte/1_5so8ksuq).

Jupyter is a Python application, i.e. you must load a Python modulefile
to use it even if you want to use R.

### Login to Picotte enabling display forwarding

In your SSH client, enable display forwarding (aka X11 forwarding). For
Linux or macOS, add the "-Y" option to "ssh". For MobaXterm on Windows,
it should be enabled by default.

Once you are on `picotte001`, check that the `DISPLAY` environment
variable is set (the value may be different from this example):

``` text
[juser@picotte001 ~]$ echo $DISPLAY
localhost:13.0
```

### Request an interactive job with X11 forwarding

Request an interactive job in the appropriate partition with X11
forwarding enabled. This example shows the `def` partition, i.e. no GPUs
in use.

``` text
[juser@picotte001 ~]$ srun --x11 -p def --cpus-per-task=12 --mem-per-cpu=3G --time=1:00:00 --pty /bin/bash
[juser@node011 ~]$
```

### Run Google Chrome

If you have never run Google Chrome anywhere on Picotte before, Google
Chrome may prompt you to accept it as the default web browser. Do so.

The additional stuff on the commandline is to hide all warning and error
messages that Google Chrome may emit, and to put it in the background.
To "put an application in the background" means to return control to the
terminal so you can continue to type in it.

``` text
[juser@node011 ~]$ google-chrome > /dev/null 2>&1 &
```

You should see the browser window appear.

### Load the python/gcc module

The Jupyter application is an application written in Python.

``` text
[juser@node011 ~]$ module load python/gcc
[juser@node011 ~]$ which python3
/ifs/opt/python/gcc/3.10.2/bin/python3
[juser@node011 ~]$ which jupyter-lab
/ifs/opt/python/gcc/3.10.2/bin/jupyter-lab
```

### Run jupyter-lab

Note the name of the application is **`jupyter-lab`**. You can ignore
all the messages it produces. The "I" in the timstamp indicates they are
just "informational".

``` text
[juser@node011 ~]$ jupyter-lab
[I 2022-05-02 19:50:46.136 ServerApp] jupyterlab | extension was successfully linked.
[I 2022-05-02 19:50:46.145 ServerApp] nbclassic | extension was successfully linked.
[I 2022-05-02 19:50:46.936 ServerApp] notebook_shim | extension was successfully linked.
[I 2022-05-02 19:50:46.936 ServerApp] voila.server_extension | extension was successfully linked.
[I 2022-05-02 19:50:46.992 ServerApp] notebook_shim | extension was successfully loaded.
[I 2022-05-02 19:50:46.993 LabApp] JupyterLab extension loaded from /ifs/opt/python/gcc/3.10.2/lib/python3.10/site-packages/jupyterlab
...
[I 2022-05-02 19:50:47.025 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 2022-05-02 19:50:47.152 ServerApp]

    To access the server, open this file in a browser:
        file:///home/dwc62/.local/share/jupyter/runtime/jpserver-48408-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/lab?token=a73eacb31c0c1dbb52ffc82724b71652f76400f0cb7e4021
     or http://127.0.0.1:8888/lab?token=a73eacb31c0c1dbb52ffc82724b71652f76400f0cb7e4021
[W 2022-05-02 19:50:56.687 LabApp] Could not determine jupyterlab build status without nodejs
[I 2022-05-02 19:50:57.873 ServerApp] Kernel started: 2ed5345f-2151-4c67-b440-637c21b4fd10
```

You should see a new tab appear in Google Chrome. There should be two
panes, with a menu bar on the top. The left main pane is a file browser,
while the right main pane is a Launcher.

![<File:JupyterLab_at_launch.png>](/JupyterLab_at_launch.png "wikilink")

### Create a new notebook with the appropriate kernel

To create a new notebook, look in the Launcher pane on the right. Click
either "Python 3 (ipykernel)" or "R 4.1".

The new notebook will take over the right pane.

![<File:JupyterLab_Python.png>](/JupyterLab_Python.png "wikilink")

![<File:JupyterLab_R.png>](assets/JupyterLab_R.png "wikilink")

### Finishing up

Once you are done working, use the JupyterLab "File" menu to select
"Shut Down". This ensures the Jupyter server is shutdown, along with any
running kernels.

Then, Exit Google Chrome.

References
----------

<references/>

[1] [Project Jupyter website](https://jupyter.org/)

[2] [Tips for macOS Users\#Graphical Display](/Tips_for_macOS_Users#Graphical_Display "wikilink")

[3] [Tips for Windows Users\#Graphical Display to Windows](/Tips_for_Windows_Users#Graphical_Display_to_Windows "wikilink")
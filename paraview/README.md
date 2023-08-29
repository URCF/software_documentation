---
title: ParaView
permalink: /ParaView/
---

**ParaView** is an open-source, multi-platform data analysis and
visualization application.[1]

Installed Versions
------------------

**NOT CURRENTLY WORKING - prebuilt executables do not work because the
login node does not have a display connected, and remote display does
not work for GL 3D graphics. Headless versions should work.**

**ParaView 5.10.1** and **5.11.0** are installed. Use the modulefile:

`    paraview/5.10.1`
`    paraview/5.11.0`

ParaView provides its own copy of Python 3.x with the command `pvpython`
(instead of `python` or `python3`).

### “Headless” versions

Command line-only (headless) versions are also installed. They can be
used with [OpenFOAM](/OpenFOAM "wikilink"), though OpenFOAM provides its
own copy. If you are using OpenFOAM, it is recommended you use the one
provided with OpenFOAM: there is no separate module to load.

`   paraview-headless/5.10.1`
`   paraview-headless/5.11.0`

The headless versions can also serve the ParaView Lite web interface.[2]

To use it, make sure to set up your PC for remote display.[3] This
example uses XQuartz for macOS. N.B. specify a random port number &gt;
1024 (and &lt; 65536).

``` text
[MyName@MyPC ~]$ ssh -X juser@picotte001.urcf.drexel.edu
Password: .....
[juser@picotte001 ~]$ module load paraview-headless/5.11.0
[juser@picotte001 ~]$ pvpython -m paraview.apps.lite --data my_data_dir --port 12847 &
[juser@picotte001 ~]$ google-chrome &
```

The Google Chrome window should appear on your PC. In the search/URL
bar, enter the address

`    localhost:12847`

The example below used port 9999.

![756px](/ParaView_Lite_example_1.png "wikilink")

Then, click on the “CONNECT TO PARAVIEW” rectangle, and you should be
connected to your local session that you launched. Click on the “FILES”
menu to select the data file to view.

![756px](/ParaView_Lite_example_2.png "wikilink")

When done:

1.  Quit Google Chrome
2.  Terminate ParaView Lite in the terminal by first putting it in the
    foreground with the “`fg`” command, and then typing “Ctrl-C” to stop
    it.

![thumbnail\|500px\|[ParaView Gallery](http://www.paraview.org/gallery/)\|Example from Sandia Labs](/Fire_sandia_labs.png "wikilink")

References
----------

<references/>

[1] [ParaView Official Website](http://www.paraview.org)

[2] [ParaView Lite: A customizable Web client for ParaView](https://kitware.github.io/paraview-lite/)

[3] [Running GUI Applications on Compute Nodes](/Running_GUI_Applications_on_Compute_Nodes "wikilink")
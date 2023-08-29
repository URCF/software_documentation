---
title: Mathematica
permalink: /Mathematica/
---

**Mathematica** is technical computing software from Wolfram.[1][2]

Installed Versions
------------------

### Picotte

*'Mathematica 12.3*, **13.0**, and **13.2** are installed on Picotte.
Use the appropriate modulefile:

`   mathematica/12.3`
`   mathematica/13.0`
`   mathematica/13.2`

### Activation

Each user must activate Mathematica for themselves, even though everyone
uses the same installation. This will be a one-time process, but will
have to be repeated for each new version of Mathematica.

-   Create a Wolfram ID using your `@drexel.edu` email address at
    <https://user.wolfram.com> beforehand which gives access to Wolfram
    Alpha cloud resources. It also gives you an Activation Key which you
    will use in the following steps.
    -   After registration, go to:
        <https://user.wolfram.com/portal/myProducts.html>
    -   Near the bottom of that page is a section “My Products and
        Services” which should show “Mathematica for Sites” and a
        download link “Get downloads” in the same line. Click on the
        “Get downloads” link and you will be on an information page for
        “Mathematica for Sites”: you just need the Activation Key (a
        string of text) from that page. You DO NOT need to download
        anything.
-   Run the "`math`" command after loading the module.

``` text
[juser@picotte001 ~]$ module load mathematica
[juser@picotte001 ~]$ math
Mathematica 13.2.0 Kernel for Linux x86 (64-bit)
Copyright 1988-2022 Wolfram Research, Inc.

Mathematica 13.2.0 Kernel cannot find a valid password.

For automatic Web Activation enter your activation key
(enter return to skip Web Activation): [enter/paste your activation key here]
Automatic Web Activation received a password.

Creating password file entry in:
/home/juser/.Mathematica/Licensing/mathpass

In[1]:=
```

Documentation and Training
--------------------------

Documentation is provided within the application, and also online at
<http://reference.wolfram.com>

There are also online training courses provided by Wolfram:
<http://www.wolfram.com/training/courses/> Of particular interest, is
"[HPC: Introduction to HPC and Grid Computing](http://www.wolfram.com/training/courses/hpc012.html)."

Using Mathematica on Picotte
----------------------------

You can use Mathematica interactively, or non-interactively.

### Interactive Use

You can use Mathematica interactively on either login node. However, for
"heavier" use, please request an interactive job which will run on one
of the compute nodes.

To run on the login node, the instructions above for activating
Mathematica will set your environment up for also running Mathematica.

To request an graphical interactive session on a compute node, the
process is similar to the one for
[MATLAB\#Interactive_Sessions](/MATLAB#Interactive_Sessions "wikilink").
The command for the graphical version of Mathematica is (note the
capitalization):

`   Mathematica`

To run Mathematica without a GUI, the command is:

`   math`

### Non-interactive Use (Job Scripts)

You can use a Wolfram language script,[3] or a Mathematica .m file. We
deal with .m files here.

The suggestions here are from [Michigan State University's Inst. for Cyber-Enabled Research](https://wiki.hpcc.msu.edu/display/hpccdocs/Using+Mathematica+in+Batch+Mode)

You can start with an existing Mathematica notebook. The non-interactive
script would just be the input lines. To prepare a script:

-   Clear all output cells. Use the "Cell -&gt; Delete All Output" menu
    item.
-   Select all remaining cells, which should only be input cells. Use
    the "Edit -&gt; Select All" menu item, or a keyboard shortcut.
-   Copy the selected cells as "input text": use the "Edit -&gt; Copy As
    -&gt; Input Text" menu item.
-   Paste the copied text into a new text document using your editor of
    choice. You can do this on your own computer and upload the newly
    created .m script to Proteus.

### Writing Text Output to Files

You can use the "`>>`" (two greater than signs) operator to create the
file and empty it when created (i.e. overwrite any existing data):

``` mathematica
N[Pi, 1000] >>"pi_digits.txt"
```

And you can keep appending to the file by using the "`>>>`" (three
greater than signs) operator:

``` mathematica
N[Pi, 25] >>>"pi_digits.txt"
N[Pi, 50] >>>"pi_digits.txt"
N[Pi, 75] >>>"pi_digits.txt"
```

Consult Mathematica documentation for more complex output handling using
i/o streams and `Write[]`.[4]

### Writing Graphical Output to Files

You can export plots to files with the `Export[]` function:

``` mathematica
Export[ "sine_result.svg", Plot[ Sin[ x ], { x, 0, 2*Pi } ] ]
```

For publication-ready plots, use a vector-based format such as SVG, PS
(PostScript), or PDF.

### Parallel Execution

Mathematica kernels can run multithreaded on multi-core machines. The
default behavior is to use all available cores. That number depends on
the specific machine assigned to the job.

Use the "`sinfo_detail -p def`" command in the shell to see a list of
all nodes in the `def` partition in Picotte; the "CPUS" column shows the
number of cores. Remove the "`-p def`" to see all nodes in all
partitions.

``` text
[juser@picotte001 ~]$ module load slurm_util
[juser@picotte001 ~]$ sinfo_detail -p def
NODELIST      NODES PART       STATE CPUS    S:C:T     GRES   MEMORY FREE_MEM TMP_DISK CPU_LOAD REASON
node001           1 def*   draining@   48   4:12:1   (null)   192000    83516   864000    13.73 maint
node002           1 def*       mixed   48   4:12:1   (null)   192000   186742   864000     3.04 none
node003           1 def*   allocated   48   4:12:1   (null)   192000   100872   864000    48.08 none
node004           1 def*   draining@   48   4:12:1   (null)   192000    75582   864000    16.19 Drained by CMDaemon
node005           1 def*       mixed   48   4:12:1   (null)   192000   186622   864000     3.01 none
node006           1 def*   allocated   48   4:12:1   (null)   192000     4399   864000    48.04 none
node007           1 def*   allocated   48   4:12:1   (null)   192000     1226   864000    48.05 none
...
node070           1 def*   allocated   48   4:12:1   (null)   192000   170831   864000    48.06 none
node071           1 def*   allocated   48   4:12:1   (null)   192000   172282   864000    48.02 none
node072           1 def*   allocated   48   4:12:1   (null)   192000   106453   864000    48.06 none
node073           1 def*   allocated   48   4:12:1   (null)   192000    31668   864000    48.27 none
node074           1 def*   allocated   48   4:12:1   (null)   192000    51996   864000    21.00 none
```

### Cluster Integration

TBA

### Example Job Script

See: [Slurm - Job Script Example 09 Mathematica](/Slurm_-_Job_Script_Example_09_Mathematica "wikilink")

References
----------

<references/>

[1] [Wolfram Mathematica website](https://www.wolfram.com/mathematica/)

[2] [CoE - Mathematica and Wolfram Alpha Pro at Drexel](https://tech.coe.drexel.edu/mathematica/)

[3] [Wolfram Language & System Documentation: Wolfram Language Scripts Tutorial](http://reference.wolfram.com/language/tutorial/WolframLanguageScripts.html)

[4] [Wolfram Language & System Documentation: Write](http://reference.wolfram.com/language/ref/Write.html)
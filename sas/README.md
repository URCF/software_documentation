---
title: SAS
permalink: /SAS/
---

**SAS** is a software suite for statistical analysis, etc.[1]

Picotte
-------

**SAS 9.4** is installed on Picotte. To use, load this modulefile:

`   sas/9.4`

The installation does not include SAS Studio or Simulation Studio.

### Running SAS

SAS is GUI (graphical user interface) application by default. This means
that SAS needs to be able to display its application windows on your PC.
For information on the software you will need on your PC to do this,
see:

-   [Tips for Windows Users\#Graphical Display to Windows](/Tips_for_Windows_Users#Graphical_Display_to_Windows "wikilink")
-   [Tips for macOS Users\#Graphical Display](/Tips_for_macOS_Users#Graphical_Display "wikilink")

It can also be run non-interactively, a.k.a. batch mode. This is how
compute jobs would be submitted.

The command to run SAS (EN) is:

`  sas`

Or one of these, based on language:

-   `sas_en` - English
-   `sas_u8` - UTF-8 Unicode; NB the SAS Display Manager System does not
    fully support UTF-8
-   `sas_es` - Spanish (Castillian)
-   `sas_zh` - Chinese (Simplified)
-   `sas_zt` - Chinese (Traditional)
-   `sas_zt.euc` - Chinese (Trad. - EUC encoding)

#### Interactive, On the Login Node

Please do this only for brief sessions. When SAS is run on the login
node, it will be sharing the resources (CPU cores, memory) of that node
with others who may be doing work on it. Thus, performance may not be
the best. And if SAS uses a lot of resources, it may affect the other
users on the login node. See the next section for details on requesting
a compute node for exclusive use.

To run SAS on `picottelogin`:

``` text
[juser@picotte001 ~]$ module load sas/9.4
[juser@picotte001 ~]$ sas_en
```

this results in the multiple-window SAS application:

![640px](/SAS_GUI_application.png "wikilink")

To quit SAS, in the "SAS: Explorer" window, use the menus to select File
→ Exit…

#### Interactive, On a Compute Node

This is the preferred way to run SAS interactively. This guarantees
exclusive access to all resources on a compute node to do your work,
rather than sharing resources with other logged-in users. Since there
are 74 (standard) compute nodes in Picotte, chances are high that the
wait time for free resources is low.

See: [Running GUI Applications on Compute Nodes](/Running_GUI_Applications_on_Compute_Nodes "wikilink")

#### Non-Interactive (Batch)

Batch mode is the best way to exploit the resources that Picotte offers
to perform large amounts of computation work.

-   Use example code by P. MacDonald at McMaster University:
    <https://ms.mcmaster.ca/peter/s4p03/s4p03_0304/sasnotes.htm>
-   This is the SAS program -- filename `lranova.sas`:

``` text
data crack;
  input id age load agef;
  datalines;
  1  20 11.45 20
  2  20 10.42 20
  3  20 11.14 20
  4  25 10.84 25
  5  25 11.17 25
  6  25 10.54 25
  7  31  9.47 31
  8  31  9.19 31
  9  31  9.54 31
  ;

proc glm data=crack;
  class agef;
  model load = age agef / p;
  output out=crackreg p=pred r=resid;
run;

proc plot data=crackreg;
  plot load*age="*" pred*age="+"/ overlay;
run;
```

-   Job script -- filename `testsas.sh`; submit job with
    "`sbatch testsas.sh`":

``` bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --mem=44G
#SBATCH --account=myrsrchprj
#SBATCH --time=1:00:00

module load sas/9.4

sas lranova.sas
```

-   Output files generated:
    -   `lranova.lst`
    -   `lranova.log`
-   `lranova.lst` contents (blank lines removed):

``` text
                                                           The SAS System                      00:54 Thursday, February 25, 2021   1
                                                         The GLM Procedure
                                                      Class Level Information

                                                 Class         Levels    Values
                                                 agef               3    20 25 31
                                              Number of Observations Read           9
                                              Number of Observations Used           9
^L                                                           The SAS System                      00:54 Thursday, February 25, 2021   2
                                                         The GLM Procedure

                                                    Dependent Variable: load
                                                                Sum of
                        Source                      DF         Squares     Mean Square    F Value    Pr > F
                        Model                        2      4.69668889      2.34834444      17.07    0.0033
                        Error                        6      0.82566667      0.13761111
                        Corrected Total              8      5.52235556
                                         R-Square     Coeff Var      Root MSE     load Mean
                                         0.850487      3.560833      0.370960      10.41778
                        Source                      DF       Type I SS     Mean Square    F Value    Pr > F
                        age                          1      4.03621252      4.03621252      29.33    0.0016
                        agef                         1      0.66047637      0.66047637       4.80    0.0710
                        Source                      DF     Type III SS     Mean Square    F Value    Pr > F
                        age                          0      0.00000000       .                .       .
                        agef                         1      0.66047637      0.66047637       4.80    0.0710
^L                                                           The SAS System                      00:54 Thursday, February 25, 2021   3
                                                         The GLM Procedure
                               Observation             Observed          Predicted           Residual
                                         1          11.45000000        11.00333333         0.44666667
                                         2          10.42000000        11.00333333        -0.58333333
                                         3          11.14000000        11.00333333         0.13666667
                                         4          10.84000000        10.85000000        -0.01000000
                                         5          11.17000000        10.85000000         0.32000000
                                         6          10.54000000        10.85000000        -0.31000000
                                         7           9.47000000         9.40000000         0.07000000
                                         8           9.19000000         9.40000000        -0.21000000
                                         9           9.54000000         9.40000000         0.14000000
                                        Sum of Residuals                        -0.00000000
                                        Sum of Squared Residuals                 0.82566667
                                        Sum of Squared Residuals - Error SS     -0.00000000
                                        First Order Autocorrelation             -0.61749428
                                        Durbin-Watson D                          2.96961378
^L                                                           The SAS System                      00:54 Thursday, February 25, 2021   4
                                               Plot of load*age.  Symbol used is '*'.
                                               Plot of pred*age.  Symbol used is '+'.
 load |
      |
11.50 +
      | *
      |
      |
11.25 +
      |                                                        *
      | *
      |
11.00 + +
      |
      |                                                        +
      |                                                        *
10.75 +
      |
      |
      |                                                        *
10.50 +
      | *
      |
      |
10.25 +
      |
      |
      |
10.00 +
      |
      |
      |
 9.75 +
      |
      |
      |                                                                                                                          *
 9.50 +                                                                                                                          *
      |
      |                                                                                                                          +
      |
 9.25 +
      |                                                                                                                          *
      |
      |
 9.00 +
      |
      --+----------+----------+----------+----------+----------+----------+----------+----------+----------+----------+----------+--
       20         21         22         23         24         25         26         27         28         29         30         31
                                                                    age
NOTE: 6 obs hidden.
```

-   Contents of `lranova.log`:

``` text
1                                                          The SAS System                          00:54 Thursday, February 25, 2021

NOTE: Copyright (c) 2016 by SAS Institute Inc., Cary, NC, USA.
NOTE: SAS (r) Proprietary Software 9.4 (TS1M7)
      Licensed to DREXEL UNIVERSITY - T&R - SFA, Site 70131026.
NOTE: This session is executing on the Linux 4.18.0-147.el8.x86_64 (LIN X64) platform.

NOTE: Analytical products:

      SAS/STAT 15.2
      SAS/ETS 15.2
      SAS/OR 15.2
      SAS/IML 15.2
      SAS/QC 15.2

NOTE: Additional host information:

 Linux LIN X64 4.18.0-147.el8.x86_64 #1 SMP Thu Sep 26 15:52:44 UTC 2019 x86_64 Red Hat Enterprise Linux release 8.1 (Ootpa)

You are running SAS 9. Some SAS 8 files will be automatically converted
by the V9 engine; others are incompatible.  Please see
http://support.sas.com/rnd/migration/planning/platform/64bit.html

PROC MIGRATE will preserve current SAS file attributes and is
recommended for converting all your SAS libraries from any
SAS 8 release to SAS 9.  For details and examples, please see
http://support.sas.com/rnd/migration/index.html

This message is contained in the SAS news file, and is presented upon
initialization.  Edit the file "news" in the "misc/base" directory to
display site-specific news and information in the program log.
The command line option "-nonews" will prevent this display.

NOTE: SAS initialization used:
      real time           0.15 seconds
      cpu time            0.02 seconds

1          data crack;
2            input id age load agef;
3            datalines;

NOTE: The data set WORK.CRACK has 9 observations and 4 variables.
NOTE: DATA statement used (Total process time):
      real time           0.00 seconds
      cpu time            0.01 seconds


13           ;
14
15         proc glm data=crack;
16           class agef;
17           model load = age agef / p;
18           output out=crackreg p=pred r=resid;
19         run;
^L2                                                          The SAS System                          00:54 Thursday, February 25, 2021

20

NOTE: The data set WORK.CRACKREG has 9 observations and 6 variables.
NOTE: The PROCEDURE GLM printed pages 1-3.
NOTE: PROCEDURE GLM used (Total process time):
      real time           0.04 seconds
      cpu time            0.01 seconds


21         proc plot data=crackreg;
22           plot load*age="*" pred*age="+"/ overlay;
23         run;

24
NOTE: There were 9 observations read from the data set WORK.CRACKREG.
NOTE: The PROCEDURE PLOT printed page 4.
NOTE: PROCEDURE PLOT used (Total process time):
      real time           0.00 seconds
      cpu time            0.00 seconds


NOTE: SAS Institute Inc., SAS Campus Drive, Cary, NC USA 27513-2414
NOTE: The SAS System used:
      real time           0.22 seconds
      cpu time            0.05 seconds
```

Moving from SAS to R
--------------------

[R](/R "wikilink") is an open source statistical analysis language. The
advantage over SAS is that it requires no expensive license, and also
provides a terminal-based interface which does not require a graphical
display.

Here are some tutorials for going from SAS to R:

-   Microsoft Azure AI Gallery: SAS to R tutorial
    -   [Part 1](https://gallery.azure.ai/Notebook/SAS-to-R-Tutorial-Part-1-1)
    -   [Part 2](https://gallery.azure.ai/Notebook/SAS-to-R-Tutorial-Part-2-1)
    -   [Part 3](https://gallery.azure.ai/Notebook/SAS-to-R-Tutorial-Part-3-1)

There are also several case studies online by groups who converted from
SAS to R: do a search for "SAS to R".

See Also
--------

-   [SAS Starter Kit](https://www.sas.com/en_us/sas-starter-kit.html)
-   [SAS Technical Support](https://support.sas.com/en/technical-support.html)
-   [SAS® 9.4 Companion for UNIX Environments, Sixth Edition](https://documentation.sas.com/?docsetId=hostunx&docsetTarget=hostunxwhatsnew94.htm&docsetVersion=9.4&locale=en)

References
----------

<references/>

[1] [SAS Website](https://www.sas.com/)
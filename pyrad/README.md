---
title: PyRAD
permalink: /PyRAD/
---

**PyRAD** is software to assemble de novo RADseq loci from
restriction-site associated sequence data (RAD,ddRAD,GBS,PEddRAD,PEGBS)
by Deren Eaton.[1]

Installed Version
-----------------

**pyRAD 3.0.5** is installed on Proteus. Use the following modulefile:

`    pyrad/3.0.5`

It requires these modulefiles to be loaded, first:

`    python/2.7-current`
`    vsearch`
`    muscle`

Usage
-----

Please consult the official documentation for full details.[2] This is
just a brief summary gleaned from following the author's tutorials.

Unlike most Linux programs, the options to define the behavior of the
**pyRAD** program are read from a text file named **`params.txt`**. To
generate a new `params.txt` file, do:

`    [juser@proteusa01 pyrad-tut]$ pyRAD -n`
`            new params.txt file created`

Then, modify `params.txt`. Each line of the file has a description of
the setting.

### Number of Slots

There are two params in pyRAD which control the parallelization:

-   line 7 -- N processors
-   line 37 -- vsearch max threads per job

Some experimentation may need to be done to figure out what the
appropriate values these should take given a particular hardware
platform. See the pyRAD documentation.

Intalling Your Own Version
--------------------------

pyRAD is a set of Python 2.x scripts which require no compilation. They
do, however, require some Python modules/packages. (See pyRAD
documentation.)

pyRAD also requires [VSEARCH](/VSEARCH "wikilink") (or USEARCH[3]) and
[MUSCLE](/MUSCLE "wikilink").

Examples
--------

D. Eaton has provided several tutorials on the pyRAD website.[4]

### Translation of SE RAD Tutorial 3.0

Please see [Job Script Example 05 pyRAD](/Job_Script_Example_05_pyRAD "wikilink")

References
----------

<references/>

[1] [pyRAD official website](http://dereneaton.com/software/pyrad/)

[2]

[3] [USEARCH official website](http://www.drive5.com/usearch/)

[4]
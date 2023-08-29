---
title: Git
permalink: /Git/
---

**Git** is a free and open source distributed version control system
designed to handle everything from small to very large projects with
speed and efficiency.[1] Like other distributed version control systems,
and unlike client-server systems, every git working directory is a
full-fledged repository with complete history and version tracking,
independent of network access.

Installed Versions
------------------

Red Hat Enterprise Linux 6 comes with **git 1.7.1**.

We also have several locally-compiled versions available via modulefiles
-- use "module avail" to see what is currently installed:

`    git/2.16.1`
`    git/2.17.1`
`    git/2.20.1`
`    git/2.23.0`
`    git/2.25.0`

### Which version should I use?

If you get an error message like

`    error: unknown option 'recurse-submodules'`

you will need to use the newer version via the modulefile. In
particular, [GitHub](https://github.com) requires version 2.x

GitHub
------

GitHub has its own command-line interface called **gh**.[2] It is
available on Picotte without using modulefiles.

References
----------

<references/>

[1] [Git official website](http://git-scm.com/)

[2] [GitHub CLI](https://cli.github.com/)
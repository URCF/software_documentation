---
title: Docker
permalink: /Docker/
---

**Docker** is an enterprise container platform.[1] A container is a unit
of sotfware that packages all dependencies together, while still using
the underlying server's kernel (base system).[2]

Docker was originally written to address the issue of enterprise
software. It gave a way to bundle services that needed to run
concurrently but may have had different dependencies. E.g. the web
server may require Perl version 5.30, but the database server may
require Perl version 5.26, and the web server needed to communicate with
the database server both running on the same physical computer. Docker
provides a way to bundle the web server software with all its
dependencies into a separate "container" from the database server
software with all of its own separate dependencies.

Use of Docker has expanded beyond its initial scope. In most cases, it
works very well because the users are single users on single-user
systems: someone wants to run a complex software bundle on the lab
workstation, for example.

Docker requires "root" access in order to work. Root access grants full
control over the server: the ability to read, write, modify, delete any
data, the ability to reconfigure hardware, and the ability to completely
wipe all storage clean unrecoverably. As such, it is not well-suited for
a shared cluster computing system such as Proteus or Picotte. It also
has no support for the high performance networking hardware used in HPC
clusters.

Additionally, the version of Red Hat that runs on Proteus does not
support Docker.

If you require containers, use [Singularity](/Singularity "wikilink")
instead. Singularity can run Docker images.

References
----------

<references/>

[1] [Docker webpage](https://www.docker.com/)

[2] [Docker - What is a container?](https://www.docker.com/resources/what-container)
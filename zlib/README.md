---
title: Zlib
permalink: /Zlib/
---

**Zlib** is a free, unpatented compression library.[1]

Installed Versions
------------------

**Zlib 1.2.3** is installed by default with Red Hat Enterprise Linux. No
modules need to be loaded to use this.

### Optimized Versions

There are at least two optimized versions of Zlib: one created by
Intel[2] (available in the Intel Performance Primitives package), and
another created by CloudFlare[3][4] Third-party benchmarks[5] indicate
that the CloudFlare patches perform the best.

On Proteus, the CloudFlare optimizations on **zlib 1.2.8** compiled with
GCC and with Intel Composer XE 2015 are available. The "Intel" versions
will run only on the Intel-based nodes. The GCC version will run on both
Intel- and AMD-based nodes. These are the modulefiles:

`    zlib/cloudflare/gcc/1.2.8`
`    zlib/cloudflare/intel/2015/1.2.8`

These environment variables are defined by the modules:

`    ZLIBDIR = $ZLIBHOME/lib`
`    ZLIBHOME`
`    ZLIBINC = $ZLIBHOME/include`

References
----------

<references/>

[1] [Zlib official website](http://www.zlib.net/)

[2] [Intel® IPP ZLIB Coding Functions](https://software.intel.com/en-us/articles/intel-ipp-zlib-coding-functions)

[3] [Fighting Cancer: The Unexpected Benefit Of Open Sourcing Our Code](https://blog.cloudflare.com/cloudflare-fights-cancer/), CloudFlare
blog post by Vlad Krasnov

[4] [CloudFlare zlib fork at GitHub](https://github.com/cloudflare/zlib)

[5] [Updated zlib benchmarks](http://www.snellman.net/blog/archive/2015-06-05-updated-zlib-benchmarks/),
blog post by Juho Snellman (2015-06-05) comparing zlib, zlib-cloudflare,
zlib-intel, and zlib-ng
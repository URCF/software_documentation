
Prerequisites
-------------

-   openblas/dynamic
-   gsl/gcc
-   perl-threaded
-   htslib -- version matching BCFtools version

Configure
---------

`   ./configure --prefix=foo --enable-largefile --enable-bcftools-plugins --enable-perl-filters --enable-libgsl --with-cblas=openblas --with-htslib=$HTSLIBDIR`

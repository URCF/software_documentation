---
title: Perl
permalink: /Perl/
---

Red Hat Enterprise Linux 6.5 comes with **Perl 5.10.1**.[1]

Locally-Compiled Versions
-------------------------

### Picotte

-   Perl (with threads) 5.32.1 -- modulefile perl-threaded/5.32.1

### Proteus

Proteus provides the following versions:

-   Perl (with threads) 5.26.2 -- modulefile perl-threaded/5.26.2
-   Perl (with threads) 5.24.1 -- modulefile perl-threaded/5.24.1
-   Perl 5.24.0 -- modulefile perl/5.24.0
-   Perl 5.22.1 -- modulefile perl/5.22.1
-   Perl 5.20.3 -- modulefile perl/5.20.3
-   Perl 5.20.0 -- modulefile perl/5.20.0
-   Perl 5.18.2 -- modulefile perl/5.18.2

### Modules Installed

This list is not exhaustive. Please email URCF Support if you would like
specific modules installed. Or install them into your own directory (see
next section).

-   PDL (Perl Data Language)[2]
-   BioPerl[3]

You can use this Perl script to display all installed modules:

``` perl
#!/usr/bin/env perl
### from http://www.perlmonks.org/?node_id=868212
use strict;
use warnings;
use feature qw(:5.12);

use ExtUtils::Installed;
use Module::CoreList;
use Module::Info;

my $inst  = ExtUtils::Installed->new();
my $count = 0;
my %modules;
foreach ( $inst->modules() ) {
    next if m/^[[/:lower:|:lower:]]/;    # skip pragmas
    next if $_ eq 'Perl';       # core modules aren't present in this list,
                                # instead coming under the name Perl
    my $version = $inst->version($_);
    $version = $version->stringify if ref $version; # version may be returned as
                                                    # a version object
    $modules{$_} = { name => $_, version => $version };
    $count++;
}
foreach ( Module::CoreList->find_modules() ) {
    next if m/^[[/:lower:|:lower:]]/;    # skip pragmas
    my $module = Module::Info->new_from_module($_) or next;
    $modules{$_} = { name => $_, version => $module->version // q(???) };
    $count++;
}
foreach ( sort keys %modules ) {
    say "$_ v$modules{$_}{version}";
}
say "\nModules: $count";
__END__
```

Installing Modules into Home
----------------------------

-   See this Perl FAQ: [How do I keep my own module/library directory?](http://learn.perl.org/faq/perlfaq8.html#How-do-I-keep-my-own-module-library-directory)
-   Using a non-standard @INC path:
    -   [How do I add the directory my program lives in to the module/library search path?](http://learn.perl.org/faq/perlfaq8.html#How-do-I-add-the-directory-my-program-lives-in-to-the-module-library-search-path)
    -   [How to change @INC to find Perl modules in non-standard locations](https://perlmaven.com/how-to-change-inc-to-find-perl-modules-in-non-standard-locations)
        -- Basically, set the environment variable `PERL5LIB`

References
----------

<references/>

[1] [Perl official website](http://www.perl.org/)

[2] [Perl Data Language website](http://pdl.perl.org/)

[3] [BioPerl website](http://www.bioperl.org/)
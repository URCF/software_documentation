---
title: AlphaFold Databases
permalink: /AlphaFold_Databases/
---

[AlphaFold](/AlphaFold "wikilink") requires some databases to run. These
databases may be different depending on the version of AlphaFold being
used. These have been downloaded to Picotte in:

`    /beegfs/AlphaFoldDatabases (for AlphaFold 2.2.4)`
`    /beegfs/AlphaFoldDatabases-2-3-1 (for AlphaFold 2.3.1)`

Rather than using the actual path, the AlphaFold modulefile defines the
environment variable `ALPHAFOLD_DATADIR` correctly for the version of
AlphaFold being used.

UniProt
-------

The UniProt database contains too many files to be stored on either of
Picotte's storage systems. Instead, the UniProt API should be used for
programmatic access to only the data needed.

For info on programmatic access to UniProt, see this video seminar:

`   `[`https://www.youtube.com/watch?v=-2g3nFhZkzo`](https://www.youtube.com/watch?v=-2g3nFhZkzo)

and written documentation:

`   `[`https://www.uniprot.org/help/api`](https://www.uniprot.org/help/api)



### This code is associated with the paper from Zaaijer et al., "Rapid re-identification of human samples using portable DNA sequencing". eLife, 2017. http://dx.doi.org/10.7554/eLife.27798



Sample re-identification pipeline: 
================================


This pipeline enables the usage of the minION generated DNA reads for re-identification of DNA samples. 



Cell line authentication: 
============================
Re-identification of cell lines in (pre-) clinical research is crucial to verify working materials. Using the MinION in conjunction with our identification pipeline allows sample authentication on-site: either in the lab or in the clinic. 

Requirements for running the pipeline are a cell line database and minION reads. 

Database: 
----------
The database for Cancer Cell line authentication is available
at   https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE36139 .

The Cancer Cell line database is built from data generated by the CCLE (Broad Institute:  https://portals.broadinstitute.org/ccle/). 

MinION reads: 
-------------
The MinION libraries can be prepared by the appropriate genomics library preparation method provided by Oxford Nanopore Technologies. Input material ranges from 200ng-1000ng. 
https://nanoporetech.com/


Re-identification of forensic samples:
=======================================

Database: 
--------------
The database for forensic purposes is not provided here to respect genetic privacy of the individuals in our database. 

Publically available genomes: 
OpenSNP: https://opensnp.org/genotypes


MinION reads: 
---------------
The MinION libraries can be prepared by the appropriate genomics library preparation method provided by Oxford Nanopore Technologies. 




Analysis pipeline
===================

Prerequisites
-------------

The pipeline requires the following components:

* wget, GNU sed, and other standard unix command-line programs
* samtools, bgzip, tabix <http://www.htslib.org/>
* BWA <https://github.com/lh3/bwa>
* Python 2.7 with following python modules:
    * poretools <https://github.com/arq5x/poretools>
    * numpy, scipy
    * pysam
* For plotting - R <www.r-project.org> with these libraries:
    * hexbin
    * RColorBrewer
    * gplots
    * naturalsort
    * optparse

Detailed Installation Instructions
----------------------------------

These programs have been tested on Ubuntu 14.04 64bit GNU/Linux machine.
Other environments might require some adjustments.

Download the latest pipeline code

    # Using GIT
    git clone https://github.com/TeamErlich/personal-identification-pipeline
    cd personal-identification-pipeline

    # Or using ZIP
    wget https://github.com/TeamErlich/personal-identification-pipeline/archive/master.zip
    unzip master.zip

The `setup` directory contains helper script to install the required software:

    # Install required packages:
    sudo ./setup/setup-ubuntu1404.sh

    # Install python modules:
    sudo pip install -r ./setup/requirements.txt

    # Install samtools 1.3.1 (will use 'sudo' automatically)
    ./setup/setup-samtools.sh

    # Install bgzip/tabix 1.3.1 (will use 'sudo' automatically)
    ./setup/setup-htslib.sh

    # Install BWA 0.7.15 (will use 'sudo; automatically)
    ./setup/setup-bwa.sh


Building data files
-------------------

The personal-identification pipeline requires few pre-processed data files.

Download hg19 reference genome, build BWA index (this will take some time,
depending on the machine's hardware. About ~70m on a 2.5Ghz Intel XEON E5):

    ./setup/setup-hg19.sh

Download dbSNP-138 Common and build db (requires downloading ~620MB,
will take some time depending on the network speed):

    ./setup/setup-snp138common.sh

Optionally, download Yaniv Erlich's genotype file:

    ./setup/setup-YE-genotype.sh

Example
-------

The `demo` directory contains a simplified example of the pipeline workflow.
See `./demo/README.md` for more details.


Test re-ID:
-----------------

DIR THP1 FAST5 files: 
https://s3.amazonaws.com/archive-store7/thp1-minion-files.tar.gz

THP1 SNP-array-file:
https://github.com/TeamErlich/personal-identification-pipeline/tree/master/data

Example usage for THP1 re-id: 
```
./run-personal-id-pipeline.sh test-run THP1-FAST5-dir/ thp1-snp-file-dir/ hg19/hg19.fa
```

-or-

```
./run-personal-id-pipeline.sh –s snp138Common.fixed.txt.gz test-run THP1-FAST5-dir/ thp1-snp-file-dir/ hg19/hg19.fa

```

Help and Usage information
--------------------------

The following scripts support help and usage information with
the `--help` parameter (`-h` in case of the shell script):

    run-personal-id-pipeline.sh -h
    poretools-basenames.py --help
    sam-to-bedseq.py --help
    sam-discard-dups.py --help
    generate-snp-list.py --help
    calc-match-probs.py --help
    calc-match-probs-parallel.sh --help
    filter-high-prob-matches.sh --help


“Rapid DNA Re-Identification for Cell Line Authentication and Forensics” Zaaijer et al., 2017

Contact
-------

Yaniv Erlich <yaniv@cs.columbia.edu>
Sophie Zaaijer <szaaijer82@gmail.com>

<http://TeamErlich.org>


License
-------

Copyright (C) 2016 Yaniv Erlich (yaniv@cs.columbia.edu)

All Rights Reserved.
This program is licensed under GPL version 3.
See LICENSE file for full details.

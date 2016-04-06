# w4CSeq: software and web application to analyze 4C-Seq data.

w4CSeq applies a customized pipeline to deal with both enzyme digestion and sonication fragmentation based 4C-seq data. 
It identifies 4C sites, statistically significant regions, draws interacting plots for intra-chromosome and inter-chromosome interactions, and overlays genomic features (TSS, TTS, CpG sites), DNA replication timing and user-provided annotation onto it. 
The combined plots will help uncover significant features in/around 4C regions. 

## Features
* Easy to download and install: Users can download the software and install their own servers in a local environment, and then use command line to analyze raw 4C-Seq data.
* Able to analyze 4C-Seq data generated by both enzyme-digestion and sonication-fragmentation methods.
* Simple input specification and multiple outputs.


## Usage
### *demo web server*
* We provide a demo server [w4CSeq](http://w4cseq.wglab.org/) for you to examine. 

### *build your own server*

##### Download and Install
Users can download the cutting edge version from GitHub by git clone git@github.com:WGLab/w4CSeq.git.

##### Software prerequisite
The following softwares should be installed in your cluster before running w4CSeq command line.
  * R (Packages: RCircos, quantsmooth )
  * Perl (Module: Math::CDF)
  * BWA
  * SAMtools
  * BEDTools
 
You can modify and specify the locations of these softwares in 4C_enzyme.R and 4C_sonication.R.

##### Genome Sequence and Index files
In each sub-directory under lib/, provide genome sequence and index files. For example, under /var/www/html/w4cseq/lib/hg19/, you have to put the following files: genome.fa, genome.fa.amb, genome.fa.ann, genome.fa.bwt, genome.fa.pac, genome.fa.sa. The same has to be done for /var/www/html/w4cseq/lib/hg18/, /var/www/html/w4cseq/lib/mm10/, and /var/www/html/w4cseq/lib/mm9/. Those files can be easily downloaded from [Illumina iGenomes](http://support.illumina.com/sequencing/sequencing_software/igenome.html).

Once these are set up, the server is ready to go.

### *One-line command*
Alternatively, you can use one-line command to analyze your 4C-Seq data.

* Enzyme digestion based 4C-Seq data analysis
```
/var/www/html/w4cseq/bin/4C_enzyme.R 1 /PATH/TO/YOUR/FILE/enzyme.fastq.gz hg19 AAGGCAAATTGCCTGAGCTC GAGCTC chr10 104418100 104418600 500 200 5000 enzyme no no
```
Arguments:
  1. **1**: number of threads. 1 by default and applicable to bwa alignment
  2. **/PATH/TO/YOUR/FILE/enzyme.fastq.gz**: full path to the raw fastq file
  3. **hg19**: reference genome
  4. **AAGGCAAATTGCCTGAGCTC**: primer sequence for bait region
  5. **GAGCTC**: recognition sequence for restriction enzyme
  6. **chr10**: bait chromosome
  7. **104418100**: starting position of primer pair
  8. **104418600**: ending position of primer pair
  9. **500**: bin size for trans chromosome (count of enzyme sites)
  10. **200**: bin size for cis chromosome (count of enzyme sites)
  11. **5000**: window size for cis chromosome (count of enzyme sites)
  12. **enzyme**: working directory name (which means outputs will be generated under ./enzyme/ directory.)
  13. **no**: whether your data is uncompressed
  14. **no**: whether to include additional annotation files, supposed to be "no" for command line usage

* Sonication fragmentation based 4C-Seq data analysis
```
/var/www/html/w4cseq/bin/4C_sonication.R 1 /PATH/TO/YOUR/FILE/sonication_1.fastq.gz /PATH/TO/YOUR/FILE/sonication_2.fastq.gz mm10 chr17 35504676 35504824 500 2000000 400000 12000000 sonication no no
```
Arguments:
  1. **1**:number of threads. 1 by default and applicable to bwa alignment
  2. **/PATH/TO/YOUR/FILE/sonication_1.fastq.gz**: full path to the paired end raw fastq files #1
  3. **/PATH/TO/YOUR/FILE/sonication_2.fastq.gz**: full path to the paired end raw fastq files #2
  4. **mm10**: reference genome
  5. **chr17**: bait chromosome
  6. **35504676**: starting position of primer pair
  7. **35504824**: ending position of primer pair
  8. **500**: Extended length from end of primer pair to define "bait" neighborhood
  9. **2000000**: bin size for trans chromosome (bp)
  10. **400000**: bin size for cis chromosome (bp)
  11. **12000000**: window size for cis chromosome (bp)
  12. **sonication**: working directory name (which means outputs will be generated under ./sonication/ directory.)
  13. **no**: whether your data is uncompressed
  14. **no**: whether to include additional annotation files, supposed to be "no" for command line usage




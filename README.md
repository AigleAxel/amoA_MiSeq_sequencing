# amoA_MiSeq_sequencing

Existing tools based ‘assembly pipeline’ and ‘gap pipeline’ for paired-end MiSeq Illumina reads

Those existing tools based pipelines were designed to assemble paired-end V3 MiSeq Illumina reads (2x300 bp) for functional genes with critical length (up to 490 bp, primers gene specific included for the ‘assembly pipeline’ and larger than 490 bp (set for 620 bp, primers gene specific included) for the ‘gap pipeline’). 

Compare to existing 'ready to use' pipeline, those pipelines were implemented with an amino acid translation check step, to provide a supplementary control for functional gene validity. 

Those pipelines include three independent scripts:

#Script_1: for both ‘assembly’ and ‘gap’ pipeline: trimming (using Trim Galore), to be saved in your bin folder (don’t forget to allow executing file as program, right click on the script, “Properties”, “Permissions”, click the box “Allow executing file as program”)

#Script_2: for both ‘assembly’ and ‘gap’ pipeline: cleaning forward/reverse reads and size selection (using DADA2 on R), to be paste on your R console

#Script_3_assembly: for the ‘assembly pipeline’: assembly, assembled reads size selection, dereplication, translation check, chimeras and singletons removal (using PEAR, usearch, transeq), to be saved in your bin folder (don’t forget to allow executing file as program, right click on the script, “Properties”, “Permissions”, click the box “Allow executing file as program”)

#Script_3_gap: for the ‘gap pipeline’: merge forward and reverse reads, dereplication, translation check, chimeras and singletons removal (using usearch, transeq), to be saved in your bin folder (don’t forget to allow executing file as program, right click on the script, “Properties”, “Permissions”, click the box “Allow executing file as program”)

To run it, you will need to install on Linux (in your bin folder):

Trim Galore (http://www.bioinformatics.babraham.ac.uk/projects/trim_galore/), PEAR (‘assembly pipeline’ only) (https://cme.h-its.org/exelixis/web/software/pear/), usearch (https://drive5.com/usearch/), transeq (http://emboss.sourceforge.net/download/), seqtk (https://github.com/lh3/seqtk), fasta2tab (https://github.com/shenwei356/bio_scripts/tree/master/sequence) and gzip (https://archlinux.pkgs.org/rolling/archlinux-core-x86_64/gzip-1.10-1-x86_64.pkg.tar.xz.html).

You will need to install on R:

DADA2 (https://benjjneb.github.io/dada2/dada-installation.html)

Important parameters that you may have to set according to your dataset:

#Script_1: for both ‘assembly’ and ‘gap’ pipeline: trimming step: set --clip_R1 and --clip_R2 according to your primers size (we recommend to remove the totality of the forward and reverse primers). Important recommendation: for the ‘gap pipeline’, you will need to ensure that your forward and reverse reads get the same reading frame. Advice: use ‘seqtk seq -a sample-R1.fastq > sample-R1.fasta’ and ‘seqtk seq -a sample-R2.fastq > sample-R2.fasta’ followed by ‘transeq -sequence sample-R1.fasta -outseq sample-R1-translated.fasta -frame 1, 2 or 3 -table 11 -clean’ and  ‘transeq -sequence sample-R2.fasta -outseq sample-R2-translated.fasta -frame 1, 2 or 3 -table 11 -clean’ to check your forward and reverse reads reading frame. Correct your forward and/or reverse reading frame by adding or removing 1 or 2 bases from --clip_R1/R2. 

Optional recommendation: if R is not installed on Linux, use ‘tar -czvf trimmed-reads-to-be-cleaned.tar.gz /path/to/directory/trimmed/reads’ to transfer your files from Linux to Windows (and from Windows to Linux). 

#Script_2: Cleaning and forward/reverse reads size selection: (1) ‘assembly pipeline’: ensure at least 10 bp overlapping between your forward and your reverse reads (truncLen and minLen options) (2) ‘gap pipeline’: truncLen=c(200,200) and minLen = 200 are advised

#Script_3_assembly: 
Assembly: no recommendation
Assembled reads size selection: set -trunclen according to your expected final amplicons size
Reads names: in our script, reads are renamed as ‘>AOB-sample-name-1 to n’, you can change ‘AOB’ into ‘your-tag’: row 9: sed 's/^/>your- tag-'${sample}'-/' , and row 19: sed 's/your-tag/>your-tag/'
Dereplication : no recommendation
Translation check: set the reading frame (-frame 1, 2 or 3) according to your amplicons reading frame. Advice: use ‘transeq -sequence reads-to-translate.fasta -outseq translated-reads.fasta -frame 1, 2 or 3 -table 11 -clean’ to check your amplicons reading frame.
Chimera and singletons removal: optional recommendation: set the singletons removal minimum abundance (-minsize) to a value that suit your experimental design and questions.   

#Script_3_gap: 
Merge forward and reverse reads: no recommendation
Reads names: in our script, reads are renamed as ‘>AOA-sample-name-1 to n’, you can change ‘AOA’ into ‘your-tag’: row 10: sed 's/^/>your- tag-'${sample}'-/' 
Dereplication : no recommendation
Translation check: set the reading frame (-frame 1, 2 or 3) according to your amplicons reading frame (see Script_1 for more recommendation). 
Chimera and singletons removal: optional recommendation: set the singletons removal minimum abundance (-minsize) to a value that suit your experimental design and questions.   

To be cited if you are using one of those pipelines:

Callahan BJ, McMurdie PJ, Rosen MJ, Han AW, Johnson AJA, Holmes SP. DADA2: high-resolution sample inference from Illumina amplicon data. Nat Methods. 2016;13:581–3.
Edgar RC. Search and clustering orders of magnitude faster than BLAST. Bioinformatics. 2010;26:2460–1.
Edgar RC. UNOISE2: improved error-correction for Illumina 16S and ITS amplicon sequencing. BioRxiv. 2016. https://doi.org/10.1101/081257 
Krueger F. Trim galore: a wrapper tool around Cutadapt and FastQC to consistently apply quality and adapter trimming to FastQ files. 2015. http://www.bioinformatics.babraham.ac.uk/projects/trim_galore/
Zhang J, Kobert K, Flouri T, Stamatakis A. PEAR: a fast and accurate Illumina paired-end reAd mergeR. Bioinformatics. 2014;30:614–20.





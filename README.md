E. coli, a common bacterium, offers a fascinating case study of evolution in action. We're exploring the 
Ara-3 population, which evolved a remarkable ability to utilize a previously inaccessible food source. To 
understand how this adaptation came about, we'll be comparing its DNA to its ancestor, REL606. By 
decoding the differences between their genetic blueprints, we can pinpoint the key mutations that fueled 
Ara-3's success. This will involve aligning the DNA of Ara-3 samples to the original strain's genome and 
identifying any differences, then analyzing these changes to understand their potential impact on Ara-3's 
adaptation.

steps involved in this variant calling analysis:

• Alignment to reference genome.The alignment process consists of two steps:

▪ Indexing the reference genome

▪ Aligning the reads to the reference genome

• the reference genome for E. coli REL606 The name is CP000819.1 Escherichia coli B str. REL606, complete genome. chromosome name (CP000819.1) 

• The first step is to index the reference genome for use by BWA.

• The alignment process consists of choosing an appropriate reference genome to map our reads against and then deciding on an aligner. We will use the BWA-MEM algorithm, which is the latest and is generally recommended for high-quality queries as it is faster and more accurate.

• The SAM file, is a tab-delimited text file that contains information for each individual read and its alignment to the genome. 

• The compressed binary version of SAM is called a BAM file. We use this version to reduce size and to allow for indexing, which enables efficient random access of the data contained within the file.

• convert the SAM file to BAM format using the samtools program with the view command and tell this command that the input is in SAM format (-S) and to output BAM format (-b)

• Next we sort the BAM file using the sort command from samtools. -o tells the command where to write the output

• SAM/BAM files can be sorted in multiple ways, e.g. by location of alignment on the chromosome, by read name, etc. It is important to be aware that different alignment tools will output differently sorted SAM/BAM, and different downstream tools require differently sorted alignment files as input. You can use samtools to learn more about this bam file as well.

• first pass on variant calling by counting read coverage with bcftools. We will use the command mpileup. The flag -O b tells bcftools to generate a bcf format output file, -o specifies where to write the output file, and -f flags the path to the reference genome: Detect the single nucleotide variants (SNVs)

• Identify SNVs using bcftools call. We have to specify ploidy with the flag --ploidy, which is one for the haploid E. coli. -m allows for multiallelic and rare-variant calling, -v tells the program to output variant sites only (not every site in the genome), and -o specifies where to write the output file:

• You will see the header (which describes the format), the time and date the file was created, the version of bcftools that was used, the command line parameters used, and some additional information

Output:

##fileformat=VCFv4.2
##FILTER<ID=PASS,Description="All filters passed">
##bcftoolsVersion=1.8+htslib-1.8
##bcftoolsCommand=mpileup -O b -o results/bcf/SRR2584866_raw.bcf -f
data/ref_genome/ecoli_rel606.fasta results/bam/SRR2584866.aligned.sorted.bam
##reference=file://data/ref_genome/ecoli_rel606.fasta
#CHROM POS ID REF ALT QUAL FILTER INFO FORMAT
results/bam/SRR2584866.aligned.sorted.bam
CP000819.1 1521 . C T 207 .
DP=9;VDB=0.993024;SGB=-0.662043;MQSB=0.974597;MQ0F=0;AC=1;AN=1;DP4=0,0,4,5;
MQ=60
CP000819.1 1612 . A G 225 .
DP=13;VDB=0.52194;SGB=-0.676189;MQSB=0.950952;MQ0F=0;AC=1;AN=1;DP4=0,0,6,5;
MQ=60

The first few columns represent the information we have about a predicted variation.In an ideal world, the information in the QUAL column would be all we needed to filter out bad variant calls. the grep and wc commands you have learned to assess how many variants are in the vcf
file.

• visualization tools are useful for exploratory analysis .For us to visualize the alignment files, we will need to index the BAM fileusing ‘samtools’


• Viewing with tviewSamtools implements a very simple text alignment viewer based on the GNU ncurses library,called tview. This alignment viewer works with short indels and shows MAQ consensus.


• Viewing with IGV(Integrated Graphics Viewer)

IGV is a stand-alone browser, which has the advantage of being installed locally and providingfast access. Web-based genome browsers, like Ensembl or the UCSC browser, are slower, butprovide more functionality. They not only allow for more polished and flexible visualization, but also provide easy access to a wealth of annotations and external data sources.
STEPS:
1. Open IGV.
2. Load our reference genome file (ecoli_rel606.fasta) into IGV using the “Load Genomes
from File…“ option under the “Genomes” pull-down menu.
3. Load our BAM file (SRR2584866.aligned.sorted.bam) using the “Load from File…“
option under the “File” pull-down menu.
4. Do the same with our VCF file (SRR2584866_final_variants.vcf).

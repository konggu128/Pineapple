# Pineapple CAM Project
# download CAM RNA-Seq data;
In NCBI SRA, the paired reads were joined in one fastq, therefore flag --split-files would be used to split reads
```{php}
nano download.sh
export PATH=/data/home/qcheng1/Transcriptome/sratools/sratoolkit.2.8.2-centos_linux64/bin:$PATH
for i in $(cat download.txt)
do
 fastq-dump --split-files -A $i
done
```
# download reference genome
```{php}
mkdir reference && cd reference
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/001/540/865/GCF_001540865.1_ASM154086v1/GCF_001540865.1_ASM154086v1_genomic.fna.gz
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/001/540/865/GCF_001540865.1_ASM154086v1/GCF_001540865.1_ASM154086v1_genomic.gff.gz
gunzip *.gz
```
# fastqc
```{php}
mkdir 1_fastqc && cd 1_fastqc
module load fastqc
qrsh -q medium* mem=10G
fastqc ../raw_data/*.fastq -o ./
```
# trimming (trimmomatic)
```{php}
mkdir 2_trim && cd 2_trim
module load trimmomatic
nano trim.sh
trimmomatic \
PE \
-trimlog SRR6511 \
-phred33 \
../raw_data/*11_1.fastq \
../raw_data/*11_2.fastq \
SRR6511_1.paired.trimmed.fastq \
SRR6511_1.unpaired.trimmed.fastq \
SRR6511_2.paired.trimmed.fastq \
SRR6511_2.unpaired.trimmed.fastq \
SLIDINGWINDOW:4:15 \
MINLEN:30
>& 6511.output
```
# fastqc

mkdir 3_fastqc && cd 3_fastqc











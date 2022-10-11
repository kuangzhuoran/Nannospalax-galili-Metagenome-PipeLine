# Nannospalax-galili-Metagenome-PipeLine
This is the pipeline I have used to complete the spalax metagenome data analysis

# step1. Quality control and Remove host
```
1.1
trimmomatic PE -threads 20 raw.1.fq raw.2.fq clean.1.fq 1_se clean2.fq 2_se SLIDINGWINDOW:4:20 MINLEN:50 LEADING:2 TRAILING:2
或者
fastp -i reads.1.fq.gz -I reads.2.fq.gz -o clean.1.fq.gz -O clean.2.fq.gz -q 20 -u 30 -n
##更推荐使用fastp

1.2
kneaddata_database --download spalax_genome bowtie2 /path/to/human_db
##去除人类基因组序列，kneaddata自动下载

bowtie2-build spalax.genomic.fa /path/to/spalax_db/spalax
##简单来说就是把spalax的基因组下载下来，用bowtie2建立索引
kneaddata -i clean.1.fq.gz -i clean.2.fq.gz -o C1 --output-prefix C1 -t 20 --serial -db /path/to/spalax_db -db /path/to/human_db --bypass-trim 
```

# step2. Assembly and binning
```
megahit -1 C1.1.fastq -2 C1.2.fastq -o C1

```

# Variant Calling for RNA seq Data
#1- Installing STAR for alignning RNA reads

conda install -c bioconda star 
conda install -c bioconda/label/cf201901 star 



# 2- Generating genome index
# STAR use genome index files that saved in unique directories. ( ch22 will be used not the whole genome)

genomeDir=/home/ngs-01/workdir/Assignment2/chr22_with_ERCC92
mkdir $genomeDir
STAR --runMode genomeGenerate --genomeDir $genomeDir --genomeFastaFiles /home/ngs-01/workdir/sample_data/chr22_with_ERCC92.fa  --runThreadN 1 --sjdbGTFfile /home/ngs-01/workdir/sample_data/chr22_with_ERCC92.gtf


# 3- Alignment jobs were executed as follows:

runDir=/home/ngs-01/workdir/Assignment2/1pass
mkdir $runDir
cd $runDir
STAR --genomeDir $genomeDir --readFilesIn /home/ngs-01/workdir/Assignment2/ngs2-assignment-data/SRR8797509_1.part_001.part_001.fastq /home/ngs-01/workdir/Assignment2/ngs2-assignment-data/SRR8797509_2.part_001.part_001.fastq --runThreadN 4


# 4- For the 2-pass STAR, a new index is then created using splice junction information contained in the file sjdbList.out.tab from the first pass:

genomeDir=/home/ngs-01/workdir/Assignment2/chr22_with_ERCC92_2pass
mkdir $genomeDir
STAR --runMode genomeGenerate --genomeDir $genomeDir --genomeFastaFiles /home/ngs-01/workdir/Assignment2/chr22_with_ERCC92.fa    --sjdbFileChrStartEnd /home/ngs-01/workdir/Assignment2/chr22_with_ERCC92/sjdbList.out.tab --sjdbOverhang 75 --runThreadN 4


# 5- The resulting index is then used to produce the final alignments as follows:

runDir=/home/ngs-01/workdir/Assignment2/2pass
mkdir $runDir
cd $runDir
STAR --genomeDir $genomeDir --readFilesIn /home/ngs-01/workdir/Assignment2/ngs2-assignment-data/SRR8797509_1.part_001.part_001.fastq /home/ngs-01/workdir/Assignment2/ngs2-assignment-data/SRR8797509_2.part_001.part_001.fastq --runThreadN 4



 

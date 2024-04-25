

# Introduction

This is LIFE4136 project rotation2 group4. The group members are Vanessa, Charlotte and I. The project is about protein structural modelling to predict the effects of the mutant proteins. And my group focus on  g5766.t1 and g5766.t gene.

# Requirements

- Linux or macOS

# Installation

- [IGV](https://igv.org/)

- [pyMOL](https://pymol.org/#download)

- [Swiss Model](https://swissmodel.expasy.org/)

- [Alpha Fold](https://colab.research.google.com/github/deepmind/alphafold/blob/main/notebooks/AlphaFold.ipynb)

# Data

- Reference_genome
  - C_excelsa_V5_braker2_wRseq.gff3
  - C_excelsa_V5.dict
  - C_excelsa_V5.fasta
  - C_excelsa_V5.fasta.fai

- VCFs
  - UK_scan_dips.vcf
  - UK_scan_dips.vcf.idx
  - UK_scan_tets.vcf
  - UK_scan_tets.vcf.idx

C_excelsa_V5.fasta and UK_scan_tets.vcf are too big to upload

# Commands

**Install bcftools and samtools**

```brew brew install bcftools```

```shellbrew install samtools```

**zip the diploids vcf: UK_scan_dips.vcf.gz**

```bgzip UK_scan_dips.vcf```
**index the compressed file**

```tabix -p vcf UK_scan_dips.vcf.gz```

**zip the tetraploids: UK_scan_tets.vcf.gz**

```bgzip UK_scan_tets.vcf```
**index it**

```tabix -p vcf UK_scan_tets.vcf.gz```



**g5766.t1**
n.b. the g5766.t1 gene is read backwards
create a filtered gff3 file for the g5766.t1 gene

```grep "g5766.t1" C_excelsa_V5_braker2_wRseq.gff3 | grep "CDS" > g5766.t1.gff3```

**create an output file of the diploid g5766.t1 gene**

```bcftools view -r Cexcelsa_scaf_4:27471771-27471836,Cexcelsa_scaf_4:27471978-27472061,Cexcelsa_scaf_4:27472160-27472288,Cexcelsa_scaf_4:27472382-27472450,Cexcelsa_scaf_4:27472531-27472611,Cexcelsa_scaf_4:27472700-27472804,Cexcelsa_scaf_4:27472891-27473121,Cexcelsa_scaf_4:27473210-27474400,Cexcelsa_scaf_4:27474735-27475839,Cexcelsa_scaf_4:27475929-27476050,Cexcelsa_scaf_4:27476143-27476292,Cexcelsa_scaf_4:27476446-27476607,Cexcelsa_scaf_4:27476687-27476845,Cexcelsa_scaf_4:27476939-27477055,Cexcelsa_scaf_4:27477147-27477257,Cexcelsa_scaf_4:27477347-27477979 UK_scan_dips.vcf.gz -i 'AF>0.5' -m 1 --types snps -o dips_g5766.t1.vcf```

**zip the output file and index:**

```bgzip dips_g5766.t1.vcf```

```tabix -p vcf dips_g5766.t1.vcf.gz```

**filter the reference file for just the g5766.t1 gene**

```shellsamtools faidx C_excelsa_V5.fasta Cexcelsa_scaf_4:27471771-27471836 \
Cexcelsa_scaf_4:27471978-27472061 \
Cexcelsa_scaf_4:27472160-27472288 \
Cexcelsa_scaf_4:27472382-27472450 \
Cexcelsa_scaf_4:27472531-27472611 \
Cexcelsa_scaf_4:27472700-27472804 \
Cexcelsa_scaf_4:27472891-27473121 \
Cexcelsa_scaf_4:27473210-27474400 \
Cexcelsa_scaf_4:27474735-27475839 \
Cexcelsa_scaf_4:27475929-27476050 \
Cexcelsa_scaf_4:27476143-27476292 \
Cexcelsa_scaf_4:27476446-27476607 \
Cexcelsa_scaf_4:27476687-27476845 \
Cexcelsa_scaf_4:27476939-27477055 \
Cexcelsa_scaf_4:27477147-27477257 \
Cexcelsa_scaf_4:27477347-27477979 > reference_g5766.fasta
```

**create a consensus sequence for the diploid g5766.t1 gene**

```bcftools consensus -s - -f ../Reference_Genome/reference_g5766.fasta dips_g5766.t1.vcf.gz > ```

```unfiltered_consensus_dips_g5766.t1_sequence.fasta```

```shellgrep -v ">" unfiltered_consensus_dips_g5766.t1_sequence.fasta > semi_filtered_consensus_dips_g5766.t1_sequence.fasta
tr -d '\n' < semi_filtered_consensus_dips_g5766.t1_sequence.fasta > consensus_dips_g5766.t1_sequence.fasta
```

Warning: ignoring overlapping variant starting at Cexcelsa_scaf_4:27477243
The site Cexcelsa_scaf_4:27477719 overlaps with another variant, skipping...

**repeat for tetraploid**

```bcftools view -r Cexcelsa_scaf_4:27471771-27471836,Cexcelsa_scaf_4:27471978-27472061,Cexcelsa_scaf_4:27472160-27472288,Cexcelsa_scaf_4:27472382-27472450,Cexcelsa_scaf_4:27472531-27472611,Cexcelsa_scaf_4:27472700-27472804,Cexcelsa_scaf_4:27472891-27473121,Cexcelsa_scaf_4:27473210-27474400,Cexcelsa_scaf_4:27474735-27475839,Cexcelsa_scaf_4:27475929-27476050,Cexcelsa_scaf_4:27476143-27476292,Cexcelsa_scaf_4:27476446-27476607,Cexcelsa_scaf_4:27476687-27476845,Cexcelsa_scaf_4:27476939-27477055,Cexcelsa_scaf_4:27477147-27477257,Cexcelsa_scaf_4:27477347-27477979 UK_scan_tets.vcf.gz -i 'AF>0.5' -m 1 --types snps -o tets_g5766.t1.vcf```

```bgzip tets_g5766.t1.vcf```

```tabix -p vcf tets_g5766.t1.vcf.gz```

```bcftools consensus -s - -f ../Reference_Genome/reference_g5766.fasta tets_g5766.t1.vcf.gz > a_consensus_tets_g5766.t1_sequence.fasta``

```shellgrep -v ">" a_consensus_tets_g5766.t1_sequence.fasta > b_consensus_tets_g5766.t1_sequence.fasta
tr -d '\n' < b_consensus_tets_g5766.t1_sequence.fasta > consensus_tets_g5766.t1_sequence.fasta
```

Warning: ignoring overlapping variant starting at Cexcelsa_scaf_4:27477243
The site Cexcelsa_scaf_4:27477719 overlaps with another variant, skipping...



**g44492.t1**
n.b. the g44492.t1 gene is read backwards
create a filtered gff3 file for the g44492.t1 gene

```grep "g44492.t1" C_excelsa_V5_braker2_wRseq.gff3 | grep "CDS" > g44492.t1.gff3```

**create an output file of the diploid g44492.t1 gene**

```bcftools view -r Cexcelsa_scaf_2:28893505-28893797,Cexcelsa_scaf_2:28894013-28894486,Cexcelsa_scaf_2:28894742-28895015 UK_scan_dips.vcf.gz -i 'AF>0.5' -m 1 --types snps -o dips_g44492.t1.vcf```

**zip the output file and index**

```bgzip dips_g44492.t1.vcf```

```tabix -p vcf dips_g44492.t1.vcf.gz```

**filter the reference file for just the g44492.t1 gene**

```shellsamtools faidx C_excelsa_V5.fasta Cexcelsa_scaf_2:28893505-28893797 \
Cexcelsa_scaf_2:28894013-28894486 \
Cexcelsa_scaf_2:28894742-28895015 > reference_g44492.fasta
```

**create a consensus sequence for the diploid g44492.t1 gene**

```bcftools consensus -s - -f ../Reference_Genome/reference_g44492.fasta dips_g44492.t1.vcf.gz > a_consensus_dips_g44492.t1_sequence.fasta```

```shellgrep -v ">" a_consensus_dips_g44492.t1_sequence.fasta > b_consensus_dips_g44492.t1_sequence.fasta
tr -d '\n' < b_consensus_dips_g44492.t1_sequence.fasta > consensus_dips_g44492.t1_sequence.fasta
```

applied 0 variants



**repeat for tetraploid**

```bcftools view -r Cexcelsa_scaf_2:28893505-28893797,Cexcelsa_scaf_2:28894013-28894486,Cexcelsa_scaf_2:28894742-28895015 UK_scan_tets.vcf.gz -i 'AF>0.5' -m 1 --types snps -o tets_g44492.t1.vcf```

```bgzip tets_g44492.t1.vcf```

```tabix -p vcf tets_g44492.t1.vcf.gz```

```bcftools consensus -s - -f ../Reference_Genome/reference_g44492.t1.fasta tets_g44492.t1.vcf.gz > a_consensus_tets_g44492.t1_sequence.fasta```

```grep -v ">" a_consensus_tets_g44492.t1_sequence.fasta > b_consensus_tets_g44492.t1_sequence.fasta
tr -d '\n' < b_consensus_tets_g44492.t1_sequence.fasta > consensus_tets_g44492.t1_sequence.fasta
```

applied 0 variants

g44492.t1 gene has 0 variants, so we decide to use the whole g44492.t1 gene not only "CDS"

# View in protein structure prediction tools

**Search the PDB for homologues with crystal structures (and published papers!)**

The PDB (https://www.rcsb.org/) has a sequence search option that you can use to look for experimentally determined structures. Search the database with the wild type sequence and try to identify the crystal structure of a close homologue in *Sulfolobus solfataricus*. Papers linked to crystal structures are a rich source of specific information as they often accompany experiments that determine the function of specific residues and domains within the protein. The figures in the *Sulfolobus solfataricus* paper should contain all the information you need for this task.

**View in Swiss Model**

![](https://github.com/anyur66/Project2-Group4/blob/main/directory/WechatIMG64.jpg)

![](https://github.com/anyur66/Project2-Group4/blob/main/directory/WechatIMG65.jpg)

![](https://github.com/anyur66/Project2-Group4/blob/main/directory/WechatIMG66.jpg)



**View in Alpha Fold**

![](https://github.com/anyur66/Project2-Group4/blob/main/directory/671714042337_.pic.jpg)

![](https://github.com/anyur66/Project2-Group4/blob/main/directory/701714042514_.pic.jpg)

paste sequence(s) and run






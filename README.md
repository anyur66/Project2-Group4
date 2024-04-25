# Installation

1. IGV

2. EXPASY

3. Swiss Model

4. Alpha Fold

5. Install bcftools

   ```brew brew install bcftools```

6. Install samtools

   ```shellbrew install samtools```

# Data

1. Reference_genome

   C_excelsa_V5_braker2_wRseq.gff3

   C_excelsa_V5.dict

   C_excelsa_V5.fasta

   C_excelsa_V5.fasta.fai

2. VCFs

   UK_scan_dips.vcf

   UK_scan_dips.vcf.idx

   UK_scan_tets.vcf

   UK_scan_tets.vcf.idx

# Commands

#zip the diploids vcf: UK_scan_dips.vcf.gz
```bgzip UK_scan_dips.vcf```
#index the compressed file
```tabix -p vcf UK_scan_dips.vcf.gz```

#zip the tetraploids: UK_scan_tets.vcf.gz
```bgzip UK_scan_tets.vcf```
#index it
```tabix -p vcf UK_scan_tets.vcf.gz```

##########################################g5766.t1#############################################
#n.b. the g5766.t1 gene is read backwards
#create a filtered gff3 file for the g5766.t1 gene
```grep "g5766.t1" C_excelsa_V5_braker2_wRseq.gff3 | grep "CDS" > g5766.t1.gff3```

#create an output file of the diploid g5766.t1 gene
```bcftools view -r Cexcelsa_scaf_4:27471771-27471836,Cexcelsa_scaf_4:27471978-27472061,Cexcelsa_scaf_4:27472160-27472288,Cexcelsa_scaf_4:27472382-27472450,Cexcelsa_scaf_4:27472531-27472611,Cexcelsa_scaf_4:27472700-27472804,Cexcelsa_scaf_4:27472891-27473121,Cexcelsa_scaf_4:27473210-27474400,Cexcelsa_scaf_4:27474735-27475839,Cexcelsa_scaf_4:27475929-27476050,Cexcelsa_scaf_4:27476143-27476292,Cexcelsa_scaf_4:27476446-27476607,Cexcelsa_scaf_4:27476687-27476845,Cexcelsa_scaf_4:27476939-27477055,Cexcelsa_scaf_4:27477147-27477257,Cexcelsa_scaf_4:27477347-27477979 UK_scan_dips.vcf.gz -i 'AF>0.5' -m 1 --types snps -o dips_g5766.t1.vcf```

#zip the output file and index:
```bgzip dips_g5766.t1.vcf```
```tabix -p vcf dips_g5766.t1.vcf.gz```

#filter the reference file for just the g5766.t1 gene
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
Cexcelsa_scaf_4:27477347-27477979 > reference_g5766.fasta```
```

#create a consensus sequence for the diploid g5766.t1 gene
```bcftools consensus -s - -f ../Reference_Genome/reference_g5766.fasta dips_g5766.t1.vcf.gz > unfiltered_consensus_dips_g5766.t1_sequence.fasta```

```shellgrep -v ">" unfiltered_consensus_dips_g5766.t1_sequence.fasta > semi_filtered_consensus_dips_g5766.t1_sequence.fasta
tr -d '\n' < semi_filtered_consensus_dips_g5766.t1_sequence.fasta > consensus_dips_g5766.t1_sequence.fasta```
```

#Warning: ignoring overlapping variant starting at Cexcelsa_scaf_4:27477243
#The site Cexcelsa_scaf_4:27477719 overlaps with another variant, skipping...

#########################
##repeat for tetraploid##
#########################

```bcftools view -r Cexcelsa_scaf_4:27471771-27471836,Cexcelsa_scaf_4:27471978-27472061,Cexcelsa_scaf_4:27472160-27472288,Cexcelsa_scaf_4:27472382-27472450,Cexcelsa_scaf_4:27472531-27472611,Cexcelsa_scaf_4:27472700-27472804,Cexcelsa_scaf_4:27472891-27473121,Cexcelsa_scaf_4:27473210-27474400,Cexcelsa_scaf_4:27474735-27475839,Cexcelsa_scaf_4:27475929-27476050,Cexcelsa_scaf_4:27476143-27476292,Cexcelsa_scaf_4:27476446-27476607,Cexcelsa_scaf_4:27476687-27476845,Cexcelsa_scaf_4:27476939-27477055,Cexcelsa_scaf_4:27477147-27477257,Cexcelsa_scaf_4:27477347-27477979 UK_scan_tets.vcf.gz -i 'AF>0.5' -m 1 --types snps -o tets_g5766.t1.vcf```

```bgzip tets_g5766.t1.vcf```
```tabix -p vcf tets_g5766.t1.vcf.gz```

```bcftools consensus -s - -f ../Reference_Genome/reference_g5766.fasta tets_g5766.t1.vcf.gz > a_consensus_tets_g5766.t1_sequence.fasta```
```shellgrep -v ">" a_consensus_tets_g5766.t1_sequence.fasta > b_consensus_tets_g5766.t1_sequence.fasta
tr -d '\n' < b_consensus_tets_g5766.t1_sequence.fasta > consensus_tets_g5766.t1_sequence.fasta```
```

#Warning: ignoring overlapping variant starting at Cexcelsa_scaf_4:27477243
#The site Cexcelsa_scaf_4:27477719 overlaps with another variant, skipping...

###########################################g44492.t1###########################################
#n.b. the g44492.t1 gene is read backwards
#create a filtered gff3 file for the g44492.t1 gene
```grep "g44492.t1" C_excelsa_V5_braker2_wRseq.gff3 | grep "CDS" > g44492.t1.gff3```

#create an output file of the diploid g44492.t1 gene
```bcftools view -r Cexcelsa_scaf_2:28893505-28893797,Cexcelsa_scaf_2:28894013-28894486,Cexcelsa_scaf_2:28894742-28895015 UK_scan_dips.vcf.gz -i 'AF>0.5' -m 1 --types snps -o dips_g44492.t1.vcf```

#zip the output file and index:
```bgzip dips_g44492.t1.vcf```
```shelltabix -p vcf dips_g44492.t1.vcf.gz```

#filter the reference file for just the g44492.t1 gene
```shellsamtools faidx C_excelsa_V5.fasta Cexcelsa_scaf_2:28893505-28893797 \
Cexcelsa_scaf_2:28894013-28894486 \
Cexcelsa_scaf_2:28894742-28895015 > reference_g44492.fasta```
```

#create a consensus sequence for the diploid g44492.t1 gene
```bcftools consensus -s - -f ../Reference_Genome/reference_g44492.fasta dips_g44492.t1.vcf.gz > a_consensus_dips_g44492.t1_sequence.fasta```
```shellgrep -v ">" a_consensus_dips_g44492.t1_sequence.fasta > b_consensus_dips_g44492.t1_sequence.fasta
tr -d '\n' < b_consensus_dips_g44492.t1_sequence.fasta > consensus_dips_g44492.t1_sequence.fasta```
```

#applied 0 variants

#########################
##repeat for tetraploid##
#########################

```bcftools view -r Cexcelsa_scaf_2:28893505-28893797,Cexcelsa_scaf_2:28894013-28894486,Cexcelsa_scaf_2:28894742-28895015 UK_scan_tets.vcf.gz -i 'AF>0.5' -m 1 --types snps -o tets_g44492.t1.vcf```

```bgzip tets_g44492.t1.vcf```
```tabix -p vcf tets_g44492.t1.vcf.gz```

```bcftools consensus -s - -f ../Reference_Genome/reference_g44492.t1.fasta tets_g44492.t1.vcf.gz > a_consensus_tets_g44492.t1_sequence.fasta```

```grep -v ">" a_consensus_tets_g44492.t1_sequence.fasta > b_consensus_tets_g44492.t1_sequence.fasta
tr -d '\n' < b_consensus_tets_g44492.t1_sequence.fasta > consensus_tets_g44492.t1_sequence.fasta
```

#applied 0 variants

View in EXPASY###################################

#############################################################################Swiss Model#########################

###############################################################################Alpha Fold#####################

#diploid:

```Downloading alphafold2_ptm weights to .: 100%|██████████| 3.47G/3.47G [00:31<00:00, 120MB/s]
2024-02-26 13:18:02,679 Running on GPU
2024-02-26 13:18:02,847 Found 5 citations for tools or databases
2024-02-26 13:18:02,848 Query 1/1: dip_g5766_protein_dccf7 (length 1505)
PENDING:   0%|          | 0/150 [elapsed: 00:00 remaining: ?]2024-02-26 13:18:03,612 Sleeping for 6s. Reason: PENDING
RUNNING:   4%|▍         | 6/150 [elapsed: 00:07 remaining: 02:59]2024-02-26 13:18:10,349 Sleeping for 5s. Reason: RUNNING
RUNNING:   7%|▋         | 11/150 [elapsed: 00:13 remaining: 02:46]2024-02-26 13:18:16,152 Sleeping for 8s. Reason: RUNNING
RUNNING:  13%|█▎        | 19/150 [elapsed: 00:22 remaining: 02:29]2024-02-26 13:18:24,866 Sleeping for 8s. Reason: RUNNING
RUNNING:  18%|█▊        | 27/150 [elapsed: 00:30 remaining: 02:17]2024-02-26 13:18:33,612 Sleeping for 10s. Reason: RUNNING
RUNNING:  25%|██▍       | 37/150 [elapsed: 00:41 remaining: 02:04]2024-02-26 13:18:44,378 Sleeping for 9s. Reason: RUNNING
RUNNING:  31%|███       | 46/150 [elapsed: 00:51 remaining: 01:53]2024-02-26 13:18:54,084 Sleeping for 10s. Reason: RUNNING
RUNNING:  37%|███▋      | 56/150 [elapsed: 01:01 remaining: 01:42]2024-02-26 13:19:04,810 Sleeping for 8s. Reason: RUNNING
RUNNING:  43%|████▎     | 64/150 [elapsed: 01:10 remaining: 01:33]2024-02-26 13:19:13,497 Sleeping for 5s. Reason: RUNNING
RUNNING:  46%|████▌     | 69/150 [elapsed: 01:16 remaining: 01:28]2024-02-26 13:19:19,231 Sleeping for 7s. Reason: RUNNING
RUNNING:  51%|█████     | 76/150 [elapsed: 01:24 remaining: 01:21]2024-02-26 13:19:26,953 Sleeping for 7s. Reason: RUNNING
RUNNING:  55%|█████▌    | 83/150 [elapsed: 01:31 remaining: 01:13]2024-02-26 13:19:34,665 Sleeping for 10s. Reason: RUNNING
RUNNING:  62%|██████▏   | 93/150 [elapsed: 01:42 remaining: 01:02]2024-02-26 13:19:45,437 Sleeping for 8s. Reason: RUNNING
RUNNING:  67%|██████▋   | 101/150 [elapsed: 01:51 remaining: 00:53]2024-02-26 13:19:54,135 Sleeping for 7s. Reason: RUNNING
RUNNING:  72%|███████▏  | 108/150 [elapsed: 01:58 remaining: 00:45]2024-02-26 13:20:01,845 Sleeping for 8s. Reason: RUNNING
RUNNING:  77%|███████▋  | 116/150 [elapsed: 02:07 remaining: 00:37]2024-02-26 13:20:10,563 Sleeping for 10s. Reason: RUNNING
RUNNING:  84%|████████▍ | 126/150 [elapsed: 02:18 remaining: 00:26]2024-02-26 13:20:21,280 Sleeping for 7s. Reason: RUNNING
RUNNING:  89%|████████▊ | 133/150 [elapsed: 02:26 remaining: 00:18]2024-02-26 13:20:28,997 Sleeping for 6s. Reason: RUNNING
COMPLETE: 100%|██████████| 150/150 [elapsed: 02:34 remaining: 00:00]
2024-02-26 13:20:39,489 Could not generate input features dip_g5766_protein_dccf7: Invalid character in the sequence: -
Traceback (most recent call last):
  File "/content/colabfold/batch.py", line 1486, in run
    = generate_input_feature(query_seqs_unique, query_seqs_cardinality, unpaired_msa, paired_msa,
  File "/content/colabfold/batch.py", line 1013, in generate_input_feature
    feature_dict = build_monomer_feature(
  File "/content/colabfold/batch.py", line 866, in build_monomer_feature
    **pipeline.make_sequence_features(
  File "/content/alphafold/data/pipeline.py", line 40, in make_sequence_features
    features['aatype'] = residue_constants.sequence_to_onehot(
  File "/content/alphafold/common/residue_constants.py", line 580, in sequence_to_onehot
    raise ValueError(f'Invalid character in the sequence: {aa_type}')
ValueError: Invalid character in the sequence: -
2024-02-26 13:20:39,522 Done
0```
```
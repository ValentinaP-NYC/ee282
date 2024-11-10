# Homework 3 for EE282 

Author: Valentina Peña

## Calculate summaries of the genome

### Check file integrity
```bash
cd ~/myrepos/ee282/data/raw
wget "https://ftp.flybase.net/releases/FB2022_05/dmel_r6.48/fasta/dmel-all-chromosome-r6.48.fasta.gz"

# The output is be1f79c5922a4bf08548a2a4553e4d5f which matches to the checksum on FlyBase
md5sum dmel-all-chromosome-r6.48.fasta.gz
```

```bash
# Calculate summariez of the genome
faSize -tab dmel-all-chromosome-r6.48.fasta.gz 
```
awk '{print $3}' '<file path>' | sort | unique
awk ' NR > 1' {print $3}' '<filepath>' | sort |	unique 


## Summarize annotation file

### Check file integrity
```bash
wget "https://ftp.flybase.net/releases/FB2022_05/dmel_r6.48/gtf/dmel-all-r6.48.gtf.gz"

# The output is e13fa4e210bb47378ac2797cf78d69b3 which matches to the checksum on FlyBase
md5sum dmel-all-r6.60.gtf.gz 
```

### Total number of features of each type, sorted from the most common to the least common. 
```bash
gunzip -c dmel-all-r6.48.gtf.gz | awk '$0 !~ /^#/ {print $3}' | sort -rnk1 | uniq -c | sort -rnk1
```

### Total number of genes per chromosome arm (X, Y, 2L, 2R, 3L, 3R, 4) 
```bash
gunzip -c dmel-all-r6.48.gtf.gz | \
awk -F "\t" ' \
    $3 == "gene" && ($1 == "2L" || $1 == "2R" || $1 == "3L" || $1 == "3R" || $1 == "4" || $1 == "X" || $1 == "Y") { \
        chr_count[$1]++ \
    } \
    END { \
        for (chr in chr_count) print chr, chr_count[chr] \
    }' | \
tee chromosome_gene_counts.txt | \
sort -k2,2nr
```

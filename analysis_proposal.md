# Analysis Proposal for EE282 Final Project

Author: Valentina Peña

### Introduction

The distinct isoforms expressed in vertebrate genes through alternative splicing can be important in an ecological context. For example, alternative isoforms can lead to phenotypic changes amongst populations. In humans, the vast majority of genes are alternatively spliced, and most alternative exons show tissue-specific regulation (Wang et al., 2008). With advancements in sequencing technology and computational methods, we can conduct exploratory analyses to detect isoform enrichment and abundance. This project aims to quantify the levels of alternatively spliced genes and differentially regulated isoforms across two species of fish that underwent diet-switching experiments. 

All datasets used in this study are publicly available. The raw RNA-seq data were downloaded from the Sequence Read Archive under BioProject ID PRJNA738880 (from Herrera et al., 2022), which includes wild-caught and lab-fed individuals of four closely related prickleback species (family Stichaeidae). For the analysis conducted here, I have downloaded a subset of the available data, specifically focusing on liver tissue for two species, Xiphister mucosus and Phytichthys chirus. Using sra-tools v3.0.0, each sample was identified by its unique SRR identifier and retrieved from the SRA database with the prefetch tool. The downloaded .sra files were split and converted to .fastq format using fasterq-dump, which organized paired-end reads. After extraction, the .fastq files were compressed to .fastq.gz format. The entire process was logged into standard output and standard error log files. 

Raw data sequencing quality will be assessed using the short-read sequence inspection tool MultiQC. This tool creates a single HTML report with interactive plots across many samples. Following inspection of the report, low-quality base calls will be trimmed, and adapter contamination will be removed using Trim Galore. It includes the utilities Cutadapt adapter trimming tool, FastQC for quality control, and additional filtering by read length. Once basic cleaning has been completed, in silico normalization of the reads will be performed to low systemic coverage before assembly. This will help reduce the read volume and improve assembly performance.

Following the approach of Herrera et al. (2022), transcriptome assembly will be performed using Trinity v2.15.2. One wild-caught replicate from each species will serve as the reference assembly for that species. The quality of the de novo transcriptome assemblies will be assessed with regard to sequence length, fragmentation, and read support. A two-step approach will be followed for read alignment and transcript abundance estimation. First, reads will be aligned to the assembled contigs using the general-purpose aligner Bowtie2. Then, RSEM (RNA-seq by Expectation-Maximization) will be implemented to obtain abundance estimates. This two-step approach supports the intended analysis of splicing patterns (isoform abundance and diversity) because RSEM estimates gene and isoform expression levels by clustering transcripts closely that represent splicing isoform (Li and Dewy, 2011). Quality control of biological samples will be assessed using the Trinity script 'PtR' (Perl-to-R), which will be used to generate PCA plots and Pearson correlation heat maps for replicates. Functional annotation of the assemblies will be conducted using the Trinotate suite, which implements tools for homology search (BLAST+) and sequence feature annotation (HMMER3 against Pfam).

To detect alternative splicing using RNA-seq data, MISO (Mixture of Isoforms) will quantify differential splicing of alternative exons (Katz et al., 2010). It can process both single-end and paired-end reads. MISO operates in single-sample or multi-sample modes, enabling batch processing across multiple samples. To compare isoform expression usage within, I will use the script compare_miso.py, to quantify differences between wild and lab-diet and conditions. For data visualization, I will apply the sashimi_plot utility and produce Sashimi plots for analyses of isoform expression. Specifically, the plots show RNA-Seq densities along exons and junctions for samples while simultaneously depicting the gene model/isoform with reads that map in the dataset. Since these plots are produced per gene basis, a select few cases will be included in the final report. 

Overall, the proposed workflow encompasses the traditional pre-processing and de novo assembly for the transcriptomes of two fish species: Xiphister mucous and Anoplarchus purpurescens. With project feasibility in mind, I will instead focus on my efforts on alternative exon expression rather than a broader differential gene expression analysis (e.g., EdgeR, DESeq) that was previously conducted in the original study (Herrera et al., 2022).


### References

Herrera, M. J., Heras, J., & German, D. P. 2022. Comparative transcriptomics reveal tissue level 
specialization towards diet in prickleback fishes. Journal of Comparative Physiology B, 192(2), 275-295. doi.org/10.1007/s00360-021-01426-1.

Li, B., & Dewey, C. N. (2011). RSEM: Accurate transcript quantification from RNA-Seq data with 
or without a reference genome. BMC Bioinformatics, 12(1), 323. doi:10.1186/1471-2105-12-323

Katz, Y., Wang, E. T., Airoldi, E. M., & Burge, C. B. (2010). Analysis and design of RNA 
sequencing experiments for identifying isoform regulation. Nature methods, 7(12), 1009-1015.

Wang, E. T., Sandberg, R., Luo, S., Khrebtukova, I., Zhang, L., Mayr, C., ... & Burge, C. B. 
(2008). Alternative isoform regulation in human tissue transcriptomes. Nature, 
456(7221), 470-476.


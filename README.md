# RNA-seq Analysis Pipeline

This repository contains a reproducible RNA-seq analysis workflow designed to run on HPC systems using SLURM. The pipeline processes raw RNA sequencing data and produces gene-level count matrices suitable for downstream differential expression analysis using Gelato.

## Pipeline Overview

The workflow includes the following main steps:

1. Quality control using **FastQC** and **MultiQC**
2. Read trimming using **Cutadapt**
3. Genome index generation using **STAR**
4. RNA-seq read alignment using **STAR**
5. Extraction of alignment statistics
6. Gene-level counting using **HTSeq**
7. Aggregation of gene counts into a count matrix

## Expected Outputs

The pipeline produces several outputs including:

- **Quality control reports** for raw and trimmed reads  
- **Aligned BAM files** containing mapped reads  
- **Gene count files** generated with HTSeq  
- **Aggregated count matrix** ready for downstream analysis  

## Requirements

This pipeline is designed to run on an HPC system using SLURM and requires:

- FastQC  
- MultiQC  
- Cutadapt  
- STAR  
- HTSeq  
- Python 3  
- Conda or Mamba  

## Repository Structure

01_quality_control.md – FastQC and MultiQC tutorial  
02_trimming.md – Adapter trimming with Cutadapt  
03_star_installation.md – Installing STAR with Conda  
04_star_index_creation.md – Building the STAR genome index  
05_star_alignment.md – RNA-seq read alignment using STAR  
06_extract_star_alignment_results.md – Extract alignment statistics  
07_HTSeq_installation_and_script.md – Gene counting with HTSeq  
08_generate_count_matrix_from_htseq.md – Generate count matrix for downstream analysis
README.md - Overview of the RNA-seq pipeline 
RNA-seq-analysis-scripts.docx - Complete tutorial describing the full RNA-seq workflow and scripts.

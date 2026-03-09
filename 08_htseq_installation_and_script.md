## Gene-Level Counting with HTSeq

This script performs **gene-level counting of RNA-seq reads using HTSeq**.

It takes **BAM files** as input, along with a **GTF annotation file**, and produces counts of reads mapping to each gene.

This is a crucial step in RNA-seq data analysis, enabling downstream analyses such as **differential expression analysis**.

---

# Steps to Follow

## 1. Install HTSeq

Ensure **HTSeq** is installed on your system.

If it is not available as a module, you can install it using **Conda**:

```bash
conda create -n htseq_env -c bioconda htseq
conda activate htseq_env
```

---

## 2. Prepare Required Directories

### HTSeq Output Directory

Create a directory to store HTSeq output files.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/htseq
```

### Reference Directory

Ensure the reference **GTF file** is located in the specified directory.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/reference
```

### BAM Files Directory

Ensure the BAM files and their **locations file** are in the correct directory.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/BAM_files
```

---

## 3. Adjust the Script

Replace the placeholders:

- `<your_student_number>`
- `<your_email>`

with your **actual student number and email address**.

---

# Script: `htseq_count.sh`

```bash
#!/bin/bash
#SBATCH --job-name=htseq_count
#SBATCH --output="/mnt/scratch2/users/<student_number>/Your_analysis_directory/htseq/%A_%a_htseq.log.txt"
#SBATCH --mail-user=<your_email>
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --partition=k2-hipri

#SBATCH --array=0-149
# Adjust this range based on the number of BAM files listed in bamlocations.txt.
# Example: if you have 150 BAM files, use 0-149. If you have 20 BAM files, use 0-19.

#SBATCH --mem-per-cpu=32G

# Load the HTSeq module
module load apps/htseq/0.10.0/gcc-4.8.5+python-2.7.8+setuptools-24.0.1+numpy-1.11.3+matplotlib-1.5.1

# Define base directories
base_dir="/mnt/scratch2/users/<student_number>/Your_analysis_directory"
gtf="$base_dir/reference/Homo_sapiens.GRCh38.110.chr_patch_hapl_scaff.gtf"
bam_list="$base_dir/star_alignment/bamlocations.txt"
output_dir="$base_dir/htseq"

# Create output directory if it does not exist
mkdir -p "$output_dir"

# Read BAM file paths from the input list
readarray -t bamfiles < "$bam_list"

# Select the BAM file corresponding to the SLURM array task ID
bamfile="${bamfiles[$SLURM_ARRAY_TASK_ID]}"
fname=$(basename -- "$bamfile")

# Run HTSeq
htseq-count \
    -s yes \
    -r pos \
    -t gene \
    -i gene_id \
    -f bam \
    "$bamfile" "$gtf" > "$output_dir/${fname%.bam}.htseq"
```

---

# Important Notes

- **Job Name:** Change `htseq_count` to a meaningful name for your job.
- **Output Path:** Adjust the `--output` path to your own directory structure.
- **Email Notification:** Replace `your_email@domain.com` with your actual email address.
- **SLURM Array Size:** Adjust the `--array` size based on the number of BAM files you have.
- **Directory Paths:** Ensure the paths to the reference **GTF file**, **BAM files**, and **output directories** are correct.

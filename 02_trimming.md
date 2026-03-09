## 2. Trimming

### Purpose

To remove adapter sequences and low-quality bases from the raw RNA-seq reads. This ensures that only high-quality, adapter-free reads are used for the next steps, improving the accuracy of the alignment and quantification.

---

## Prepare the Input File List for Trimming

Before running the trimming script, create a text file containing the full paths to all **R1 raw FASTQ files**. Each path should be written on a separate line.

This file will be used by the trimming script to process each sample.

### Command

```bash
find /mnt/scratch2/users/<student_number>/raw_files -name "*_R1_001.fastq.gz" | sort > /mnt/scratch2/users/<student_number>/raw_files/filelocations.txt
```

### Verify the file

```bash
cat /mnt/scratch2/users/<student_number>/raw_files/filelocations.txt
```

Only **R1 files** should be listed, because the script automatically identifies the corresponding **R2 files**.

---

# Steps

### 1. Create Directories

#### Output Folder

Create a directory for the output of the quality check.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/output
```

#### FastQC Results Folder

Create a folder to store FastQC results.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/fastqc
```

---

### 2. Submit the Quality Check Script (checkQuality.sh)

Adapt the script to your Kelvin environment. Change the job name, output directory, and mail user to your details.

Submit the script using the SLURM job scheduler.

```bash
sbatch checkQuality.sh
```

---

# Example Script: trimRNAseq.sh

```bash
#!/bin/bash
#SBATCH --job-name=trim_rnaseq
#SBATCH --output="/mnt/scratch2/users/<student_number>/Your_analysis_directory/logs/%A_%a_cutadapt.log.txt"
#SBATCH --mail-user=<your_email>
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --partition=k2-medpri

#SBATCH --array=0-149
# Adjust this range based on the number of samples listed in filelocations.txt.
# Example: if you have 150 samples, use 0-149. If you have 20 samples, use 0-19.

#SBATCH --cpus-per-task=10
#SBATCH --mem-per-cpu=10G

# Load required software
module load mamba/1.3.1
conda activate preprocessing

# Define base directories
base_dir="/mnt/scratch2/users/<student_number>/Your_analysis_directory"
raw_dir="$base_dir/raw_files"
output_dir="$base_dir/adapted"
log_dir="$base_dir/logs"

# Create directories if they do not exist
mkdir -p "$output_dir"
mkdir -p "$log_dir"

# Read list of input R1 files
input_files=($(<"$raw_dir/filelocations.txt"))

# Select the file corresponding to the SLURM array task ID
input_R1="${input_files[$SLURM_ARRAY_TASK_ID]}"

# Identify the corresponding R2 file
input_R2="${input_R1%R1_001.fastq.gz}R2_001.fastq.gz"

# Extract sample name
sample_name=$(basename "$input_R1")
sample_name="${sample_name%_R1_001.fastq.gz}"

# Define output files
output_R1="$output_dir/trimmed_${sample_name}_R1.fastq.gz"
output_R2="$output_dir/trimmed_${sample_name}_R2.fastq.gz"

# Run Cutadapt
cutadapt \
    -q 28 \
    -j 10 \
    -m 30 \
    --trim-n \
    -a AGATCGGAAGAGCACACGTCTGAACTCCAGTCA \
    -A AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT \
    -o "$output_R1" \
    -p "$output_R2" \
    "$input_R1" "$input_R2"
```

---

# Important Notes

- **Job Name:** Change `trim_rnaseq` to a meaningful name for your job.
- **Output Path:** Adjust the `--output` path to your own directory structure.
- **Email Notification:** Replace `your_email@domain.com` with your actual email address.
- **SLURM Array Size:** Adjust the `--array` size based on the number of input files you have.
- **Directory Paths:** Ensure the paths to the raw files and output directory are correct.
- **Adapter Sequences:** Replace the adapter sequences `AGATCGGAAGAGCACACGTCTGAACTCCAGTCA` and `AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT` with the specific adapter sequences used in your sequencing data.
- **Directory Names:** Change `Your_analysis_directory` to the name of your project or directory.

---

# Transfer Files for Visualization

To transfer the files:

Use `scp` to copy the files from Kelvin to your local computer for visualization and sharing with your professor.

### Command

```bash
scp <your_student_number>@kelvin:/mnt/scratch2/users/<your_student_number>/fastqc /path/to/local/directory
```

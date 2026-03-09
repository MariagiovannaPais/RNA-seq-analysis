## 5. STAR Alignment Script

This script is designed to perform RNA-seq read alignment using the **STAR (Spliced Transcripts Alignment to a Reference)** aligner. It takes paired-end RNA-seq reads and aligns them to a specified reference genome.

The resulting output includes **sorted BAM files**, which are essential for downstream analysis such as:

- quantification of gene expression  
- identification of novel transcripts  
- differential expression analysis  

---

# Prepare the Input File List for STAR Alignment

Before running the STAR alignment script, create a text file containing the paths to all **trimmed R1 FASTQ files** produced during the trimming step.

Each file path should be written on a separate line.

This file will be used by the STAR script to identify the input reads for alignment.

### Command

```bash
find /mnt/scratch2/users/<student_number>/Your_analysis_directory/adapted -name "*_R1.fastq.gz" | sort > /mnt/scratch2/users/<student_number>/Your_analysis_directory/adapted/adaptedlocations.txt
```

### Verify the file

```bash
cat /mnt/scratch2/users/<student_number>/Your_analysis_directory/adapted/adaptedlocations.txt
```

Only **R1 files** should be listed in `adaptedlocations.txt`.

The STAR alignment script automatically identifies the corresponding **R2 files** using the same filename prefix.

---

# Example Script: `starAlignment.sh`

```bash
#!/bin/bash
#SBATCH --job-name=star_alignment
#SBATCH --output="/mnt/scratch2/users/<student_number>/Your_analysis_directory/logs/%A_%a_star.log.txt"
#SBATCH --mail-user=<your_email>
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --partition=k2-hipri

#SBATCH --array=0-149
# Adjust this range based on the number of input R1 files listed in adaptedlocations.txt.
# Example: if you have 150 samples, use 0-149. If you have 20 samples, use 0-19.

#SBATCH --cpus-per-task=10
#SBATCH --mem-per-cpu=20G

# Load STAR module
module load apps/star/2.7.3a/gcc-4.8.5

# Define base directories
base_dir="/mnt/scratch2/users/<student_number>/Your_analysis_directory"
genomeDir="$base_dir/reference/star_index"

# Text file containing the full paths to all trimmed R1 FASTQ files
input_list="$base_dir/adapted/adaptedlocations.txt"

output_dir="$base_dir/star_alignment"
log_dir="$base_dir/logs"

# Create directories if they do not exist
mkdir -p "$output_dir"
mkdir -p "$log_dir"

# Read file paths from the input list
readarray -t inputFiles < "$input_list"

# Select current paired-end read files based on SLURM array task ID
inputFileR1="${inputFiles[$SLURM_ARRAY_TASK_ID]}"
inputFileR2="${inputFileR1/_R1.fastq.gz/_R2.fastq.gz}"

# Define output prefix
sample_name=$(basename "${inputFileR1%_R1.fastq.gz}")
outputPrefix="$output_dir/${sample_name}_"

# Run STAR alignment
STAR \
    --genomeDir "$genomeDir" \
    --runThreadN 10 \
    --readFilesIn "$inputFileR1" "$inputFileR2" \
    --readFilesCommand gunzip -c \
    --outFileNamePrefix "$outputPrefix" \
    --outSAMtype BAM SortedByCoordinate \
    --outSAMunmapped Within \
    --outSAMattributes Standard
```

---

# Important Notes

- **Job Name:** Change `star_alignment` to a meaningful name for your job.
- **Output Path:** Adjust the `--output` path to your own directory structure.
- **Email Notification:** Replace `your_email@domain.com` with your actual email address.
- **SLURM Array Size:** Adjust the `--array` size based on the number of input files you have.
- **Directory Paths:** Ensure the paths to the reference genome, input files, and output directories are correct.

---

# Steps to Follow

### 1. Create Required Directories

#### Reference Directory

Ensure the reference genome directory exists.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/reference/hg38
```

#### Adapted Files Directory

Ensure the directory for adapted files exists.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/Adapted
```

#### STAR Output Directory

Create a directory to store the STAR alignment output files.

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/Your_analysis_directory/STAR
```

---

### 2. Prepare the Input File List

Ensure the file **adaptedlocations.txt** contains the paths to all the adapted **R1 fastq files**.

Each path should be on a new line.

---

### 3. Submit the SLURM Script

Save the script as:

```
starAlignment.sh
```

Then submit it using the SLURM job scheduler:

```bash
sbatch starAlignment.sh
```

---

By following these steps and adjusting the script accordingly, you will be able to run the **STAR alignment for your RNA-seq data effectively**.

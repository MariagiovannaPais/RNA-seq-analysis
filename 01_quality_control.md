# RNA-seq Analyses

## 1. Quality Control with FastQC and MultiQC

Quality control is an essential step both before and after trimming your RNA-seq data to ensure you are working with high-quality data throughout your analysis.

### Before Trimming

#### Purpose

To assess the initial quality of your raw sequencing data and identify any issues that need to be addressed during trimming.

#### Steps

1. **Transfer Data to Kelvin**
   - Transfer your data from the Grande to your Kelvin account.
   - Command (run this on Grande):

```bash
rsync -r /mnt/hdd/data/Your_fastq_files/fastq <your_student_number>@dm1.kelvin.alces.network:/mnt/scratch2/users/<your_student_number>
```

2. **Create Directories in Kelvin**
   - Output Folder:
     - Create a directory for the output of the quality check.
     - Command:

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/output
```

   - FastQC Results Folder:
     - Create a folder to store FastQC results.
     - Command:

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/fastqc
```

   - Analysis Folder:
     - Create a directory (folder) for your analysis.
     - Command:

```bash
mkdir -p /mnt/scratch2/users/<your_student_number>/analysis_folder
```

3. **Log in to Kelvin**
   - Use your credentials to access Kelvin.

4. **Navigate to Your Directory**
   - Change to your working directory where you will perform the analysis.
   - Command:

```bash
cd /mnt/scratch2/users/<your_student_number>
```

5. **Load Mamba (a package manager)**
   - Load the mamba module, which helps manage software packages.
   - Command:

```bash
module load mamba/1.3.1
```

6. **Create and Activate a Conda Environment**
   - Create a new environment to install and manage the software needed for pre-processing (if not already done).
   - Commands:

```bash
conda create -n preprocessing
conda activate preprocessing
```

7. **Install Required Software**
   - Ensure you have fastqc and multiqc installed. If not, install them using conda (if not already done).
   - Commands:

```bash
conda install -c bioconda fastqc
conda install -c bioconda multiqc
```

8. **Transfer Data to Kelvin (if needed)**
   - Command (run this on Grande):

```bash
rsync -r /mnt/hdd/data/Your_fastq_files/fastq <your_student_number>@dm1.kelvin.alces.network:/mnt/scratch2/users/<your_student_number>
```

9. **Verify Data in Kelvin**
   - After transferring, check that your files are in the correct directory.
   - Command:

```bash
cd /mnt/scratch2/users/<your_student_number>
```

10. **Run FastQC to Check Quality**
    - Use FastQC to analyze the quality of your raw fastq files.
    - Command:

```bash
fastqc -t 15 /mnt/scratch2/users/<your_student_number>/raw_files/* -o /mnt/scratch2/users/<your_student_number>/output
```

11. **Run MultiQC to Combine Reports**
    - Command:

```bash
multiqc /mnt/scratch2/users/<your_student_number>/output -o /mnt/scratch2/users/<your_student_number>/output/multiqc_raw
```

#### Outputs

- **FastQC Output:** This tool creates a detailed quality report for each raw fastq file. The output is in HTML format, so you can open it in a web browser to see the quality of your sequencing data.
- **MultiQC Output:** This tool combines the FastQC reports from all your samples into one single report. This combined HTML report makes it easy to compare the quality of all your samples at once.

### After Trimming

#### Purpose

To verify that the trimming process has successfully removed low-quality bases and adapter sequences, resulting in high-quality reads ready for alignment.

#### Steps

1. **Run FastQC on Trimmed Files**
   - Command:

```bash
fastqc -t 15 /mnt/scratch2/users/<your_student_number>/adapted/* -o /mnt/scratch2/users/<your_student_number>/output
```

2. **Run MultiQC to Combine Reports**
   - Command:

```bash
multiqc /mnt/scratch2/users/<your_student_number>/output -o /mnt/scratch2/users/<your_student_number>/output/multiqc_trimmed
```

#### Outputs

- **FastQC Output:** Generates quality reports for each trimmed fastq file, available in HTML format.
- **MultiQC Output:** Combines the FastQC reports from all trimmed samples into one report, also in HTML format.

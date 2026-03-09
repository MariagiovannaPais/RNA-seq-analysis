## Extracting STAR Alignment Results

This script extracts and compiles key metrics from **STAR alignment result files (`Log.final.out`)** into a structured CSV file.

It processes multiple log files and extracts important alignment statistics including:

- Total number of input reads
- Number of uniquely mapped reads
- Percentage of uniquely mapped reads
- Number of reads mapped to multiple loci
- Percentage of reads mapped to multiple loci

The results are then sorted and saved in a **CSV file for downstream analysis and quality assessment**.

---

## Script: `extract_star_alignment_results.py`

```python
import pandas as pd
import os
import glob

# Directory containing STAR Log.final.out files
log_files_dir = '/mnt/scratch2/users/<student_number>/Your_analysis_directory/star_alignment'

# Output CSV file
csv_file_path = '/mnt/scratch2/users/<student_number>/Your_analysis_directory/star_alignment/star_alignment_summary.csv'

# Prepare list to collect results
data = []

# Find all STAR log files
log_files = glob.glob(os.path.join(log_files_dir, '*Log.final.out'))

for log_file in log_files:
    with open(log_file, 'r') as file:

        sample_id = os.path.basename(log_file).replace('_Log.final.out', '')

        total_reads = ''
        uniquely_mapped = ''
        percentage_uniquely_mapped = ''
        multiply_mapped = ''
        percentage_multiply_mapped = ''

        for line in file:

            if 'Number of input reads' in line:
                total_reads = line.split('|')[1].strip()

            elif 'Uniquely mapped reads number' in line:
                uniquely_mapped = line.split('|')[1].strip()

            elif 'Uniquely mapped reads %' in line:
                percentage_uniquely_mapped = line.split('|')[1].strip()

            elif 'Number of reads mapped to multiple loci' in line:
                multiply_mapped = line.split('|')[1].strip()

            elif '% of reads mapped to multiple loci' in line:
                percentage_multiply_mapped = line.split('|')[1].strip()

        data.append([
            sample_id,
            total_reads,
            uniquely_mapped,
            percentage_uniquely_mapped,
            multiply_mapped,
            percentage_multiply_mapped
        ])

# Create DataFrame
df = pd.DataFrame(data, columns=[
    "Sample ID",
    "Total Reads",
    "Uniquely Mapped",
    "Percentage of Uniquely Mapped",
    "Multiply Mapped",
    "Percentage of Multiply Mapped"
])

# Sort samples alphabetically
df_sorted = df.sort_values(by="Sample ID")

# Save to CSV
df_sorted.to_csv(csv_file_path, index=False)
```

---

## Run the Script

Save the script as:

```
extract_star_alignment_results.py
```

Run it on Kelvin using:

```bash
python extract_star_alignment_results.py
```

---

## Output

The script generates a file named:

```
star_alignment_summary.csv
```

This file contains a table summarizing the alignment statistics for all samples.

---

## Important Notes

- **Log Files Directory:** Ensure that `log_files_dir` is set to the directory containing your `Log.final.out` files.
- **CSV Output Path:** Adjust `csv_file_path` to the desired location and name for the output CSV file.
- **Student Number Placeholder:** Replace `<student_number>` with your actual student number to correctly specify the file paths.

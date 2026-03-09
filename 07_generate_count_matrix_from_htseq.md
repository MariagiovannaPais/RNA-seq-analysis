## Aggregating HTSeq Counts

After running **HTSeq**, each sample produces a separate `.htseq` file containing the number of reads assigned to each gene.

To perform downstream analyses, such as **differential expression analysis**, these individual files must be combined into a **single count matrix**, where:

- rows correspond to **genes**
- columns correspond to **samples**

The following Python script aggregates all HTSeq output files into a single table.

In the resulting file:

- the **first column contains the Ensembl Gene IDs (GeneID)**
- each additional column represents the **read counts for a sample**

Save the script as:

```
aggregatedcount.py
```

and run it on **Kelvin**.

---

# Example Script: `aggregatedcount.py`

```python
import pandas as pd
import os
import glob

# Step 1: Aggregate counts from HTSeq output files
def aggregate_counts(htseq_dir, output_csv_path):

    data = []

    # Find all HTSeq output files
    htseq_files_pattern = os.path.join(htseq_dir, '*Aligned.sortedByCoord.out.htseq')
    htseq_files = glob.glob(htseq_files_pattern)

    for htseq_file in htseq_files:

        # Extract sample name from filename
        sample_name = os.path.basename(htseq_file).replace('Aligned.sortedByCoord.out.htseq', '')

        with open(htseq_file, 'r') as f:
            for line in f:

                gene_id, count = line.strip().split('\t')

                data.append([gene_id, sample_name, int(count)])

    # Create dataframe
    df = pd.DataFrame(data, columns=["GeneID", "Sample", "Count"])

    # Convert to wide format (genes x samples)
    df_wide = df.pivot(index="GeneID", columns="Sample", values="Count").fillna(0).astype(int)

    # Reset index so GeneID becomes a column
    df_wide.reset_index(inplace=True)
    df_wide.columns.name = None

    # Sort genes alphabetically
    df_wide = df_wide.sort_values(by="GeneID")

    # Save output
    df_wide.to_csv(output_csv_path, index=False)

    print(f"Aggregated counts saved to {output_csv_path}")


if __name__ == "__main__":

    # Directory containing HTSeq output files
    htseq_dir = "/mnt/scratch2/users/<student_number>/Your_analysis_directory/htseq"

    # Output count matrix
    aggregated_counts_csv = "/mnt/scratch2/users/<student_number>/Your_analysis_directory/aggregated_counts_wide.csv"

    aggregate_counts(htseq_dir, aggregated_counts_csv)
```

---

# Run the Script

Run the script on Kelvin using:

```bash
python aggregatedcount.py
```

---

# Output

The script generates the file:

```
aggregated_counts_wide.csv
```

This file contains the **gene count matrix** required for downstream **differential expression analysis**.

---

# Next Step: Differential Expression Analysis with GELATO

The generated count matrix (`aggregated_counts_wide.csv`) can now be used as input for **GELATO**, which performs differential expression analysis and visualization.

To proceed with the analysis, follow the **GELATO installation and usage instructions** available in the GELATO repository:

➡ https://github.com/Gelato-hub/gelato

The repository contains instructions for:

- installing **Docker**
- running **GELATO containers**
- performing **RNA-seq differential expression analysis**
- generating **visualizations and reports**

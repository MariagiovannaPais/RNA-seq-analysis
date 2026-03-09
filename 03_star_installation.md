## 3. Step to Install STAR

To install STAR on your system using Conda, follow these steps:

---

### 1. Install Conda (if not already installed)

If you do not have Conda installed, you can download and install **Miniconda** from the official Miniconda website.

---

### 2. Create a Conda environment for RNA-seq analysis and install STAR

Open a terminal and run the following commands:

```bash
# Create a new Conda environment named 'rnaseq_star'
conda create -n rnaseq_star -c bioconda star

# Activate the Conda environment
conda activate rnaseq_star
```

This will install the **STAR aligner** within the `rnaseq_star` environment.

---

### 3. Verify the installation of STAR

After activating the environment, verify that STAR is installed correctly by checking its version:

```bash
star --version
```

If the installation was successful, the command will return the installed **STAR version number**.

This confirms that STAR is installed and ready to use in your Conda environment.

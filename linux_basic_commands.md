## Linux for Beginners

The RNA-seq pipeline is executed using the Linux command line.  
If you are not familiar with Linux, the following basic commands will help you navigate directories and manage files.

---

### Show your current directory

```bash
pwd
```

Displays the current working directory.

---

### List files in a directory

```bash
ls
```

Lists files and folders in the current directory.

Useful options:

```bash
ls -l
```

Shows detailed file information.

```bash
ls -lh
```

Shows file sizes in a human-readable format.

```bash
ls -a
```

Shows hidden files.

---

### Change directory

```bash
cd directory_name
```

Move into a directory.

Example:

```bash
cd data
```

Move one level up:

```bash
cd ..
```

Return to the home directory:

```bash
cd ~
```

---

### Create a new directory

```bash
mkdir directory_name
```

Example:

```bash
mkdir results
```

---

### Remove an empty directory

```bash
rmdir directory_name
```

Example:

```bash
rmdir old_folder
```

This command only works if the directory is empty.

---

### Remove files

```bash
rm filename
```

Example:

```bash
rm temporary_file.txt
```

⚠️ This permanently deletes the file.

Remove a directory and its contents:

```bash
rm -r folder_name
```

---

### Copy files

```bash
cp source destination
```

Example:

```bash
cp sample.fastq.gz backup/
```

Copy an entire directory:

```bash
cp -r folder_name new_folder
```

---

### Move or rename files

```bash
mv old_name new_name
```

Example (rename a file):

```bash
mv sample1.fastq.gz sample_A.fastq.gz
```

Move a file to another directory:

```bash
mv sample.fastq.gz results/
```

---

### Create an empty file

```bash
touch filename
```

Example:

```bash
touch notes.txt
```

---

### View file contents

Show the first lines of a file:

```bash
head filename
```

Show the last lines:

```bash
tail filename
```

View a file interactively:

```bash
less filename
```

Press **q** to exit.

---

### Count lines, words, or characters in a file

```bash
wc filename
```

Example:

```bash
wc genes.txt
```

---

### Display file contents quickly

```bash
cat filename
```

Example:

```bash
cat README.md
```

---

### Search text inside files

```bash
grep "word" filename
```

Example:

```bash
grep "gene" results.txt
```

---

### Check disk space

```bash
df -h
```

Shows available disk space in a readable format.

---

### Check file or directory size

```bash
du -h
```

Example:

```bash
du -h results/
```

---

### Get help for a command

```bash
command_name --help
```

Example:

```bash
ls --help
```

Or open the manual page:

```bash
man ls
```

Press **q** to exit the manual.

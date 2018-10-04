

# FASTA files



The FASTA format is a common file format that is used for storing DNA, RNA or
Protein sequences. FASTA files begin with a header line starting with ‘>’ that
contains text information of the sequence and often an identifier such as the
genbank ID (NM_004985.3). The subsequent lines contain the DNA, RNA or
Protein sequence. FASTA files usually end with the \*.fa or \*.fasta extension.



Let’s get some transcriptomes!



Notice this file is compressed using tar and gzip.

What size is this file?

Let's decompress it

```
tar xvzf assemblies.tar.gz
```

What is the size now?

How many sequences are in each transcriptome?
```
grep -c ">" file.fasta
```
How many nucleotides are in each transcriptome?
```
grep -v ">" file.fasta| wc -c
```

How many contigs and isoforms?

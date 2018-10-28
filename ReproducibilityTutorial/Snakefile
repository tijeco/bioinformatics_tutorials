SAMPLES, = glob_wildcards("{sample}.fasta")

rule final:
    input:expand("{sample}.tree",sample=SAMPLES)

rule mafft:
    input:
        "{sample}.fasta"
    output:
        "{sample}.aln"
    conda:
        "envs/mafft.yaml"
    shell:
        "mafft --auto --phylipout {input} > {output}"

rule fasttree:
    input:
        "{sample}.aln"
    output:
        "{sample}.tree"
    conda:
        "envs/fasttree.yaml"
    shell:
        "fasttree  {input}  > {output}"

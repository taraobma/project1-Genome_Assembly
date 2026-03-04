# Genome Assembly and Evaluation Pipeline

## Project Overview
This project implements a reproducible Nextflow pipeline for bacterial genome assembly, polishing, annotation, and quality assessment using long- and short-read sequencing data.
The workflow integrates assembly, polishing, completeness analysis, and reference-based evaluation in a modular HPC-ready framework.


## Workflow Overview
Long reads (Nanopore) → Assembly (Flye) → Polishing (Pilon) → Annotation (Prokka) → Quality Assessment (QUAST, BUSCO).  
Short reads (Illumina) are used for alignment-based polishing.  
The pipeline is implemented in Nextflow with modular processes for each tool.


## Key Results
### Assembly Quality (QUAST – Polished)
- Genome fraction: 98.43%
- Total length: 4,627,920 bp
- Duplication ratio: 1.001
- Misassemblies: 19
- Mismatches per 100 kbp: 18.18
- GC content: 44.70%

Polishing reduced base-level mismatches compared to the unpolished assembly.

### Genome Completeness (BUSCO)
- C:99.6% [S:99.5%, D:0.1%], F:0.0%, M:0.4%, n:1828
- 99.6% complete conserved genes
- 0.4% missing
- No fragmented genes

Indicates a highly complete and structurally accurate assembly.

### Visualization 
#### Genome Annotation (Circos Plot)
![Circos Plot](./figures/circosplot.png)
#### BUSCO Completeness Plot
![BUSCO Completeness Plot](./figures/buscoplot.png)


## Tools Used
- Nextflow
- Flye
- Pilon
- Bowtie2
- Samtools
- Prokka
- QUAST
- BUSCO
- Python (pycirclize, matplotlib)

## Takeaway
This project demonstrates:
- End-to-end genome assembly workflow design
- Assembly polishing and evaluation
- Interpretation of structural and completeness metrics
- Reproducible pipeline development using Nextflow

## References

- **QUAST:** [http://quast.sourceforge.net/](http://quast.sourceforge.net/)
- **BUSCO:** [https://busco.ezlab.org/](https://busco.ezlab.org/)
- **Data source:** [NCBI SRA](https://www.ncbi.nlm.nih.gov/sra)
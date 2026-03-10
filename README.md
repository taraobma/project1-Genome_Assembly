# Genome Assembly and Evaluation Pipeline
## Overview
Implemented a modular Nextflow DSL2 genome assembly pipeline for hybrid bacterial genome assembly using long- and short-read sequencing data.

The workflow performs read QC, long-read filtering, de novo assembly, short-read polishing, assembly evaluation, and functional annotation.

## Biological Question
Can high-quality bacterial genome assemblies be generated using long-read assembly followed by short-read polishing, and how does polishing improve assembly completeness and accuracy?

## Dataset
Organism: Alteromonas macleodii   
Long reads: Oxford Nanopore    
Short reads: Illumina paired-end   
Reference genome: NCBI Refseq Assembly: GCF_000172635.2

## Workflow
<img src="./figures/full_pipeline.png" width="450" alt="Workflow DAG">

FILTLONGER → FLYE → BOWTIE2_INDEX → BOWTIE2_ALIGN (with FASTQC short reads) → SAMTOOLS_SORT → PILON → [BUSCO, PROKKA, QUAST] in parallel

Implemented using modular Nextflow processes.


## Usage

**1. Clone the repository**
```
git clone https://github.com/taraobma/project1-Genome_Assembly.git

cd project1-Genome_Assembly
```
**2. Configure inputs**   
Edit `nextflow.config` to set your samplesheet paths and parameters.

**3. Run the pipeline**
```
nextflow run main.nf -profile conda
```
**Requirements:** Nextflow ≥ 22.0, Conda


## Quality Control
- Long reads filtered using Filtlong (min length: 500 bp, top 90% by quality retained)
- Short reads assessed with FastQC; no adapter contamination or major GC bias detected
- Short-read alignment with Bowtie2 provided coverage for Pilon polishing
- Polished assembly GC content (44.70%) matches reference (44.71%), confirming no major bias



## Key Results
> Single-contig assembly (4,627,920 bp) |  BUSCO: 99.6% complete | Genome fraction: 98.429%

### Assembly Improvement
Polishing with Pilon:
- Mismatches reduced from 18.33 → 18.18 per 100 kbp after Pilon polishing
- Genome fraction: 98.429% of reference (4,627,920 bp assembled vs. 4,653,851 bp reference)
- GC content: 44.70% (reference: 44.71%) — near-identical, confirming assembly accuracy
- Duplication ratio: 1.001 — minimal redundancy
- 19 misassemblies identified in the single contig

The marginal improvement in mismatches suggests Flye produced a high-quality 
draft assembly, with Pilon refining residual small-scale errors.



### BUSCO Analysis
<img src="./figures/buscoplot.png" width="400" alt="BUSCO Plot">

- 99.6% complete (S: 99.5% single-copy, D: 0.1% duplicated), n=1828
- 0.0% fragmented, 0.4% missing


### QUAST Evaluation
Compared polished vs unpolished assemblies:
| Metric | Polished | Unpolished |
|---|---|---|
| Genome Fraction | 98.429% | 98.429% |
| Total Length | 4,627,920 bp | 4,627,920 bp |
| Mismatches per 100 kbp | 18.18 | 18.33 |
| Misassemblies | 19 | 19 |
| Duplication Ratio | 1.001 | 1.001 |
| GC Content | 44.70% | 44.70% |


### Annotation
<img src="./figures/circosplot.png" width="450" alt="Circos Plot">


Prokka annotated the single-contig assembly (4,627,920 bp), identifying forward/reverse 
CDS, rRNA operons, and tRNA genes distributed across the full chromosome.


## Technical Highlights
- Hybrid assembly (long + short reads)
- Automated polishing workflow
- Comparative evaluation (polished vs unpolished)
- Reference retrieval via NCBI CLI
- Quality visualization (BUSCO plots)
- Fully modular Nextflow DSL2 structure

## Repository Structure
```
├── main.nf                  # Main nextflow pipeline processes
├── nextflow.config          # Configuration file
├── bac_samples.csv          # Sample sheet
├── modules/                 # Modular process definitions
├── envs/                    # Per-process Conda environment YMLs
├── figures/                 # DAG, BUSCO plot, and Circos plot
├── results/                 # Pipeline outputs (not tracked)
└── .gitignore
```

## Tools Used
- FastQC
- Filtlong
- Flye
- Bowtie2
- Samtools
- Pilon
- BUSCO
- QUAST
- Prokka
- NCBI Datasets CLI
- Nextflow

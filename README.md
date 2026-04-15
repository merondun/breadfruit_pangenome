# Breadfruit Pangenome Graph Resource

## Table of contents
- [Background Info](#background-info)
- [Tutorials & resources](#tutorials--resources)
- [SciNet Overview](#scinet-overview)
- [Documentation & tools](#documentation--tools)
- [People](#people)
- [Metadata](#metadata)
- [Inputs & naming conventions](#inputs--naming-conventions)
- [Highest-priority goals/deliverables](#highest-priority-goals-deliverables)
- [Tentative outline (10-week plan)](#tentative-outline-10-week-plan)

---

The goal of this ~10 week project is to build and release a sequence-based pangenome graph reference for breadfruit using **14 collapsed haploid, chromosome-scale assemblies** (28 chromosomes each). The goal is to generate one pangenome graph per chromosome (Chr01–Chr28) using the reference-free pangenome graph builder **PGGB**.

The main deliverables are (i) one **pangenome graph per chromosome** (together forming a breadfruit pangenome reference), (ii) a **pangenome accumulation curve** showing how the total graph sequence covered grows as we add accessions (1...14; summed across chromosomes), and (iii) one hero-locus **tube map** highlighting a clear structural variant/alternative path in the graph.

Please note that this outline is intended as a tentative roadmap!

---

# Background Info

## Breadfruit context
We have a set of **14 breadfruit (Artocarpus altilis)** accessions with high-quality chromosome-level assemblies. Breadfruit has 28 chromosomes, and our assemblies are collapsed haploid (we don't care about haplotypes).

To provide some ecological/evolutionary background, this [2023 paper](https://doi.org/10.1016/j.cub.2022.12.001) has a great overview of our current understanding of breadfruit genetic and geographic diversity.

Info on our germplasm collection of breadfruit near Hilo: [link](https://www.ars.usda.gov/pacific-west-area/hilo-hi/daniel-k-inouye-us-pacific-basin-agricultural-research-center/tropical-plant-genetic-resources-and-disease-research/docs/breadfruit-collection/) 

Info on breadfruit cultivar varieties from the national botanical garden: [link](https://ntbg.org/breadfruit/about-breadfruit/varieties/) 

## What is the "pangenome graph" for this project?
A pangenome graph is a graph representation of multiple genomes that captures:
- shared sequence as shared graph segments
- structural variation and alternative sequence as branching paths

A nice review of pangenome utility and development is [Kaur et al 2024](https://doi.org/10.1186/s12864-024-10931-w).

---

# Tutorials & resources

## PGGB
- [PGGB official quick start](https://pggb.readthedocs.io/en/latest/rst/quick_start.html). 
- [PGGB GitHub](https://github.com/pangenome/pggb).
- [Pangenomics workshop tutorial](https://inbre.ncgr.org/pangenomics-workshop/reference-free-graphs-with-pggb.html) ("Reference-free graphs with pggb").
- [CPANG shorter tutorial](https://gtpb.github.io/CPANG22/) maybe useful.
- [Panacus](https://github.com/codialab/panacus/tree/main) for pangenome statistics / growth curves. 
- [SequenceTubeMap](https://github.com/vgteam/sequenceTubeMap) for visualization. 
- [Bandage](https://rrwick.github.io/Bandage/) plots for graph visualization. 
---

## SciNet Overview

Nearly all of our work is done on the SciNet cluster [ceres](https://scinet.usda.gov/guides/use/). I'm not sure how much background you have on HPC systems, but there's some good ceres-specific resources available, such as [here](https://datascience.101workbook.org/06-hpc/01-hpc-networks/02-scinet-usda-ars/03-scinet-ceres-cluster/#gsc.tab=0) .

I also highly recommend using ceres' [OnDemand](https://ceres-ondemand.scinet.usda.gov/pun/sys/dashboard)  tools: their VScode sessions allow you to request CPUs/RAM so you can troubleshoot / edit scripts directly in the session, save your workspace / folders / scripts for easy access at next log-in, and even easy figure downloading. I'd be happy to give you a quick screenshare overview in case you're curious. 

Typically, we will submit jobs using a SLURM header like this, "charging" our hours to our core project `coffea_pangenome`

```bash
#!/bin/bash

#SBATCH --time=4-00:00:00    
#SBATCH --cpus-per-task=24
#SBATCH --mem=48Gb
#SBATCH --partition=ceres
#SBATCH --account=coffea_pangenome
```

You will have a small home directory (~15 Gb), but we have a large project directory here: `/project/coffea_pangenome` 

A project subfolder already exists for the pangenome work here: `/project/coffea_pangenome/Breadfruit_Pangenome` 


## Documentation & tools

Ceres has lots of tools available via typical `module load X`. If you are a conda/mamba user, you can access this via:

``` 
module load miniconda
mamba create -n env1 bcftools
source activate env1

#to deactivate
conda deactivate env1
```

You probably already have ways of documenting your code (Git / markdown), but in case not - I highly suggest a markdown editor for project-specific code management. These 'binders' can be great ways to organize your projects, particularly larger bioinfo projects where not everything will run in a single jupyter/rmarkdown doc. I forked over the one-time $15 for a [typora](https://typora.io/) license because I haven't found anything nearly as good for tracking code across dozens of projects, but there are free alternatives like [MarkText](https://marktext.me/) and [Obsidian](https://obsidian.md/). Not necessary, but potentially very useful! Also happy to give a quick markdown crash course if interested. 

## People

| Name                     | Position                             | Pic                          |
| ------------------------ | ------------------------------------ | ---------------------------- |
| **Dr. Qingyi Yu**        | Primary advisor, Research Geneticist | ![Qingyi](imgs/Qingyi.png)   |
| **Dr. Justin Merondun**  | Research advisor, Postdoc            | ![Justin](imgs/Justin.png)   |
| **Amberly Buer**         | Lab support, Technician              | ![Amberly](imgs/Amberly.png) |
| **Dr. Zhikai Yang**      | Ancillary support, Postdoc           | ![Zhikai](imgs/Zhikai.png)   |
| **Dr. Tracie Matsumoto** | USDA Hilo Plant Unit Research Leader | ![Tracie](imgs/Tracie.png)   |

## Metadata

The assemblies were generated from long read PacBio HiFi and HiC data for 14 breadfruit samples. Breadfruit has recently diversified into different ploidy levels including 3N and 4N. This elevation in ploidy was recent and results from self-duplication (autopolyploid), so we don't expect much variation to have accumulated between the different haplotypes of the 3N/4N samples yet. Metadata available as a tsv [here](/metadata.tsv), including paths to the raw HiFi data. 

| Accession | Cultivar      | Ploidy    | Seeds    |
| --------- | ------------- | --------- | -------- |
| HART001   | Maafala       | 2N        | Seedless |
| HART050   | Kukumu_tasi   | 2N        | Seeded   |
| HART069   | Ulu_Fiti      | 2N        | Seeded   |
| HART030   | Huero         | 3N        | Seedless |
| HART032   | Hamoa         | 3N        | Seedless |
| HART033   | Patara        | 3N        | Seedless |
| HART038   | Lemai         | 3N        | Seedless |
| HART053   | Nahnmwal      | 3N        | Seedless |
| ZZ3       | Ulu_hamoa     | hybrid_2N | Seeded   |
| ZZ7       | Ulu_afa_elise | hybrid_2N | Seeded   |
| ZZ9       | Ulu_afa       | hybrid_2N | Seeded   |
| HART046   | Midolab       | hybrid_3N | Seedless |
| HART049   | Meinpohnsakar | hybrid_3N | Seedless |
| H6        | Meikole       | hybrid_4N | Seeded   |

---

# Inputs & naming conventions

## Assemblies
We have 14 accessions with chromosome-scale assemblies (28 chromosomes). Assemblies are collapsed haploid assemblies (not haplotype-phased). 

## Chromosome-by-chromosome input FASTAs
You will be provided with one multi-FASTA per chromosome in `/project/coffea_pangenome/Breadfruit_Pangenome/01_assemblies`, containing **one record per accession**:

- `input/chr01.fa.gz` contains 14 sequences:
  - `>HART001#Chr01`
  - `>HART030#Chr01`
  - ...
  - `>ZZ9#Chr01`

This header style is important (following the [PanSN-spec](https://github.com/pangenome/PanSN-spec)) because it creates clear per-accession paths like `HART001#Chr01` in the graph.

All per-chromosome FASTAs are already bgzipped and indexed:
- `bgzip -c chr01.fa > chr01.fa.gz`
- `samtools faidx chr01.fa.gz`

---

# Highest-priority goals/deliverables

1. **Per-chromosome pangenome graphs (Chr01–Chr28)** built with PGGB
2. **QC summary tables** (graph sizes, path counts per chromosome)
3. **Whole-genome pangenome accumulation curve**:
   - y-axis = bp encompassed (union bp of graph sequence traversed by the first k accessions' paths)
   - x-axis = number of accessions (k = 1..14) 
4. **One hero-locus visualization**:
   - a subgraph chunk + tube map figure demonstrating alternative paths / SV
5. Scripts/md notebook + software versions

Example of (3) accumulation curve from [Kopalli et al 2025 Sup fig 2](https://doi.org/10.1093/gigascience/giaf121):

![Kapalli et al](/imgs/growth_curve_kapalli.png)

Example of (4) bandage & tube plot from [Hu et al 2026](https://doi.org/10.1016/j.isci.2026.115539):

![Hu et al](/imgs/bandage_tube_hu.png)

---

# Tentative outline (10-week plan)

| Week | Overall objective | Suggested tools | Target outputs |
|------|-------------------|-----------------|----------------|
| 1 | Familiarze on ceres; run a **pilot PGGB build** | Apptainer, `pggb` | `Chr01` pilot graph; verified FASTA naming scheme |
| 2 | Validate pilot graph (paths present, basic sanity); **tune PGGB parameters** | `pggb`, [odgi](https://github.com/pangenome/odgi) | Final parameter choice; "golden" Chr01 run; confirm n=14 paths |
| 3 | **Start full PGGB builds** | bash | some updates on graphs |
| 4 | **Run PGGB builds** for Chr01–Chr28; monitor failures/re-run | `pggb` | Majority of chromosome graphs completed; build status table |
| 5 | Finish all per-chromosome graphs; **create QC summary** (graph file sizes, graph bp, number of paths) | bash/Python/R; odgi utilities | summary stats table; all final GFAs archived |
| 6 | **Finalize QC**, convert graphs to final odgi format; compute per-path and per-graph stats per chromosome | `odgi` | Tables for: (i) graph length per chr, (ii) per-path length per chr, (iii) path list per chr |
| 7 | **Compute accumulation curve** from the final graphs: union-of-path bp per k, sum across chromosomes | odgi-output tables, [panacus](https://github.com/codialab/panacus) | figure or table for curves |
| 8 | Finalize accumulation curve; identify a "hero locus" region and plot tube map  | R/python, `odgi`/`vg`, [`sequenceTubeMap`](https://github.com/vgteam/sequenceTubeMap) | accumulation curve and tube map figures  |
| 9 | Edit, clean, and **finalize bioinformatic documentation, start writing methods/report.**  | Markdown/similar  | Organized git repo or md notebook |
| 10 | **Finish final report**, focus on proper cited methods, short results & discussion | Markdown, word | Methods and figures! |

---

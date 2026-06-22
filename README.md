# Z-DNA-induced genomic instability in the human pangenome

Analysis of Z-DNA forming sequences across the human pangenome and their
association with germline and de novo mutation. Z-DNA is predicted with
[ZSeeker](https://github.com/Georgakopoulos-Soares-lab/ZSeeker) and compared
against GC- and length-matched random controls.

Genomes analysed: **HPRC release 2** (minigraph-cactus haplotype assemblies),
**T2T-CHM13v2.0**, **GRCh38.p14**, and **GRCh37**.

## Data sources

| Resource | Accession / location |
|---|---|
| HPRC release 2 assemblies | `data.humanpangenome.org`, `human-pangenomics/hprc_intermediate_assembly` |
| T2T-CHM13v2.0 + annotations | NCBI `GCF_009914755.1`, `marbl/CHM13` (CenSat, RepeatMasker, telomere, Liftoff genes) |
| GRCh38.p14 (+ MANE v1.5) | NCBI `GCF_000001405.40` |
| GRCh37 | NCBI `GCF_000001405.13` |
| Germline variants | HPRC v2 minigraph-cactus "wave" VCF (CHM13 coordinates) |
| De novo variants | [denovo-db](https://denovo-db.gs.washington.edu) (GRCh37) |

## Pipeline

The full analysis lives in [`zdna_pangenomes.ipynb`](zdna_pangenomes.ipynb):

1. **Z-DNA prediction** — ZSeeker on every assembly (HPRC run as a SLURM array).
2. **Effective genome size and density** — per-chromosome/per-haplotype Z-DNA
   density over ACGT-only effective length.
3. **Figure 1** — private vs shared predictions, score/length distributions,
   density across superpopulations, per-chromosome and parental differences.
4. **GC-matched controls** at 5 / 10 / 15% GC tolerance.
5. **Karyograms** — Z-DNA density by cytoband, CenSat class, and sequence class.
6. **Genomic-feature enrichment** — genes, introns, repeats, CenSat, telomere.
7. **Mutation-anchored metaprofiles** — Z-DNA vs control enrichment around
   germline and de novo mutations, stratified by mutation type, Z-DNA score,
   variant size, genomic feature, and allele frequency.

## Requirements

Python ≥ 3.11 with `numpy`, `pandas`, `polars`, `pyarrow`, `scipy`,
`matplotlib`, `biopython`, `tqdm`, plus `ZSeeker` and `bedtools` on the path.

## Notes

- All file paths in the notebook are short placeholders (`data/...`, `out/...`);
  replace them with your own locations.
- Heavy steps (ZSeeker prediction, per-haplotype tables, control generation)
  were run on an HPC cluster. The notebook shows the analysis logic rather than
  a single turn-key driver script.

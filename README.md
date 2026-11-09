# Sex-Chromosome Dosage Analysis in *Plutella xylostella* (Diamondback Moth)
**Using NCBI, Galaxy, and R**

---

## Project Information
| Item | Details |
|---|---|
| **Title** | Sex-Chromosome Dosage in *Plutella xylostella* (Diamondback Moth) Using NCBI, Galaxy, and R |
| **Course** | Cell and Molecular Biology — Individual Practical Exercise |
| **Student** | Jade Angela Suan |
| **Section** | B |
| **Date** | 2026-11-09 |
| **BioSample** | SAMN08388737 |
| **BioProject** | PRJNA431333 |
| **Sex System** | ZZ/ZW |

---

## 1. Selected Species and Study Question

### Species
*Plutella xylostella* (diamondback moth), Order Lepidoptera.
Sex determination system: **ZZ/ZW** — female-heterogametic (ZW = female; ZZ = male).

### Study Question
Does the whole-genome sequencing (WGS) read depth on the Z chromosome, relative to the autosomes, indicate that the individual that supplied the reads carries **two Z copies (ZZ-like)** or **one Z copy (ZW-like)**?

### Approach
1. WGS reads are mapped to a chromosome-level reference genome.
2. Mean read depth per chromosome is calculated using **Samtools coverage**.
3. The ratio of Z chromosome depth to the **median autosome depth** is computed in **R**.
4. Under a standard ZZ/ZW system:
   - **Z:A ≈ 1.0** → consistent with **ZZ** (two copies, male)
   - **Z:A ≈ 0.5** → consistent with **ZW** (one copy, female)

---

## 2. Accessions and Links

| Item | Accession / Details | Link |
|---|---|---|
| Assembly (RefSeq) | GCF_932276165.2 (iPluXylo3.2) | https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_932276165.2/ |
| Assembly level | Chromosome | https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_932276165.2/ |
| Assembly BioSample | SAMEA7520369 | https://www.ncbi.nlm.nih.gov/biosample/SAMEA7520369/ |
| WGS run | SRR6505218<br />(Illumina HiSeq 2000, WGS, paired) | https://www.ncbi.nlm.nih.gov/sra/SRR6505218 |
| WGS BioSample | SAMN08388737 | https://www.ncbi.nlm.nih.gov/biosample/SAMN08388737 |
| WGS BioProject | PRJNA431333 | https://www.ncbi.nlm.nih.gov/bioproject/PRJNA431333 |
| Z chromosome (RefSeq/GenBank) | NC_064012.1 / OW026553.1 | https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_932276165.1/ |

Same donor? No. The assembly is from a male collected at Wytham Great Wood, United Kingdom (2019). The reads come from an adult moth collected at Bairds Bay, South Australia (2013). The result therefore refers to the WGS donor only.

---

## Workflow
Raw Reads → FastQC → BWA-MEM Alignment → Samtools Sort → Samtools Coverage → R Analysis


### Tools & Parameters
| Step | Tool | Details |
|---|---|---|
| Quality Control | FastQC | Default settings |
| Alignment | BWA-MEM | Paired-end reads; reference genome GCF_932276165.2 |
| Coverage | Samtools Coverage | MAPQ ≥ 20; exclude unmapped, duplicates, low-quality alignments |
| Analysis | R | Median normalization; dosage ratio calculation |

---

## Results

| Metric | Value |
|---|---|
| Median Autosome Depth | 19.281 × |
| Z Chromosome Depth | 23.372 × |
| **Z/A Ratio** | **1.212** |
| W Chromosome Depth | Not detected |
| **W/A Ratio** | **0.000** |
| **Final Sex Call** | **MALE (ZZ)** |

### Interpretation
- **Z/A = 1.212** → Z chromosome coverage is consistent with two copies (diploid)
- **W chromosome absent** → No female-specific chromosome detected
- **Conclusion:** Individual is **male (ZZ)**
- Ratio slightly above 1.0 is normal — caused by sequence divergence, mappability differences, and reference genome completeness

---

## Visualization
- **chromosome_depth.png** — Bar plot of relative depth per chromosome
- Y-axis: depth normalized to median autosome level (dashed line = 1.0)
- Z chromosome (far right bar) shows coverage matching autosome levels → ZZ genotype
- No chromosome shows ~0.5× coverage → no single-copy Z as in ZW female

---

## Results Summary 
> The BioSample record (SAMN08388737) for *Plutella xylostella* did not include sex metadata. Read coverage dosage analysis was performed to infer sex. Median autosome coverage was 19.28×. The Z chromosome showed a Z/A ratio of 1.21, consistent with two copies (ZZ). No coverage was detected for the W chromosome. These results indicate the sampled individual is **male (ZZ)**.

---

## Limitations
- W chromosome may be absent from reference assembly due to high repeat content
- Single-sample analysis — further samples would strengthen confirmation
- Z/A ratio deviation from 1.0 reflects reference and mapping variation, not ploidy

---

## References
- BioSample: https://www.ncbi.nlm.nih.gov/biosample/SAMN08388737
- BioProject: https://www.ncbi.nlm.nih.gov/bioproject/PRJNA431333
- Reference: GCF_932276165.2 (*Plutella xylostella*, iPluXylo3.2)

---

## Prepared By
**Jade Angela Suan**
Course / Lab: *BS-BIO Cell Mol*

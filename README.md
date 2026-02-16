# Principal Component Analysis for Genomic Data

##  Overview

Principal Component Analysis (PCA) is a fundamental technique in population genetics for detecting population structure, ancestry, and genetic relatedness. This project applies PCA to genome-wide SNP data to visualize genetic diversity, identify population clusters, and correct for ancestry confounding in association studies.

### Applications
- **Population Structure**: Identify distinct genetic populations
- **Ancestry Inference**: Determine individual ancestry proportions
- **GWAS Quality Control**: Detect population stratification and outliers
- **Admixture Analysis**: Identify mixed-ancestry individuals
- **Evolutionary Studies**: Visualize human migration patterns

##  Methods & Workflow

### Analysis Pipeline

1. **Genotype Quality Control**
   - Filter variants by minor allele frequency (MAF > 0.01)
   - Hardy-Weinberg equilibrium test (p > 1e-6)
   - Remove variants with high missingness
   - Sample quality control (call rate, heterozygosity)

2. **LD Pruning**
   - Remove correlated SNPs (r² < 0.2)
   - Independent variants for accurate PCA
   - Typically retain ~100K-500K SNPs

3. **PCA Computation**
   - Standardize genotype matrix
   - Compute covariance matrix
   - Calculate eigenvectors and eigenvalues
   - Extract principal components (PCs)

4. **Variance Explained Analysis**
   - Scree plot of eigenvalues
   - Determine informative PCs
   - Typically PC1-10 capture population structure

5. **Population Assignment**
   - Cluster individuals by PC scores
   - K-means or hierarchical clustering
   - Compare to reference populations

6. **Visualization**
   - 2D PC plots (PC1 vs PC2, PC2 vs PC3)
   - 3D interactive plots
   - Color by population/ancestry
   - Outlier detection

### Technologies Used
- Python, NumPy for matrix operations
- scikit-learn for PCA implementation
- Matplotlib/Seaborn for visualization
- PLINK for LD pruning (external tool)
- pandas for data manipulation

##  Key Results

### PC Interpretation
- **PC1-2**: Usually separate major continental populations
  - European, African, East Asian, South Asian, American
- **PC3-5**: Capture finer population substructure
  - Within-continent variation
- **PC6-10**: Often less interpretable, but useful for GWAS correction
- **Variance Explained**: PC1 typically explains 5-15% of variation

### Population Clustering
- Clear separation of ancestral populations
- Admixed individuals plot between reference populations
- Clines along migration routes (e.g., Europe-Middle East-Asia)
- Outliers may indicate:
  - Sample contamination
  - Incorrect metadata
  - Recent admixture
  - Sequencing errors

##  Usage

### Prerequisites
```bash
pip install -r requirements.txt
```

### Running the Analysis
```bash
# Interactive mode
jupyter notebook genomic-pca.ipynb

# Batch mode
jupyter nbconvert --execute --to html genomic-pca.ipynb
```

### Input Data Format
The analysis expects genotype data in one of these formats:
- **VCF**: Variant Call Format (`.vcf` or `.vcf.gz`)
- **PLINK**: Binary format (`.bed/.bim/.fam`)
- **Matrix**: Samples × SNPs genotype matrix (0/1/2 encoding)

Example genotype matrix:
```
       rs123  rs456  rs789  ...
IND001    0      1      2
IND002    1      0      1
IND003    2      2      0
```

##  Project Structure

```
genomic-pca/
├── genomic-pca.ipynb                 # Main analysis notebook
├── README.md                          # This file
└── requirements.txt                   # Python dependencies
```

##  Data Sources

### Datasets Used

This analysis was performed on genomic datasets from the Computational Genomics course:

**Genotype Data:**
- Genome-wide SNP data from course materials
- Quality-controlled variant calls
- Multi-population samples for demonstrating PCA

**Reference Populations:**
- Samples representing major ancestral populations
- Used for benchmarking and validation
- Typical populations: African (AFR), European (EUR), East Asian (EAS), South Asian (SAS), Admixed American (AMR)

**Public Alternatives for Reproduction:**
To perform similar PCA analysis with your own data:

1. **1000 Genomes Project**: https://www.internationalgenome.org/
   - Phase 3: 2,504 individuals from 26 populations
   - Whole genome sequences
   - Perfect for PCA benchmarking

2. **Human Genome Diversity Project (HGDP)**: https://www.hagsc.org/hgdp/
   - 1,043 individuals from 52 populations
   - Global geographic coverage
   - Good for population structure studies

3. **UK Biobank**: https://www.ukbiobank.ac.uk/
   - 500K+ participants (application required)
   - Dense SNP array data
   - Within-Europe fine structure

**To reproduce this analysis:**
1. Download genotype data from public sources
2. Perform quality control (MAF, HWE, missingness)
3. LD prune to independent SNPs
4. Follow PCA steps in the notebook
5. Visualize and interpret results


##  Biological & Clinical Significance

### Why PCA Matters in Genomics

**1. Correct for Population Stratification in GWAS**
- False-positive associations due to ancestry differences
- Include top PCs as covariates in regression
- Standard practice in all modern GWAS

**2. Quality Control**
- Detect sample swaps, contamination
- Verify self-reported ancestry
- Identify cryptic relatedness

**3. Ancestry Prediction**
- Assign individuals to populations
- Estimate admixture proportions
- Relevant for:
  - Clinical genetics (ancestry-specific risk)
  - Pharmacogenomics (drug response varies by ancestry)
  - Personalized medicine

**4. Evolutionary Insights**
- Visualize human migration patterns
- Quantify genetic drift and gene flow
- Study adaptation to different environments

### Practical Applications

**Clinical Genetics:**
- Some variants are population-specific (e.g., sickle cell in AFR)
- Disease prevalence varies by ancestry
- Polygenic risk scores trained on EUR don't transfer well to other ancestries

**Pharmacogenomics:**
- CYP2D6 poor metabolizers common in East Asians
- VKORC1 variants affect warfarin dosing by ancestry
- Abacavir hypersensitivity (HLA-B*5701) varies by population

##  Dependencies

See `requirements.txt`:
```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
scikit-learn>=1.1.0
jupyter>=1.0.0
```

**Optional External Tools:**
- PLINK 1.9+ (for efficient LD pruning)
- EIGENSOFT (alternative PCA implementation)

## 📖 References & Resources

### Key Publications
- Price AL, et al. "Principal components analysis corrects for stratification in genome-wide association studies." *Nature Genetics* (2006)
- Patterson N, et al. "Population structure and eigenanalysis." *PLOS Genetics* (2006)
- Novembre J, et al. "Genes mirror geography within Europe." *Nature* (2008)

### Software & Tools
- **EIGENSOFT**: http://www.hsph.harvard.edu/alkes-price/software/
- **PLINK**: https://www.cog-genomics.org/plink/
- **scikit-learn PCA**: https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html

### Databases
- **1000 Genomes**: https://www.internationalgenome.org/
- **HGDP**: https://www.hagsc.org/hgdp/
- **gnomAD**: https://gnomad.broadinstitute.org/ (population frequencies)

---

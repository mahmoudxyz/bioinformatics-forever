# PLINK Genotype File Formats

## What is PLINK and Why Do We Need It?

**[PLINK](https://www.cog-genomics.org/plink/)** is a free, open-source toolset designed for genome-wide association studies (GWAS) and population genetics analysis.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZRyfpe1zqVg?si=d7uege6VS5B7pO6N" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Why PLINK Exists

When you're dealing with genotype data from thousands (or millions) of people across hundreds of thousands (or millions) of genetic variants, you face several problems:

1. **File size:** Raw genotype data is MASSIVE
2. **Processing speed:** Reading and analyzing this data needs to be fast
3. **Standardization:** Different labs and companies produce data in different formats
4. **Analysis tools:** You need efficient ways to compute allele frequencies, test for associations, filter variants, etc.

PLINK solves these problems by providing:

- Efficient binary file formats (compact storage)
- Fast algorithms for common genetic analyses
- Format conversion tools
- Quality control utilities

### When You'd Use PLINK

- Analyzing data from genotyping chips (Illumina, Affymetrix)
- Running genome-wide association studies (GWAS)
- Computing population genetics statistics
- Quality control and filtering of genetic variants
- Converting between different genotype file formats

---

## PLINK Binary Format (.bed/.bim/.fam)

This is PLINK's primary format - a set of three files that work together. It's called "binary" because the main genotype data is stored in a compressed binary format rather than human-readable text.

### The .fam File (Family/Sample Information)

The `.fam` file contains information about each individual (sample) in your study. It has **6 columns** with **NO header row**.

**Format:**

```text

FamilyID  IndividualID  FatherID  MotherID  Sex  Phenotype

```

**Example .fam file:**

```text

FAM001  IND001  0  0  1  2
FAM001  IND002  0  0  2  1
FAM002  IND003  IND004  IND005  1  -9
FAM002  IND004  0  0  1  1
FAM002  IND005  0  0  2  1
```

#### Column Breakdown:

**Column 1: Family ID**
- Groups individuals into families
- Can be the same as Individual ID if samples are unrelated
- Example: `FAM001`, `FAM002`

**Column 2: Individual ID**
- Unique identifier for each person
- Must be unique within each family
- Example: `IND001`, `IND002`

**Column 3: Paternal ID (Father)**
- Individual ID of the father
- `0` = father not in dataset (unknown or not genotyped)
- Used for constructing pedigrees and family-based analyses

**Column 4: Maternal ID (Mother)**
- Individual ID of the mother
- `0` = mother not in dataset
- Must match an Individual ID if the parent is in the study

**Column 5: Sex**
- `1` = Male
- `2` = Female
- `0` = Unknown sex
- Other codes (like `-9`) are sometimes used for unknown, but `0` is standard

**Column 6: Phenotype**
- The trait you're studying (disease status, quantitative trait, etc.)
- **For binary (case-control) traits:**
  - `1` = Control (unaffected)
  - `2` = Case (affected)
  - `0` or `-9` = Missing phenotype
- **For quantitative traits:** Any numeric value
- `-9` = Standard missing value code

#### Important Notes About Special Codes:

**`0` (Zero):**
- In Parent columns: Parent not in dataset
- In Sex column: Unknown sex
- In Phenotype column: Missing phenotype (though `-9` is more common)

**`-9` (Negative nine):**
- Universal "missing data" code in PLINK
- Most commonly used for missing phenotype
- Sometimes used for unknown sex (though `0` is standard)

**Why these codes matter:**
- PLINK will skip individuals with missing phenotypes in association tests
- Parent information is crucial for family-based tests (like TDT)
- Sex information is needed for X-chromosome analysis

---

### The .bim File (Variant Information)

The `.bim` file (binary marker information) describes each genetic variant. It has **6 columns** with **NO header row**.

**Format:**
```
Chromosome  VariantID  GeneticDistance  Position  Allele1  Allele2
```

**Example .bim file:**
```
1   rs12345    0    752566    G    A
1   rs67890    0    798959    C    T
2   rs11111    0    1240532   A    G
3   rs22222    0    5820321   T    C
X   rs33333    0    2947392   G    A
```

#### Column Breakdown:

**Column 1: Chromosome**
- Chromosome number: `1-22` (autosomes)
- Sex chromosomes: `X`, `Y`, `XY` (pseudoautosomal), `MT` (mitochondrial)
- Example: `1`, `2`, `X`

**Column 2: Variant ID**
- Usually an rsID (reference SNP ID from dbSNP)
- Format: `rs` followed by numbers (e.g., `rs12345`)
- Can be any unique identifier if rsID isn't available
- Example: `chr1:752566:G:A` (chromosome:position:ref:alt format)

**Column 3: Genetic Distance**
- Position in centimorgans (cM)
- Measures recombination distance, not physical distance
- Often set to `0` if unknown (very common)
- Used in linkage analysis and some phasing algorithms

**Column 4: Base-Pair Position**
- Physical position on the chromosome
- Measured in base pairs from the start of the chromosome
- Example: `752566` means 752,566 bases from chromosome start
- **Critical for genome builds:** Make sure you know if it's GRCh37 (hg19) or GRCh38 (hg38)!

**Column 5: Allele 1**
- First allele (often the reference allele)
- Single letter: `A`, `C`, `G`, `T`
- Can also be `I` (insertion), `D` (deletion), or `0` (missing)

**Column 6: Allele 2**
- Second allele (often the alternate/effect allele)
- Same coding as Allele 1

#### Important Notes:

**Allele coding:**
- These alleles define what genotypes mean in the .bed file
- Genotype `AA` means homozygous for Allele1
- Genotype `AB` means heterozygous
- Genotype `BB` means homozygous for Allele2

**Strand issues:**
- Alleles should be on the forward strand
- Mixing strands between datasets causes major problems in meta-analysis
- Always check strand alignment when combining datasets!

---

### The .bed File (Binary Genotype Data)

The `.bed` file contains the actual genotype calls in compressed binary format. **This file is NOT human-readable** - you can't open it in a text editor and make sense of it.

**Key characteristics:**

**Why binary?**
- **Space efficiency:** A text file with millions of genotypes is huge; binary format compresses this dramatically
- **Speed:** Computer can read binary data much faster than parsing text
- **Example:** A dataset with 1 million SNPs and 10,000 people:
  - Text format (.ped): ~30 GB
  - Binary format (.bed): ~2.4 GB

**What's stored:**
- Genotype calls for every individual at every variant
- Each genotype is encoded efficiently (2 bits per genotype)
- **Encoding:**
  - `00` = Homozygous for allele 1 (AA)
  - `01` = Missing genotype
  - `10` = Heterozygous (AB)
  - `11` = Homozygous for allele 2 (BB)

**SNP-major vs. individual-major:**
- PLINK binary files are stored in **SNP-major** mode by default
- This means genotypes are organized by variant (all individuals for SNP1, then all individuals for SNP2, etc.)
- More efficient for most analyses (which process one SNP at a time)

**You never edit .bed files manually** - always use PLINK commands to modify or convert them.

---

## PLINK Text Format (.ped/.map)

This is the original PLINK format. It's human-readable but **much larger** and **slower** than binary format. Mostly used for small datasets or when you need to manually inspect/edit data.

### The .map File (Variant Map)

Similar to .bim but with only **4 columns**.

**Format:**
```
Chromosome  VariantID  GeneticDistance  Position
```

**Example .map file:**
```
1   rs12345    0    752566
1   rs67890    0    798959
2   rs11111    0    1240532
3   rs22222    0    5820321
```

Notice: **NO allele information** in .map files (unlike .bim files).

---

### The .ped File (Pedigree + Genotypes)

Contains both sample information AND genotype data in one large text file.

**Format:**
```
FamilyID  IndividualID  FatherID  MotherID  Sex  Phenotype  [Genotypes...]
```

The first 6 columns are identical to the .fam file. After that, genotypes are listed as **pairs of alleles** (one pair per SNP).

**Example .ped file:**
```
FAM001  IND001  0  0  1  2  G G  C T  A G  T T
FAM001  IND002  0  0  2  1  G A  C C  A A  T C
FAM002  IND003  0  0  1  1  A A  T T  G G  C C
```

#### Genotype Encoding:

Each SNP is represented by **two alleles separated by a space**:
- `G G` = Homozygous for G allele
- `G A` = Heterozygous (one G, one A)
- `A A` = Homozygous for A allele
- `0 0` = Missing genotype

**Important:** The order of alleles in heterozygotes doesn't matter (`G A` = `A G`).

**Problems with .ped format:**
- **HUGE files** for large datasets (gigabytes to terabytes)
- **Slow to process** (text parsing is computationally expensive)
- **No explicit allele definition** (you have to infer which alleles exist from the data)

**When to use .ped/.map:**
- Small datasets (< 1,000 individuals, < 10,000 SNPs)
- When you need to manually edit genotypes
- Importing data from older software
- **Best practice:** Convert to binary format (.bed/.bim/.fam) immediately for analysis

---

## Transposed Format (.tped/.tfam)

This format is a "transposed" version of .ped/.map. Instead of one row per individual, you have **one row per SNP**.

### The .tfam File

Identical to .fam file - contains sample information.

**Format:**
```
FamilyID  IndividualID  FatherID  MotherID  Sex  Phenotype
```

---

### The .tped File (Transposed Genotypes)

Each row represents one SNP, with genotypes for all individuals.

**Format:**
```
Chromosome  VariantID  GeneticDistance  Position  [Genotypes for all individuals...]
```

**Example .tped file:**
```
1  rs12345  0  752566  G G  G A  A A  G G  A A
1  rs67890  0  798959  C T  C C  T T  C T  C C
2  rs11111  0  1240532 A G  A A  G G  A G  A A
```

The first 4 columns are like the .map file. After that, genotypes are listed for **all individuals** (2 alleles per person, space-separated).

**When to use .tped/.tfam:**
- When your data is organized by SNP rather than by individual
- Converting from certain genotyping platforms
- Some imputation software prefers this format
- **Still text format** so same size/speed issues as .ped

---

## Long Format

Long format (also called "additive" or "dosage" format) represents genotypes as **numeric values** instead of allele pairs.

**Format options:**

**Additive coding (most common):**
```
FamilyID  IndividualID  VariantID  Genotype
FAM001    IND001        rs12345    0
FAM001    IND001        rs67890    1
FAM001    IND001        rs11111    2
FAM001    IND002        rs12345    1
```

**Numeric genotype values:**
- `0` = Homozygous for reference allele (AA)
- `1` = Heterozygous (AB)
- `2` = Homozygous for alternate allele (BB)
- `NA` or `-9` = Missing

**Why long format?**
- **Easy to use in statistical software** (R, Python pandas)
- **Flexible for merging with other data** (phenotypes, covariates)
- **Good for database storage** (one row per observation)
- **Can include dosages** for imputed data (values between 0-2, like 0.85)

**Downsides:**
- **MASSIVE file size** (one row per person per SNP)
- Example: 10,000 people × 1 million SNPs = 10 billion rows
- Not practical for genome-wide data without compression

**When to use:**
- Working with a small subset of SNPs in R/Python
- Merging genotypes with other tabular data
- Machine learning applications where you need a feature matrix

---

## Variant Call Format (VCF)

VCF is the **standard format for storing genetic variation** from sequencing data. Unlike genotyping arrays (which only check specific SNPs), sequencing produces **all** variants, including rare and novel ones.

**Key characteristics:**

**Comprehensive information:**
- Genotypes for all samples at each variant
- Quality scores for each call
- Read depth, allele frequencies
- Functional annotations
- Multiple alternate alleles at the same position

**File structure:**
- **Header lines** start with `##` (metadata about reference genome, samples, etc.)
- **Column header line** starts with `#CHROM` (defines columns)
- **Data lines:** One per variant

**Standard VCF columns:**
```
#CHROM  POS     ID         REF  ALT     QUAL  FILTER  INFO           FORMAT  [Sample genotypes...]
1       752566  rs12345    G    A       100   PASS    AF=0.23;DP=50  GT:DP   0/1:30  1/1:25  0/0:28
```

#### Column Breakdown:

**CHROM:** Chromosome (1-22, X, Y, MT)

**POS:** Position on chromosome (1-based coordinate)

**ID:** Variant identifier (rsID or `.` if none)

**REF:** Reference allele (what's in the reference genome)

**ALT:** Alternate allele(s) - can be multiple, comma-separated
- Example: `A,T` means two alternate alleles

**QUAL:** Quality score (higher = more confident call)
- Phred-scaled: QUAL=30 means 99.9% confidence
- `.` if unavailable

**FILTER:** Quality filter status
- `PASS` = passed all filters
- `LowQual`, `HighMissing`, etc. = failed specific filters
- `.` = no filtering applied

**INFO:** Semicolon-separated annotations
- `AF=0.23` = Allele frequency 23%
- `DP=50` = Total read depth
- `AC=10` = Allele count
- Many possible fields (defined in header)

**FORMAT:** Describes the per-sample data fields
- `GT` = Genotype
- `DP` = Read depth for this sample
- `GQ` = Genotype quality
- Example: `GT:DP:GQ`

**Sample columns:** One column per individual
- Data corresponds to FORMAT field
- Example: `0/1:30:99` means heterozygous, 30 reads, quality 99

#### Genotype Encoding in VCF:

**GT (Genotype) format:**
- `0/0` = Homozygous reference (REF/REF)
- `0/1` = Heterozygous (REF/ALT)
- `1/1` = Homozygous alternate (ALT/ALT)
- `./.` = Missing genotype
- `1/2` = Heterozygous with two different alternate alleles
- `0|1` = Phased genotype (pipe `|` instead of slash `/`)

**Phased vs. unphased:**
- `/` = unphased (don't know which allele came from which parent)
- `|` = phased (know parental origin)
- `0|1` means reference allele from parent 1, alternate from parent 2

#### Compressed VCF (.vcf.gz):

VCF files are usually **gzipped and indexed**:
- `.vcf.gz` = compressed VCF (much smaller)
- `.vcf.gz.tbi` = tabix index (allows fast random access)
- Tools like `bcftools` and `vcftools` work directly with compressed VCFs

**Example sizes:**
- Uncompressed VCF: 100 GB
- Compressed .vcf.gz: 10-15 GB
- Always work with compressed VCFs!

**When to use VCF:**
- **Sequencing data** (whole genome, exome, targeted)
- When you need detailed variant information
- Storing rare and novel variants
- Multi-sample studies with complex annotations
- **NOT typical for genotyping array data** (use PLINK binary instead)

---

## Oxford Format (.gen / .bgen + .sample)

Developed by the Oxford statistics group, commonly used in UK Biobank and imputation software (IMPUTE2, SHAPEIT).

### The .sample File

Contains sample information, similar to .fam but with a **header row**.

**Format:**
```
ID_1 ID_2 missing sex phenotype
0 0 0 D B
IND001 IND001 0 1 2
IND002 IND002 0 2 1
```

**First two rows are special:**
- **Row 1:** Column names
- **Row 2:** Data types
  - `D` = Discrete/categorical
  - `C` = Continuous
  - `B` = Binary
  - `0` = Not used

**Subsequent rows:** Sample data
- **ID_1:** Usually same as ID_2 for unrelated individuals
- **ID_2:** Sample identifier
- **missing:** Missingness rate (usually `0`)
- **sex:** `1`=male, `2`=female
- **phenotype:** Your trait of interest

---

### The .gen File (Genotype Probabilities)

Stores **genotype probabilities** rather than hard calls. This is crucial for **imputed data** where you're not certain of the exact genotype.

**Format:**
```
Chromosome  VariantID  Position  Allele1  Allele2  [Genotype probabilities for all samples...]
```

**Example .gen file:**
```
1  rs12345  752566  G  A  1 0 0  0.95 0.05 0  0 0.1 0.9
```

#### Genotype Probability Triplets:

For each sample, three probabilities (must sum to 1.0):
- **P(AA)** = Probability of homozygous for allele 1
- **P(AB)** = Probability of heterozygous
- **P(BB)** = Probability of homozygous for allele 2

**Example interpretations:**
- `1 0 0` = Definitely AA (100% certain)
- `0 0 1` = Definitely BB (100% certain)
- `0 1 0` = Definitely AB (100% certain)
- `0.9 0.1 0` = Probably AA, might be AB (uncertain genotype)
- `0.33 0.33 0.33` = Completely uncertain (missing data)

**Why probabilities matter:**
- **Imputed genotypes** aren't perfectly certain
- Better to use probabilities than picking "best guess" genotype
- Allows proper statistical modeling of uncertainty
- Example: If imputation says 90% chance of AA, 10% chance AB, you should account for that uncertainty

---

### The .bgen File (Binary Gen)

Binary version of .gen format - **compressed and indexed** for fast access.

**Key features:**
- **Much smaller** than text .gen files
- **Includes variant indexing** for rapid queries
- **Supports different compression levels**
- **Stores genotype probabilities** (like .gen) or dosages
- Used by UK Biobank and other large biobanks

**Associated files:**
- `.bgen` = Main genotype file
- `.bgen.bgi` = Index file (for fast lookup)
- `.sample` = Sample information (same as with .gen)

**When to use Oxford format:**
- Working with **imputed data**
- UK Biobank analyses
- Using Oxford software (SNPTEST, QCTOOL, etc.)
- When you need to preserve **genotype uncertainty**

**Converting to PLINK:**
- PLINK2 can read .bgen files
- Can convert to hard calls (loses probability information)
- Or use dosages (keeps uncertainty as 0-2 continuous values)

---

## 23andMe Format

23andMe is a direct-to-consumer genetic testing company. Their raw data format is simple but **NOT standardized** for research use.

**Format:**
```
# rsid    chromosome    position    genotype
rs12345    1    752566    AG
rs67890    1    798959    CC
rs11111    2    1240532   --
```

#### Column Breakdown:

**rsid:** Variant identifier (rsID from dbSNP)

**chromosome:** Chromosome number (1-22, X, Y, MT)
- Note: Sometimes uses `23` for X, `24` for Y, `25` for XY, `26` for MT

**position:** Base-pair position
- **Warning:** Build version (GRCh37 vs GRCh38) is often unclear!
- Check the file header or 23andMe documentation

**genotype:** Two-letter allele call
- `AG` = Heterozygous
- `AA` = Homozygous
- `--` = Missing/no call
- `DD` or `II` = Deletion or insertion (rare)

#### Important Limitations:

**Not standardized:**
- Different builds over time (some files are GRCh37, newer ones GRCh38)
- Allele orientation issues (forward vs. reverse strand)
- Variant filtering varies by chip version

**Only genotyped SNPs:**
- Typically 500k-1M SNPs (depending on chip version)
- No imputed data in raw download
- Focused on common variants (rare variants not included)

**Missing quality information:**
- No quality scores
- No read depth or confidence metrics
- "No call" (--) doesn't tell you why it failed

**Privacy and consent issues:**
- Users may not understand research implications
- IRB approval needed for research use
- Cannot assume informed consent for specific research

#### Converting 23andMe to PLINK:

Many online tools exist, but be careful:
1. Determine genome build (critical!)
2. Check strand orientation
3. Handle missing genotypes (-- → 0 0)
4. Verify chromosome coding (especially X/Y/MT)

**Typical workflow:**
```bash
# Convert to PLINK format (using a conversion script)
python 23andme_to_plink.py raw_data.txt

# Creates .ped and .map files
# Then convert to binary
plink --file raw_data --make-bed --out data
```

**When you'd use 23andMe data:**
- Personal genomics projects
- Ancestry analysis
- Polygenic risk score estimation
- Educational purposes
- **NOT suitable for:** Clinical decisions, serious GWAS (too small), research without proper consent

---

## Summary: Choosing the Right Format

| Format | Best For | Pros | Cons |
|--------|----------|------|------|
| **PLINK binary (.bed/.bim/.fam)** | GWAS, large genotyping arrays | Fast, compact, standard | Loses probability info |
| **PLINK text (.ped/.map)** | Small datasets, manual editing | Human-readable | Huge, slow |
| **VCF (.vcf/.vcf.gz)** | Sequencing data, rare variants | Comprehensive info, standard | Complex, overkill for arrays |
| **Oxford (.bgen/.gen)** | Imputed data, UK Biobank | Preserves uncertainty | Less common in US |
| **23andMe** | Personal genomics | Direct-to-consumer | Not research-grade |
| **Long format** | Statistical analysis in R/Python | Easy to manipulate | Massive file size |

**General recommendations:**

1. **For genotyping array data:** Use PLINK binary format (.bed/.bim/.fam)
2. **For sequencing data:** Use compressed VCF (.vcf.gz)
3. **For imputed data:** Use Oxford .bgen or VCF with dosages
4. **For statistical analysis:** Convert subset to long format
5. **For personal data:** Convert 23andMe to PLINK, but carefully

**File conversions:**
- PLINK can convert between most formats
- Always document your conversions (genome build, strand, filters)
- Verify a few variants manually after conversion
- Keep original files - conversions can introduce errors


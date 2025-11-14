# Illumina Sequencing

Illumina is the **dominant next-generation sequencing platform** worldwide. It uses reversible terminator chemistry and fluorescent detection to sequence millions of DNA fragments simultaneously with high accuracy.

---

## The Chemistry: Reversible Terminators

### The Core Principle

Unlike Ion Torrent (which detects H⁺ ions), Illumina detects **fluorescent light** from labeled nucleotides.

**Key innovation:** Reversible terminators

**Normal dNTP:**
- Has 3'-OH group
- Polymerase adds it and continues to next base

**Reversible terminator (Illumina):**
- Has 3'-OH **blocked** by a chemical group
- Has fluorescent dye attached
- Polymerase adds it and **stops**
- After imaging, the block and dye are **removed**
- Polymerase continues to next base

**Why this matters:** You get exactly **one base added per cycle**, making base calling precise.

---

## How It Works: Step by Step

### 1. Library Preparation

DNA is fragmented and **adapters** are ligated to both ends of each fragment.

**Adapters contain:**
- Primer binding sites
- Index sequences (barcodes for sample identification)
- Sequences complementary to flow cell oligos

### 2. Cluster Generation (Bridge Amplification)

This is Illumina's signature step - amplification happens **on the flow cell surface**.

**The flow cell:**
- Glass slide with millions of oligos attached to the surface
- Two types of oligos (P5 and P7) arranged in a lawn

**Bridge amplification process:**

**Step 1:** DNA fragments bind to flow cell oligos (one end attaches)

**Step 2:** The free end bends over and binds to nearby oligo (forms a "bridge")

**Step 3:** Polymerase copies the fragment, creating double-stranded bridge

**Step 4:** Bridge is denatured (separated into two strands)

**Step 5:** Both strands bind to nearby oligos and repeat

**Result:** Each original fragment creates a **cluster** of ~1,000 identical copies in a tiny spot on the flow cell.

**Why amplification?** Like Ion Torrent, a single molecule's fluorescent signal is too weak to detect. A thousand identical molecules in the same spot produce a strong signal.

**Visual representation:**
```
Original fragment: ═══DNA═══

After bridge amplification:
║ ║ ║ ║ ║ ║ ║ ║
║ ║ ║ ║ ║ ║ ║ ║  ← ~1000 copies in one cluster
║ ║ ║ ║ ║ ║ ║ ║
Flow cell surface
```

### 3. Sequencing by Synthesis

Now the actual sequencing begins.

**Cycle 1:**

1. **Add fluorescent reversible terminators** (all four: A, C, G, T, each with different color)
2. **Polymerase incorporates one base** (only one because it's a terminator)
3. **Wash away** unincorporated nucleotides
4. **Image the flow cell** with laser
   - Green light = A was added
   - Blue light = C was added
   - Yellow light = G was added
   - Red light = T was added
5. **Cleave off** the fluorescent dye and the 3' blocking group
6. **Repeat** for next base

**Cycle 2, 3, 4... 300+:**
Same process, one base at a time.

**Key difference from Ion Torrent:**
- Illumina: All four nucleotides present at once, polymerase chooses correct one
- Ion Torrent: One nucleotide type at a time, polymerase adds it only if it matches



## Color System

2 color and 4 colors system 
---

## No Homopolymer Problem

### Why Illumina Handles Homopolymers Better

Remember Ion Torrent's main weakness? Homopolymers like `AAAA` produce strong signals that are hard to quantify (is it 3 A's or 4?).

**Illumina doesn't have this problem** because:

1. **One base per cycle** - the terminator ensures only one nucleotide is added
2. **Direct counting** - if you see 4 green signals in a row, it's exactly 4 A's
3. **No signal intensity interpretation** - just presence/absence of color

**Example:**

**Sequence:** `AAAA`

**Illumina:**
```
Cycle 1: Green (A)
Cycle 2: Green (A)
Cycle 3: Green (A)
Cycle 4: Green (A)
→ Exactly 4 A's, no ambiguity
```

**Ion Torrent:**
```
Flow A: Large signal (proportional to 4 H⁺ ions)
→ Is it 4? Or 3? Or 5? (requires signal quantification)
```

---

## Error Profile: Substitutions, Not Indels

### Illumina's Main Error Type

**Substitution errors** - reading the wrong base (A instead of G, C instead of T)

**Error rate:** ~0.1% (1 error per 1,000 bases, or 99.9% accuracy)

**Common causes:**
1. **Phasing/pre-phasing** - some molecules in a cluster get out of sync
2. **Dye crosstalk** - fluorescent signals bleed between channels
3. **Quality degradation** - accuracy decreases toward end of reads

### Why Few Indels?

Because of the reversible terminator:
- Exactly one base per cycle
- Can't skip a base (would need terminator removal without incorporation)
- Can't add two bases (terminator blocks second addition)

**Comparison:**

| Error Type | Illumina | Ion Torrent |
|------------|----------|-------------|
| **Substitutions** | ~99% of errors | ~30% of errors |
| **Insertions/Deletions** | ~1% of errors | ~70% of errors |
| **Homopolymer errors** | Rare | Common |

---

## Phasing and Pre-phasing

### The Synchronization Problem

In a perfect world, all molecules in a cluster stay perfectly synchronized - all at the same base position.

**Reality:** Some molecules lag behind (phasing) or jump ahead (pre-phasing).

### Phasing (Lagging Behind)

**Cycle 1:** All molecules at position 1 ✓  
**Cycle 2:** 98% at position 2, 2% still at position 1 (incomplete extension)  
**Cycle 3:** 96% at position 3, 4% behind...

As cycles progress, the cluster becomes a **mix of molecules at different positions**.

**Result:** Blurry signal - you're imaging multiple bases at once.

### Pre-phasing (Jumping Ahead)

**Cause:** Incomplete removal of terminator or dye

A molecule might:
- Have terminator removed
- BUT dye not fully removed
- Next cycle adds another base (now 2 bases ahead of schedule)

### Impact on Quality

**Early cycles (1-100):** High accuracy, minimal phasing  
**Middle cycles (100-200):** Good accuracy, some phasing  
**Late cycles (200-300+):** Lower accuracy, significant phasing

**Quality scores decline** with read length. This is why:
- Read 1 (first 150 bases) typically has higher quality than Read 2
- Paired-end reads are used (sequence both ends, higher quality at each end)

---

## Paired-End Sequencing

### What Is Paired-End?

Instead of sequencing only one direction, sequence **both ends** of the DNA fragment.

**Process:**

1. **Read 1:** Sequence from one end (forward direction) for 150 bases
2. **Regenerate clusters** (bridge amplification again)
3. **Read 2:** Sequence from the other end (reverse direction) for 150 bases

**Result:** Two reads from the same fragment, separated by a known distance.

### Why Paired-End?

**1. Better mapping**
- If one end maps ambiguously, the other might be unique
- Correct orientation and distance constrain mapping

**2. Detect structural variants**
- Deletions: Reads closer than expected
- Insertions: Reads farther than expected
- Inversions: Wrong orientation
- Translocations: Reads on different chromosomes

**3. Improve assembly**
- Links across repetitive regions
- Spans gaps

**4. Quality assurance**
- If paired reads don't map correctly, flag as problematic

---

## Illumina Systems

### Different Throughput Options

Illumina offers multiple sequencing platforms for different scales:

| System | Throughput | Run Time | Read Length | Best For |
|--------|------------|----------|-------------|----------|
| **iSeq 100** | 1.2 Gb | 9-19 hours | 150 bp | Small targeted panels, amplicons |
| **MiniSeq** | 8 Gb | 4-24 hours | 150 bp | Small labs, targeted sequencing |
| **MiSeq** | 15 Gb | 4-55 hours | 300 bp | Targeted panels, small genomes, amplicon seq |
| **NextSeq** | 120 Gb | 12-30 hours | 150 bp | Exomes, transcriptomes, small genomes |
| **NovaSeq** | 6000 Gb (6 Tb) | 13-44 hours | 250 bp | Whole genomes, large projects, population studies |

**Key trade-offs:**
- Higher throughput = longer run time
- Longer reads = lower throughput or longer run time
- Bigger machines = higher capital cost but lower cost per Gb

---

## Advantages of Illumina

**1. High Accuracy**
- 99.9% base accuracy (Q30 or higher)
- Few indel errors
- Reliable base calling

**2. High Throughput**
- Billions of reads per run
- Suitable for whole genomes at population scale

**3. Low Cost (at scale)**
- ~$5-10 per Gb for high-throughput systems
- Cheapest for large projects

**4. Mature Technology**
- Well-established protocols
- Extensive bioinformatics tools
- Large user community

**5. Flexible Read Lengths**
- 50 bp to 300 bp
- Single-end or paired-end

**6. Multiplexing**
- Sequence 96+ samples in one run using barcodes
- Reduces cost per sample

---

## Disadvantages of Illumina

**1. Short Reads**
- Maximum ~300 bp (vs. PacBio: 10-20 kb)
- Hard to resolve complex repeats
- Difficult for de novo assembly of large genomes

**2. Run Time**
- 12-44 hours for high-throughput systems
- Longer than Ion Torrent (2-4 hours)
- Not ideal for ultra-rapid diagnostics

**3. PCR Amplification Bias**
- Bridge amplification favors certain sequences
- GC-rich or AT-rich regions may be underrepresented
- Some sequences difficult to amplify

**4. Equipment Cost**
- NovaSeq: $850,000-$1,000,000
- High upfront investment
- Requires dedicated space and trained staff

**5. Phasing Issues**
- Quality degrades with read length
- Limits maximum usable read length

---

## When to Use Illumina

### Ideal Applications

**Whole Genome Sequencing (WGS)**
- Human, animal, plant genomes
- Resequencing (alignment to reference)
- Population genomics

**Whole Exome Sequencing (WES)**
- Capture and sequence only coding regions
- Clinical diagnostics
- Disease gene discovery

**RNA Sequencing (RNA-seq)**
- Gene expression profiling
- Transcript discovery
- Differential expression analysis

**ChIP-Seq / ATAC-Seq**
- Protein-DNA interactions
- Chromatin accessibility
- Epigenomics

**Metagenomics**
- Microbial community profiling
- 16S rRNA sequencing
- Shotgun metagenomics

**Targeted Panels**
- Cancer hotspot panels
- Carrier screening
- Pharmacogenomics

### Not Ideal For

**Long-range phasing** (use PacBio or Oxford Nanopore)  
**Structural variant detection** (short reads struggle with large rearrangements)  
**Ultra-rapid turnaround** (use Ion Torrent for speed)  
**De novo assembly of repeat-rich genomes** (long reads better)

---

## Illumina vs Ion Torrent: Summary

| Feature | Illumina | Ion Torrent |
|---------|----------|-------------|
| **Detection** | Fluorescence | pH (H⁺ ions) |
| **Chemistry** | Reversible terminators | Natural dNTPs + ddNTPs |
| **Read length** | 50-300 bp | 200-400 bp |
| **Run time** | 12-44 hours (high-throughput) | 2-4 hours |
| **Accuracy** | 99.9% | 98-99% |
| **Main error** | Substitutions | Indels (homopolymers) |
| **Homopolymers** | No problem | Major issue |
| **Throughput** | Up to 6 Tb (NovaSeq) | Up to 15 Gb |
| **Cost per Gb** | $5-10 (at scale) | $50-100 |
| **Best for** | Large projects, WGS, high accuracy | Targeted panels, speed |

---

## The Bottom Line

**Illumina is the workhorse of genomics.** It's not the fastest (Ion Torrent), not the longest reads (PacBio/Nanopore), but it hits the sweet spot of:
- High accuracy
- High throughput  
- Reasonable cost
- Mature ecosystem

For most genomic applications - especially resequencing, RNA-seq, and exomes - **Illumina is the default choice**.

The main limitation is **short reads**. For applications requiring long-range information (phasing variants, resolving repeats, de novo assembly), you'd combine Illumina with long-read technologies or use long-read platforms alone.

**Key takeaway:** Illumina's reversible terminator chemistry elegantly solves the homopolymer problem by ensuring exactly one base per cycle, trading speed (longer run time) for accuracy (99.9%).

---

## About Course Materials
   
**These notes contain NO copied course materials.** Everything here is my personal understanding and recitation of concepts, synthesized from publicly available resources (Illumina documentation, sequencing technology literature, bioinformatics tutorials).

This is my academic work—how I've processed and reorganized information from legitimate sources. I take full responsibility for any errors in my understanding.

**If you believe any content violates copyright, contact me at mahmoudahmedxyz@gmail.com and I'll remove it immediately.**
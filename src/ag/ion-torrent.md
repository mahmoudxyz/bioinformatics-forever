# Next-Generation Sequencing (NGS)

## Ion Torrent Sequencing

Ion Torrent is a next-generation sequencing technology that detects DNA sequences by measuring **pH changes** instead of using light or fluorescence. It's fast, relatively cheap, and doesn't require expensive optical systems.

---

## The Chemistry: Detecting Hydrogen Ions

### The Core Principle

When DNA polymerase adds a nucleotide to a growing DNA strand, it releases a **hydrogen ion (H⁺)**.

**The reaction:**
```
dNTP + DNA(n) → DNA(n+1) + PPi + H⁺
```

- DNA polymerase incorporates a nucleotide
- Pyrophosphate (PPi) is released
- **One H⁺ ion is released per nucleotide added**
- The H⁺ changes the pH of the solution
- A pH sensor detects this change

**Key insight:** No fluorescent labels, no lasers, no cameras. Just chemistry and pH sensors.


**Why amplification?** A single molecule releasing one H⁺ isn't detectable. A million copies releasing a million H⁺ ions at once creates a measurable pH change.


## The Homopolymer Problem

### What Are Homopolymers?

A **homopolymer** is a stretch of identical nucleotides in a row:
- `AAAA` (4 A's)
- `TTTTTT` (6 T's)
- `GGGGG` (5 G's)

### Why They're a Problem in Ion Torrent

**Normal case (single nucleotide):**
- Flow A → 1 nucleotide added → 1 H⁺ released → small pH change → signal = 1

**Homopolymer case (multiple identical nucleotides):**
- Flow A → 4 nucleotides added (AAAA) → 4 H⁺ released → larger pH change → signal = 4

**The challenge:** Distinguishing between signal strengths. Is it 3 A's or 4 A's? Is it 7 T's or 8 T's?

### The Math Problem

Signal intensity is proportional to the number of nucleotides incorporated:
- 1 nucleotide = signal intensity ~100
- 2 nucleotides = signal intensity ~200
- 3 nucleotides = signal intensity ~300
- ...but measurements have noise

**Example measurements:**
- True 3 A's might measure as 290-310
- True 4 A's might measure as 390-410
- **Overlap zone:** Is a signal of 305 actually 3 or 4?


**The longer the homopolymer, the harder it is to count accurately.**

**Consequences:**
- **Insertions/deletions** (indels) in homopolymer regions
- **Frameshifts** if in coding regions (completely changes protein)
- **False variants** called in genetic studies
- **Harder genome assembly** (ambiguous regions)

Here's a concise section on Ion Torrent systems:

---

## Ion Torrent Systems

Ion Torrent offers different sequencing systems optimized for various throughput needs.

### System Comparison

| Feature | Ion PGM | Ion Proton/S5 |
|---------|---------|---------------|
| **Throughput** | 30 Mb - 2 Gb | Up to 15 Gb |
| **Run time** | 4-7 hours | 2-4 hours |
| **Read length** | 35-400 bp | 200 bp |
| **Best for** | Small targeted panels, single samples | Exomes, large panels, multiple samples |
| **Cost per run** | Lower | Higher |
| **Lab space** | Benchtop | Benchtop |


## Advantages of Ion Torrent

**1. Speed**
- No optical scanning between cycles
- Direct electronic detection
- Runs complete in 2-4 hours (vs. days for some platforms)

**2. Cost**
- No expensive lasers or cameras
- Simpler hardware = lower instrument cost
- Good for small labs or targeted sequencing

**3. Scalability**
- Different chip sizes for different throughput needs
- Can sequence 1 sample or 96 samples
- Good for clinical applications

**4. Long reads (relatively)**
- 200-400 bp reads standard
- Longer than Illumina (75-300 bp typically)
- Helpful for some applications

---

## Disadvantages of Ion Torrent

**1. Homopolymer errors** (the big one)
- Indel errors in long homopolymers
- Limits accuracy for some applications

**2. Lower overall accuracy**
- ~98-99% accuracy vs. 99.9% for Illumina
- More errors per base overall

**3. Smaller throughput**
- Maximum output: ~15 Gb per run
- Illumina NovaSeq: up to 6 Tb per run
- Not ideal for whole genome sequencing of complex organisms

**4. Systematic errors**
- Errors aren't random - they cluster in homopolymers
- Harder to correct computationally


## Conclusion

Ion Torrent is a clever technology that trades optical complexity for electronic simplicity. It's fast and cost-effective for targeted applications, but the homopolymer problem remains its Achilles' heel.

**The homopolymer issue isn't a deal-breaker** - it's manageable with proper bioinformatics and sufficient coverage. But you need to know about it when designing experiments and interpreting results.

For clinical targeted sequencing (like cancer panels), Ion Torrent is excellent. For reference-quality genome assemblies or ultra-high-accuracy applications, other platforms might be better choices.

**The key lesson:** Every sequencing technology has trade-offs. Understanding them helps you choose the right tool for your specific question.

---

## About Course Materials

**These notes contain NO copied course materials.** Everything here is my personal understanding and recitation of concepts, synthesized from publicly available resources (sequencing technology documentation, bioinformatics tutorials, scientific literature).

This is my academic work—how I've processed and reorganized information from legitimate sources. I take full responsibility for any errors in my understanding.

**If you believe any content violates copyright, contact me at mahmoudahmedxyz@gmail.com and I'll remove it immediately.**
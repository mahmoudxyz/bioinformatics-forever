# ABI SOLiD Sequencing (Historical)

## What Was SOLiD?

**SOLiD** (Sequencing by Oligonucleotide Ligation and Detection) was a next-generation sequencing platform developed by Applied Biosystems (later acquired by Life Technologies, then Thermo Fisher).

**Status:** Essentially discontinued. Replaced by Ion Torrent and other technologies.

---

## The Key Difference: Ligation Instead of Synthesis

Unlike other NGS platforms:
- **Illumina:** Sequencing by synthesis (polymerase adds nucleotides)
- **Ion Torrent:** Sequencing by synthesis (polymerase adds nucleotides)
- **SOLiD:** Sequencing by **ligation** (ligase joins short probes)

### How It Worked (Simplified)

1. **DNA fragments** attached to beads (emulsion PCR, like Ion Torrent)
2. **Fluorescent probes** (short 8-base oligonucleotides) compete to bind
3. **DNA ligase** joins the matching probe to the primer
4. **Detect fluorescence** to identify which probe bound
5. **Cleave probe**, move to next position
6. **Repeat** with different primers to read the sequence

**Key concept:** Instead of building a complementary strand one nucleotide at a time, SOLiD interrogated the sequence using short probes that bind and get ligated.

## Why It's Dead (or Nearly Dead)

**Advantages that didn't matter enough:**
- Very high accuracy (>99.9% after two-base encoding)
- Error detection built into chemistry

**Fatal disadvantages:**
1. **Complex bioinformatics** - two-base encoding required specialized tools
2. **Long run times** - 7-14 days per run (vs. hours for Ion Torrent, 1-2 days for Illumina)
3. **Expensive** - high cost per base
4. **Company pivot** - Life Technologies acquired Ion Torrent and shifted focus there

**The market chose:** Illumina won on simplicity and throughput, Ion Torrent won on speed.



## What You Should Remember

**1. Different chemistry** - Ligation-based, not synthesis-based

**2. Two-base encoding** - Clever error-checking mechanism, but added complexity

**3. Historical importance** - Showed alternative approaches to NGS were possible

**4. Why it failed** - Too slow, too complex, company shifted to Ion Torrent

**5. Legacy** - Some older papers used SOLiD data; understanding the platform helps interpret those results

---


## The Bottom Line

SOLiD was an interesting experiment in using **ligation chemistry** for sequencing. It achieved high accuracy through two-base encoding but couldn't compete with faster, simpler platforms.

**Why learn about it?**
- Understand the diversity of approaches to NGS
- Interpret older literature that used SOLiD
- Appreciate why chemistry simplicity matters (Illumina's success)

**You won't use it**, but knowing it existed helps you understand the evolution of sequencing technologies and why certain platforms won the market.

---


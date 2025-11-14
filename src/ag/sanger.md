# Sanger Sequencing

## The Chemistry: dNTPs vs ddNTPs

**dNTP (deoxynucleotide triphosphate):**
- Normal DNA building blocks: dATP, dCTP, dGTP, dTTP
- Have a 3'-OH group → DNA polymerase can add another nucleotide
- Chain **continues** growing

**ddNTP (dideoxynucleotide triphosphate):**
- Modified nucleotides: ddATP, ddCTP, ddGTP, ddTTP
- **Missing** the 3'-OH group → no place to attach next nucleotide
- Chain **terminates** (stops growing)

**The key idea:** Mix normal dNTPs with a small amount of ddNTPs. Sometimes the polymerase adds a normal dNTP (chain continues), sometimes it adds a ddNTP (chain stops). This creates DNA fragments of different lengths, all ending at the same type of base.

---

## The Classic Method: Four Separate Reactions

You set up **four tubes**, each with:
- Template DNA (what you want to sequence)
- Primer (starting point)
- DNA polymerase
- All four dNTPs (A, C, G, T)
- **One type of ddNTP** (different for each tube)

### The Four Reactions:

**Tube 1 - ddATP:** Chains terminate at every A position
**Tube 2 - ddCTP:** Chains terminate at every C position  
**Tube 3 - ddGTP:** Chains terminate at every G position  
**Tube 4 - ddTTP:** Chains terminate at every T position

### Example Results:

Let's say the template sequence is: `5'-ACGTACGT-3'`

**Tube A (ddATP):** Fragments ending at A positions
```
A
ACGTA
ACGTACGTA
```

**Tube C (ddCTP):** Fragments ending at C positions
```
AC
ACGTAC
```

**Tube G (ddGTP):** Fragments ending at G positions
```
ACG
ACGTACG
```

**Tube T (ddTTP):** Fragments ending at T positions
```
ACGT
ACGTACGT
```

---

## Gel Electrophoresis Separation

Run all four samples on a gel. Smallest fragments move furthest, largest stay near the top.

```
        A    C    G    T
        |    |    |    |
Start → ━━━━━━━━━━━━━━━━  (loading wells)
        
        |              |  ← ACGT (8 bases)
        |    |         |  ← ACGTACG (7 bases)
        |              |  ← ACGTAC (6 bases) 
        |         |    |  ← ACGTA (5 bases)
        |         |    |  ← ACGT (4 bases)
        |    |    |    |  ← ACG (3 bases)
        |    |         |  ← AC (2 bases)
        |              |  ← A (1 base)
        
      ↓ Direction of migration ↓
```

**Reading the sequence:** Start from the bottom (smallest fragment) and go up:
```
Bottom → Top:  A - C - G - T - A - C - G - T
Sequence:      A   C   G   T   A   C   G   T
```

The sequence is **ACGTACGT** (read from bottom to top).

---

## Modern Method: Fluorescent Dyes

Instead of four separate tubes, we now use **one tube** with four different fluorescent ddNTPs:

- ddATP = **Green** fluorescence
- ddCTP = **Blue** fluorescence  
- ddGTP = **Yellow** fluorescence
- ddTTP = **Red** fluorescence

**What happens:**
1. All fragments are created in one tube
2. Run them through a capillary (tiny tube) instead of a gel
3. Laser detects fragments as they pass by
4. Computer records the color (= which base) and timing (= fragment size)

**Chromatogram output:**

```
Fluorescence
    ↑
    |     G  C     T     A  G  C  T
    |    /\  /\   /\    /\ /\ /\ /\
    |___/  \/  \_/  \__/  X  \/  \_____→ Time
    |                     / \
Position: 1  2  3  4  5  6  7  8
```

The computer reads the peaks and outputs: **GCTAGCT**

---

## Why Sanger Sequencing Still Matters

- **High accuracy** (~99.9%)
- **Gold standard** for validating variants
- **Good for short reads** (up to ~800 bases)
- **Single-molecule sequencing** - no PCR bias
- **Used for:** Confirming mutations, plasmid verification, PCR product sequencing

**Limitations:**
- **One fragment at a time** (not high-throughput)
- **Expensive** for large-scale projects (replaced by next-gen sequencing)
- **Can't detect low-frequency variants** (< 15-20%)

---

## About Course Materials

**These notes contain NO copied course materials.** Everything here is my personal understanding and recitation of concepts, synthesized from publicly available resources (textbooks, online tutorials, sequencing method documentation).

This is my academic work—how I've processed and reorganized information from legitimate sources. I take full responsibility for any errors in my understanding.

**If you believe any content violates copyright, contact me at mahmoudahmedxyz@gmail.com and I'll remove it immediately.**
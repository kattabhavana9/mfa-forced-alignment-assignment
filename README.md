# Montreal Forced Aligner (MFA) – Forced Alignment Assignment

## Overview
This project demonstrates the complete setup and usage of **Montreal Forced Aligner (MFA)** to perform **word-level and phoneme-level forced alignment** on English speech data.  
The alignment outputs are generated as **TextGrid** files and inspected using **Praat**.

---

## 1. Environment Setup

- **Operating System**: Windows 10 / 11  
- **MFA Version**: 3.3.9  
- **Environment Manager**: Conda / Micromamba  

Installation

```bash
conda create -n mfa -c conda-forge montreal-forced-aligner
conda activate mfa
mfa version
```

## 2. Dataset Preparation

### MFA Directory Structure

```text
data/
└── speaker1/
    ├── F2BJRL1.wav
    ├── F2BJRL1.txt
    ├── F2BJRL2.wav
    ├── F2BJRL2.txt
    ├── F2BJRL3.wav
    ├── F2BJRL3.txt
    ├── ISLE_SESS0131_BLOCKD02_01_sprt1.wav
    └── ISLE_SESS0131_BLOCKD02_01_sprt1.txt
### Notes
- Each `.wav` file has a matching `.txt` transcript  
- Transcripts contain **uppercase English text**  
- One speaker per folder (required by MFA)
```

## 3. Pronunciation Dictionary Selection

- **Dictionary used**: `english_us_arpa`  
- **Acoustic model**: `english_us_arpa`

### Download Pretrained Models

```bash
mfa model download dictionary english_us_arpa
mfa model download acoustic english_us_arpa
```
## 4. Forced Alignment Execution

```bash
mfa align data english_us_arpa english_us_arpa output --clean

### This step performs:
- MFCC feature extraction  
- Graph compilation  
- First-pass alignment  
- Final alignment  
- TextGrid generation  
```

## 5. Output Files

```text
output/
└── speaker1/
    ├── F2BJRL1.TextGrid
    ├── F2BJRL2.TextGrid
    ├── F2BJRL3.TextGrid
    └── ISLE_SESS0131_BLOCKD02_01_sprt1.TextGrid
### Each TextGrid contains:
- **Word tier**
- **Phoneme tier**
```

## 6. Alignment Inspection Using Praat

### Steps
1. Open **Praat**
2. Load the `.wav` file and the corresponding `.TextGrid`
3. Select both files and click **View & Edit**

### Observations
- Word boundaries align well with spoken segments  
- Phoneme segmentation follows **ARPAbet** labels  
- Minor timing offsets were observed in fast speech regions  

Screenshots of alignment visualization are included for reference.


## 7. Handling Out-of-Vocabulary (OOV) Words

### Problem
Some words (e.g., rare words or proper nouns) were not present in the default pronunciation dictionary.

### Solution
The `--ignore_oov` flag was used to allow alignment to continue without failure:

```bash
mfa align data english_us_arpa english_us_arpa output --ignore_oov --clean
```
### Result
- Alignment completed successfully  
- OOV words were skipped but logged  
- Overall alignment quality improved  



## Key Observations
- Pretrained MFA English models work well for clean speech  
- Word-level alignments are more stable than phoneme-level alignments  
- Handling OOV words is essential for real-world datasets  
- Manual inspection using Praat is crucial for validation  

---

## Tools Used
- Montreal Forced Aligner (MFA)  
- Praat (TextGrid visualization)  
- Conda / Micromamba  


# Dataset Description
> ⚠️ Do NOT upload raw datasets to this repository. All datasets listed here are either publicly available online or synthetically generated. Follow the download instructions below to set up the `data/` folder locally.

---

## Dataset 1: Davidson Hate Speech & Offensive Language Dataset

**Dataset Name:** Davidson Hate Speech and Offensive Language Dataset

**Source:**
https://github.com/t-davidson/hate-speech-and-offensive-language/tree/master/data

**Description:**
Contains tweets collected from Twitter, annotated by crowdworkers for hate speech, offensive language, and neutral content. Each tweet is labeled by multiple annotators and assigned a final class by majority vote. This dataset serves as the **primary training corpus** for the hate speech detection model in Phase 1 of the pipeline.

**Number of instances:** 24,783 (after deduplication)
**Number of attributes:** 7 (count, hate_speech votes, offensive_language votes, neither votes, class label, tweet text, and cleaned text)

**Class distribution:**
- `0` — Hate Speech (~5.8%)
- `1` — Offensive Language (~77.4%)
- `2` — Neither (~16.8%)

**How to download:**
```bash
# From the project root directory
mkdir -p data/davidson
wget -O data/davidson/labeled_data.csv \
  https://raw.githubusercontent.com/t-davidson/hate-speech-and-offensive-language/master/data/labeled_data.csv
```
Or run the first cell of the notebook — it loads the dataset directly via URL automatically.

**Screenshot of dataset structure:**

| count | hate_speech | offensive_language | neither | class | tweet |
|-------|-------------|-------------------|---------|-------|-------|
| 3     | 0           | 0                 | 3       | 2     | RT @user: Safe message... |
| 3     | 1           | 2                 | 0       | 1     | offensive text... |
| 3     | 3           | 0                 | 0       | 0     | hate speech text... |

---

## Dataset 2: ETHOS — Online Hate Speech Detection Dataset

**Dataset Name:** ETHOS: Multi-Label Hate Speech Detection Dataset

**Source:**
https://github.com/intelligence-csd-auth-gr/Ethos-Hate-Speech-Dataset

**Description:**
Contains 998 YouTube and Reddit comments manually annotated for hate speech. Includes binary (hate/not-hate) and multiclass (racism, sexism, etc.) labels. Used during the **domain fine-tuning phase** to adapt the model toward social-media-style healthcare discourse found in OHCs.

**Number of instances:** 998
**Number of attributes:** 2 (comment text, isHate label)

**How to download:**
```bash
mkdir -p data/ethos
wget -O data/ethos/Ethos_Dataset_Binary.csv \
  https://raw.githubusercontent.com/intelligence-csd-auth-gr/Ethos-Hate-Speech-Dataset/master/ethos/ethos_data/Ethos_Dataset_Binary_Form.csv
```

**Screenshot of dataset structure:**

| comment | isHate |
|---------|--------|
| "You people are disgusting..." | 1 |
| "I hope everyone stays safe..." | 0 |

---

## Dataset 3: Synthea Synthetic Electronic Health Records (EHR)

**Dataset Name:** Synthea — Synthetic Patient Population Simulator

**Source:**
https://synthea.mitre.org/downloads

**Description:**
Synthetically generated Electronic Health Records (EHRs) that statistically mirror real patient data without containing any actual patient information. Compliant with the FHIR standard. Used for **clinical outcome simulation and patient journey mapping** in the project. No real patient data is used anywhere in this pipeline.

**Number of instances:** Configurable (default export: ~1,000 synthetic patients)
**Number of attributes:** Varies by export format (CSV exports include demographics, conditions, medications, encounters, observations)

**How to download:**
```bash
# Option 1: Download pre-generated CSVs directly
mkdir -p data/synthea
# Visit https://synthea.mitre.org/downloads and download the CSV package
# Place all CSV files inside data/synthea/

# Option 2: Generate locally using the Synthea JAR
wget https://github.com/synthetichealth/synthea/releases/download/master-branch-latest/synthea-with-dependencies.jar
java -jar synthea-with-dependencies.jar -p 1000   # generates 1000 patients
mv output/csv/* data/synthea/
```

**Screenshot of dataset structure (patients.csv):**

| Id | BIRTHDATE | GENDER | RACE | ETHNICITY | CITY | STATE | INCOME |
|----|-----------|--------|------|-----------|------|-------|--------|
| abc-123 | 1980-04-12 | M | white | nonhispanic | Boston | MA | 75000 |

---

## Dataset 4: MetaHate (Supplementary Pre-training)

**Dataset Name:** MetaHate — A Unified Meta-Collection for Hate Speech Detection

**Source:**
https://arxiv.org/abs/2401.06526

**Description:**
A comprehensive meta-collection aggregating 36 existing hate speech datasets into a single unified corpus of 1,226,202 non-duplicated social media comments with binary (hate / no-hate) labels. Used for supplementary **model pre-training** to improve generalization before fine-tuning on OHC-specific data.

**Number of instances:** 1,226,202
**Number of attributes:** 2 (text, label)

**How to download:**
Follow the instructions in the official paper repository:
https://github.com/HimariO/MetaHate *(check repo for access and license terms)*

> ⚠️ Due to its size (~1.2M records), this dataset must **not** be committed to the repository. Store locally in `data/metahate/` and add to `.gitignore`.

---

## Dataset 5: VADER Sentiment Corpus

**Dataset Name:** VADER — Valence Aware Dictionary and sEntiment Reasoner

**Source:**
https://github.com/cjhutto/vaderSentiment

**Description:**
A lexicon and rule-based sentiment analysis tool specifically designed for social media text. Used to generate the `SentimentScore` feature during the feature engineering phase and for training the **positive content recommendation module**.

**Number of instances:** 7,500+ gold-standard labeled sentences
**Number of attributes:** 2 (text, compound sentiment score)

**How to download:**
```bash
# Installed automatically via pip — no manual download needed
pip install vaderSentiment

# The lexicon is bundled with the package
python -c "from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer; print('VADER ready ✅')"
```

---

## Folder Structure

Once all datasets are downloaded, your `data/` directory should look like this:

```
data/
├── dataset_description.md       ← this file
├── davidson/
│   └── labeled_data.csv
├── ethos/
│   └── Ethos_Dataset_Binary.csv
├── synthea/
│   ├── patients.csv
│   ├── conditions.csv
│   ├── medications.csv
│   └── encounters.csv
└── metahate/                    ← in .gitignore (too large)
    └── metahate_combined.csv
```

---

## .gitignore Entry

Add the following to your `.gitignore` to prevent large datasets from being committed:

```
# Datasets — download locally using instructions in dataset_description.md
data/davidson/
data/ethos/
data/synthea/
data/metahate/
*.csv
*.tsv
*.json
!data/dataset_description.md
```

---

*Group 17 | Data Warehouse & Mining Project | 2025–2026*
*Sidhaarth Mohandas · Kunamalla Tharun · Venkat Chiranjeevi Reddy · Gangireddy Vishnu Vardhan Reddy*

# Module 1: Job-Market Data and Skill Vocabulary

**Project:** AI-Powered Career Intelligence and Adaptive Skill-Gap
Analysis Platform for Students

## 1. Introduction

Module 1 collects, processes, and organizes job-market information to
build a standardized skill vocabulary for selected career roles. It
combines O\*NET, ESCO, and Kaggle job-posting data to identify
occupational skills, technology mentions, alternative skill labels, and
employer-mentioned skills.

## 2. Objectives

-   Collect job-market and occupational information from reliable
    datasets.
-   Process O\*NET and ESCO skill information.
-   Build a unified skill vocabulary while preserving source
    identifiers.
-   Identify alternative labels and aliases for skills.
-   Extract skills mentioned in job descriptions.
-   Generate preliminary skill profiles for five career roles.
-   Prepare a gold-set candidate dataset for human validation.

## 3. Datasets

### 3.1 O\*NET 31.0

Six O\*NET files were processed:

  Dataset                 Records
  --------------------- ---------
  Occupation Data           1,016
  Software Skills          31,821
  Essential Skills         18,200
  Knowledge                60,060
  Job Titles               54,269
  Transferable Skills      45,500

O\*NET supplied occupation information, software technologies, essential
skills, knowledge areas, and transferable skills.

### 3.2 ESCO 1.2.1

  Measure                             Result
  -------------------------------- ---------
  Source skill rows                   13,960
  Unique skill concepts               13,939
  Occupations                          3,043
  Occupation-skill relationships     126,051

ESCO supplied standardized skill concepts, preferred labels, alternative
labels, hidden labels, descriptions, and occupation-skill relationships.

### 3.3 Kaggle 2024 Job Postings

  Measure                                   Result
  -------------------------------------- ---------
  Original job postings                    123,849
  Columns                                       31
  Duplicate job IDs                              0
  Missing descriptions before cleaning           7
  Records after cleaning                   123,849

Selected missing text values were replaced with empty strings. No
postings were removed during this cleaning step. Job descriptions were
used as the main source for skill extraction.

## 4. Methodology

### 4.1 Data collection and cleaning

The datasets were loaded from CSV files in Google Colab using Python and
Pandas. Dataset dimensions, columns, missing values, and duplicate
records were inspected. Selected text columns in the Kaggle dataset were
cleaned, and processed data was saved to Google Drive.

### 4.2 Unified skill vocabulary

The O\*NET and ESCO vocabularies were combined without automatically
merging similar-looking concepts. Their original identifiers were
retained.

  Source                             Concepts
  ------------------------------ ------------
  O\*NET                                  618
  ESCO                                 13,939
  **Combined source concepts**     **14,557**

Output: `combined_skill_vocabulary.csv`

### 4.3 Alias and alternative-label mapping

ESCO alternative labels and hidden labels were extracted and linked to
their canonical skill concepts. Five manually reviewed O\*NET aliases
were also included:

-   JS → JavaScript
-   K8s → Kubernetes
-   TF → TensorFlow
-   Py → Python
-   SQL → Structured query language SQL

  Source                     Alias records
  ------------------------ ---------------
  ESCO supplied labels              86,978
  O\*NET manual mappings                 5
  **Total**                     **86,983**

Output: `skill_alias_mapping.csv`

ESCO-supplied labels are source-provided mappings; they have not each
been independently checked by a human reviewer.

### 4.4 Job-posting classification by role

Job titles were matched using Python regular expressions. Five target
roles were selected:

  Career role               Candidate postings
  ----------------------- --------------------
  Software Engineer                      1,614
  Data Analyst                             445
  AI Engineer                              142
  Cybersecurity Analyst                     60
  Full Stack Developer                     234
  **Total**                          **2,495**

Output: `role_specific_job_postings.csv`

These are automated candidate matches. Some titles may not exactly fit
the assigned role and should be checked before final use.

### 4.5 Skill extraction

The 618-skill O\*NET software vocabulary was used to identify mentions
in job descriptions. Python and FlashText were used for keyword
matching. Duplicate job-skill matches were removed, and skill
frequencies and prevalence percentages were calculated for each role.

The prevalence calculation was:

**Skill prevalence (%) = (Number of role postings mentioning the skill ÷
Total postings for that role) × 100**

Results:

-   Job-skill matches: 7,361
-   Unique extracted skills: 181
-   Roles analyzed: 5

Outputs: - `job_posting_skill_matches.csv` -
`preliminary_role_skill_profiles.csv`

### 4.6 Gold-set candidate preparation

Twenty postings were sampled from each role using a fixed random seed,
producing 100 candidate records. The file includes job details,
automatically extracted candidate skills, and fields for a human
reviewer to confirm relevant skills, remove irrelevant skills, add
missing skills, and record comments.

Output: `gold_set_for_manual_review.csv`

All 100 records are currently marked **Pending manual review**. This is
a review-ready candidate set, not yet an expert-labelled gold standard.

## 5. Results and observations

The most frequently detected skills in each role included:

### AI Engineer

  Skill            Postings mentioning skill   Prevalence
  -------------- --------------------------- ------------
  Python                                 111       78.17%
  TensorFlow                              61       42.96%
  PyTorch                                 56       39.44%
  C++                                     25       17.61%
  Scikit-learn                            25       17.61%

### Cybersecurity Analyst

  Skill      Postings mentioning skill   Prevalence
  -------- --------------------------- ------------
  Python                             9       15.00%
  Linux                              8       13.33%
  Bash                               3        5.00%
  UNIX                               3        5.00%
  VMware                             3        5.00%

### Data Analyst

  Skill          Postings mentioning skill   Prevalence
  ------------ --------------------------- ------------
  Python                               154       34.61%
  Tableau                              103       23.15%
  JavaScript                            46       10.34%
  C++                                   41        9.21%
  SAS                                   33        7.42%

### Full Stack Developer

  Skill          Postings mentioning skill   Prevalence
  ------------ --------------------------- ------------
  React                                114       48.72%
  JavaScript                           106       45.30%
  Python                                60       25.64%
  TypeScript                            58       24.79%
  Git                                   53       22.65%

### Software Engineer

  Skill          Postings mentioning skill   Prevalence
  ------------ --------------------------- ------------
  Python                               614       38.04%
  JavaScript                           505       31.29%
  C++                                  386       23.92%
  Kubernetes                           284       17.60%
  Docker                               278       17.22%

These results illustrate different skill patterns across career roles.
For example, Python was frequently mentioned in AI Engineer and Software
Engineer postings, while React and JavaScript were prominent in Full
Stack Developer postings.

## 6. Tools and technologies

  Tool or source        Purpose
  --------------------- --------------------------------------------------
  Python                Programming and data processing
  Google Colab          Development environment
  Google Drive          Dataset storage
  Pandas                CSV loading, cleaning, and analysis
  Regular expressions   Job-title matching
  FlashText             Fast skill keyword extraction
  O\*NET                Occupational and software-skill information
  ESCO                  Standardized skills and occupation relationships
  Kaggle                Job-posting dataset source

## 7. Limitations and validation needs

1.  Automated title matching may include postings that do not exactly
    fit the selected role.
2.  O\*NET and ESCO use different identifiers and skill structures.
3.  Similar skill concepts have not been semantically merged.
4.  Keyword extraction can produce false positives and miss skills
    expressed using other wording.
5.  Some detected terms may not be relevant technical skills and need
    filtering.
6.  ESCO aliases have not all been independently reviewed.
7.  Role skill profiles are preliminary and require quality review.
8.  The 100 gold-set records require human annotation before they can be
    called an expert-validated gold set.
9.  Some O\*NET occupation mappings are approximate proxies for project
    roles.

## 8. Conclusion

Module 1 established the initial data foundation for the AI-Powered
Career Intelligence and Adaptive Skill-Gap Analysis Platform. O\*NET,
ESCO, and Kaggle datasets were processed and organized. A unified
vocabulary containing 14,557 source concepts and an alias mapping
containing 86,983 records were generated.

A total of 2,495 candidate job postings were identified across five
career roles. Automated extraction detected 181 unique skills and
generated preliminary role-specific skill profiles. A 100-record
gold-set candidate file was also prepared for human review.

These outputs provide a foundation for later modules involving student
skill assessment, job-role matching, and adaptive skill-gap analysis.
Human validation and quality review remain necessary before treating the
generated profiles and gold set as final validated data.

## 9. Module 1 output files

The following outputs were generated in
`Career_Intelligence_Project/Module_1/`:

  ---------------------------------------------------------------------------
  File                                    Description
  --------------------------------------- -----------------------------------
  `normalized_skill_vocabulary.csv`       Initial O\*NET vocabulary

  `esco_skill_vocabulary.csv`             ESCO skill vocabulary

  `esco_occupation_skill_relations.csv`   ESCO occupation-skill relationships

  `combined_skill_vocabulary.csv`         Unified source vocabulary

  `skill_alias_mapping.csv`               Alias and alternative-label
                                          mappings

  `role_specific_job_postings.csv`        Candidate postings classified by
                                          role

  `job_posting_skill_matches.csv`         Extracted job-skill matches

  `preliminary_role_skill_profiles.csv`   Preliminary role skill profiles

  `gold_set_for_manual_review.csv`        Gold-set candidate records
  ---------------------------------------------------------------------------

**End of Module 1 Report**

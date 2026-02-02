# Scientific Reproducibility: Best Practices and Implementation

## Overview

This training module provides a comprehensive introduction to scientific reproducibility, covering fundamental concepts, best practices, and practical implementation strategies. Whether you're a graduate student beginning your research journey or an established researcher looking to enhance your workflow, this guide will help you create more reliable, transparent, and reproducible research.

**Duration:** Approximately 60 minutes  
**Level:** Beginner to Intermediate  
**Prerequisites:** Basic understanding of research methodology

---

## Table of Contents

1. [Introduction to Scientific Reproducibility](#introduction)
2. [Key Concepts and Why It Matters](#key-concepts)
3. [Best Practices Overview](#best-practices)
4. [Version Control with Git](#version-control)
5. [Building Conda Environments](#conda-environments)
6. [Data Management Essentials](#data-management)
7. [Documentation Standards](#documentation)
8. [Hands-on Exercises](#exercises)
9. [Resources and Further Reading](#resources)

---

## 1. Introduction to Scientific Reproducibility {#introduction}

Scientific reproducibility is the ability of independent researchers to obtain consistent results using the same methods, data, and analysis procedures as the original study. It is a cornerstone of the scientific method and essential for validating research findings.

### Learning Objectives

By the end of this training, you will be able to:

- Understand the difference between reproducibility, replicability, and repeatability
- Identify common reproducibility challenges in research
- Implement version control systems for code and documents
- Create well-documented, reproducible computational workflows
- Organize and manage research data effectively
- Write clear documentation for your research methods

---

## 2. Key Concepts and Why It Matters {#key-concepts}

### Essential Terminology

**Reproducibility**: The ability to achieve **consistent results** using the **same data** and **same methods** as the original study.
- *Example*: Another researcher downloads your data and code, runs your analysis script, and obtains the same results.

**Replicability**: The ability to achieve **consistent results** across studies using **new data** but **similar methods**.
- *Example*: A different research team conducts a similar experiment with newly collected data and finds comparable results.

**Repeatability**: The ability of the **same researcher** to achieve **consistent results** using the **same methods** on the **same data** at different times.
- *Example*: You re-run your analysis six months later and obtain identical results.

### Why Reproducibility Matters

**The Reproducibility Crisis**: Recent studies show that 50-90% of published research in some fields cannot be reproduced. Major findings:
- Psychology: Only 36-47% of 100 studies successfully replicated (Open Science Collaboration, 2015)
- Biomedicine: Only 11% of 53 landmark cancer studies replicated (Begley & Ellis, 2012)
- Economics: Only 49% of papers had sufficient data/code for reproduction (Chang & Li, 2015)

**Real-World Impact**:
- **Medical Research**: Clinical decisions based on unreproducible studies can harm patients
- **Policy Decisions**: Climate and economic policies depend on reproducible models
- **Scientific Progress**: Reproducible research allows others to build on and verify your work
- **Career Benefits**: Reproducible research is increasingly valued by funders and journals

### Common Barriers

1. Insufficient documentation of methods
2. Data and code not shared
3. Software versions not documented
4. Random seeds not set
5. Hardware-dependent results
6. Absolute file paths in code

---

## 3. Best Practices Overview {#best-practices}

### The Reproducibility Checklist

#### Project Setup
- [ ] Create a version control repository (Git)
- [ ] Establish consistent file organization
- [ ] Document software environment (requirements.txt or environment.yml)
- [ ] Create README with project description

#### During Research
- [ ] Write clear, commented code
- [ ] Use relative file paths (not absolute)
- [ ] Set random seeds for stochastic processes
- [ ] Document all data transformations
- [ ] Commit changes regularly to version control

#### Project Completion
- [ ] Test reproducibility (have someone else run your code)
- [ ] Include environment specifications
- [ ] Create comprehensive README with instructions
- [ ] Share data and code in repositories
- [ ] Document limitations and dependencies

### Key Principles: FAIR Data

- **F**indable: Use persistent identifiers and metadata
- **A**ccessible: Store in openly accessible repositories
- **I**nteroperable: Use standard formats
- **R**eusable: Include clear licensing and documentation

---

## 4. Version Control with Git {#version-control}

### Why Version Control?

- Track changes to files over time
- Revert to previous versions if needed
- Collaborate effectively
- Maintain complete project history
- Experiment without breaking working code

### Essential Git Workflow

```bash
# Initialize a new repository
mkdir my_research_project
cd my_research_project
git init

# Create and commit README
echo "# My Research Project" > README.md
git add README.md
git commit -m "Initial commit: Add README"

# Regular workflow
git status                    # Check what's changed
git add filename.py           # Stage specific files
git add .                     # Stage all changes
git commit -m "Add analysis"  # Commit with message
git log --oneline            # View history

# Branching for experiments
git checkout -b new-feature   # Create and switch to branch
# ... make changes ...
git checkout main            # Switch back to main
git merge new-feature        # Merge changes
```

### Writing Good Commit Messages

**Bad examples**: "Fixed stuff", "Update", "Changes"

**Good examples**: 
- "Fix: Correct calculation error in statistical analysis"
- "Add: Implement data validation function"
- "Update: Improve documentation for preprocessing"

### Project Structure

```
my_research_project/
├── README.md                 # Project overview
├── LICENSE                   # License information
├── environment.yml           # Conda environment
├── .gitignore               # Files to ignore
├── data/
│   ├── raw/                 # Original data (never modify)
│   ├── processed/           # Cleaned data
│   └── README.md            # Data documentation
├── src/                     # Source code
│   ├── preprocessing.py
│   ├── analysis.py
│   └── visualization.py
├── notebooks/               # Jupyter notebooks
├── results/                 # Generated outputs
│   ├── figures/
│   └── tables/
└── docs/                    # Documentation
```

### Essential .gitignore

```gitignore
# Large data files
*.csv
*.h5
data/raw/*
data/processed/*

# Python
__pycache__/
*.pyc
.ipynb_checkpoints/

# Environments
.env
venv/
.venv/
conda_env/

# OS files
.DS_Store

# Results (can be regenerated)
results/figures/
results/tables/
```

---

## 5. Building Conda Environments {#conda-environments}

### Why Use Conda?

Conda is a package and environment management system that:
- Works across platforms (Windows, Mac, Linux)
- Manages both Python and non-Python dependencies
- Isolates project dependencies
- Ensures reproducibility across systems
- Handles complex dependency conflicts

### Creating a Conda Environment

#### Method 1: Command Line

```bash
# Create environment with specific Python version
conda create -n myproject python=3.10

# Activate the environment
conda activate myproject

# Install packages
conda install numpy pandas matplotlib scipy
conda install -c conda-forge scikit-learn

# Install from pip if needed
pip install some-package

# Deactivate when done
conda deactivate
```

#### Method 2: From environment.yml (Recommended)

Create an `environment.yml` file:

```yaml
name: myproject
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - python=3.10
  - numpy=1.24.3
  - pandas=2.0.2
  - matplotlib=3.7.1
  - scipy=1.11.1
  - scikit-learn=1.3.0
  - jupyter
  - notebook
  - pip
  - pip:
    - special-package==1.0.0
```

Build from file:
```bash
conda env create -f environment.yml
conda activate myproject
```

### Best Practices for Conda Environments

#### 1. Pin Important Versions

```yaml
dependencies:
  - python=3.10        # Major.minor (allows patches)
  - numpy=1.24.3       # Exact version for critical packages
  - pandas>=2.0,<3.0   # Range for flexibility
```

#### 2. Use Conda-Forge Channel

```yaml
channels:
  - conda-forge  # Community-maintained, up-to-date packages
  - defaults
```

#### 3. Order Matters for Channels

Channels are searched top to bottom. Put most preferred first:

```yaml
channels:
  - conda-forge  # Check here first
  - bioconda     # Then here (for bioinformatics)
  - defaults     # Then defaults
```

#### 4. Mixing Conda and Pip

Install conda packages first, then pip packages:

```yaml
dependencies:
  - python=3.10
  - numpy
  - pandas
  - pip:
    - package-only-on-pip
```

#### 5. Document Your Environment

Always export your working environment:

```bash
# Export to environment.yml
conda env export > environment.yml

# Export with only explicitly installed packages (cleaner)
conda env export --from-history > environment.yml

# Export to requirements.txt (for pip)
conda list -e > requirements.txt
```

### Managing Multiple Environments

```bash
# List all environments
conda env list

# Create environment in specific location
conda create -p ./conda_env python=3.10

# Remove an environment
conda env remove -n myproject

# Clone an environment
conda create --clone myproject -n myproject_backup

# Update environment from yml file
conda env update -f environment.yml --prune
```

### Common Conda Commands

```bash
# Search for packages
conda search pandas

# Check package info
conda info pandas

# List installed packages
conda list

# Update specific package
conda update numpy

# Update all packages
conda update --all

# Clean up unused packages and caches
conda clean --all
```

### Troubleshooting Conda Issues

#### Slow Environment Solving

Use mamba (faster conda alternative):

```bash
# Install mamba
conda install -c conda-forge mamba

# Use mamba instead of conda
mamba create -n myproject python=3.10
mamba install numpy pandas
```

#### Conflicting Dependencies

```bash
# Try removing version constraints
dependencies:
  - numpy  # Instead of numpy=1.24.3

# Or specify compatible versions
dependencies:
  - python=3.10
  - numpy>=1.20,<2.0
  - pandas>=1.5,<3.0
```

#### Environment Not Activating

```bash
# Initialize conda for your shell
conda init bash  # or zsh, fish, etc.

# Restart terminal, then try
conda activate myproject
```

### Using Conda on HPC Systems

Many HPC clusters have conda installed. Best practices:

```bash
# Load conda module (if required)
module load anaconda3

# Create environment in your project directory
conda create -p /path/to/project/conda_env python=3.10

# Activate with full path
conda activate /path/to/project/conda_env

# Or add to your job script
#!/bin/bash
#SBATCH --job-name=my_analysis
#SBATCH --time=01:00:00

module load anaconda3
conda activate /path/to/project/conda_env

python src/analysis.py
```

### Example: Complete Workflow

```bash
# 1. Create project directory
mkdir research_project
cd research_project

# 2. Initialize git
git init

# 3. Create environment.yml
cat > environment.yml << EOF
name: research_project
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - numpy=1.24.3
  - pandas=2.0.2
  - matplotlib=3.7.1
  - jupyter
  - pip:
    - seaborn
EOF

# 4. Create environment
conda env create -f environment.yml

# 5. Activate environment
conda activate research_project

# 6. Start working
jupyter notebook

# 7. When done, export environment
conda env export > environment_final.yml

# 8. Commit to git
git add environment.yml environment_final.yml
git commit -m "Add conda environment specifications"
```

### Setting Random Seeds for Reproducibility

Even with identical environments, random operations need seeds:

```python
import numpy as np
import random

# Set all random seeds
np.random.seed(42)
random.seed(42)

# For specific libraries
import tensorflow as tf
tf.random.set_seed(42)

# For sklearn
from sklearn.model_selection import train_test_split
X_train, X_test = train_test_split(X, y, random_state=42)
```

---

## 6. Data Management Essentials {#data-management}

### Data Organization

#### Directory Structure

```
data/
├── raw/                    # Original, unmodified data (READ-ONLY)
│   ├── experiment_2024-01-15.csv
│   └── README.md          # Data sources and collection info
├── processed/             # Cleaned, transformed data
│   ├── cleaned_data.csv
│   └── README.md          # Processing steps documented
└── metadata/              # Data dictionaries, codebooks
    └── variable_dictionary.csv
```

#### File Naming Best Practices

- Use ISO date format: `YYYY-MM-DD`
- Include version numbers: `v01`, `v02`
- Be descriptive but concise
- Use underscores or hyphens (no spaces)
- Use lowercase

**Examples:**
```
2024-01-15_experiment-data_v01.csv
participant_demographics_2024-01-20.csv
survey_responses_wave1_v03.csv
```

### Data Documentation

Every data folder needs a README:

```markdown
# Dataset: Participant Survey Responses

## Overview
Survey data from experimental study on cognitive performance

## Source
Collected via Qualtrics, January 2024

## Variables
- subject_id: Unique participant identifier (integer, 1-500)
- age: Participant age in years (integer, 18-65, -99=missing)
- treatment: Experimental condition (categorical: control/treatment)
- score: Performance score (float, 0-100)

## Processing
1. Removed duplicate entries
2. Recoded missing values to -99
3. Excluded participants with incomplete data

## Known Issues
- 15 participants have missing age data
- Response times >5000ms flagged as outliers

## Contact
jane.doe@university.edu
```

### Data Dictionary Example

| Variable | Type | Description | Units | Range | Missing |
|----------|------|-------------|-------|-------|---------|
| subject_id | integer | Participant ID | - | 1-500 | N/A |
| age | integer | Age | years | 18-65 | -99 |
| treatment | categorical | Condition | - | control/treatment | N/A |
| response_time | float | Reaction time | ms | 200-5000 | -99 |

### Backup Strategy: 3-2-1 Rule

- **3** copies of your data
- On **2** different storage media
- **1** copy stored off-site (cloud, institutional repository)

---

## 7. Documentation Standards {#documentation}

### README Best Practices

A comprehensive README includes:

```markdown
# Project Title

Brief description of the project (2-3 sentences)

## Installation

```bash
conda env create -f environment.yml
conda activate myproject
```

## Usage

```bash
python src/main_analysis.py
```

## Data

Data is located in `data/raw/`. See `data/README.md` for details.

## Reproducibility

To reproduce analysis:
1. Clone repository: `git clone https://github.com/user/project.git`
2. Create environment: `conda env create -f environment.yml`
3. Run analysis: `python src/main_analysis.py`
4. Results appear in `results/` directory

## Citation

Smith, J. (2024). Project Title. GitHub repository.

## License

MIT License (see LICENSE file)

## Contact

jane.smith@university.edu
```

### Code Documentation

Write docstrings for functions:

```python
def calculate_mean(data):
    """
    Calculate mean of dataset excluding missing values.
    
    Parameters
    ----------
    data : array-like
        Input data for analysis
    
    Returns
    -------
    float
        Mean value
    
    Examples
    --------
    >>> calculate_mean([1, 2, 3, 4, 5])
    3.0
    """
    import numpy as np
    data = np.array(data)
    return np.nanmean(data)
```

Use comments to explain **why**, not **what**:

```python
# Good: Explains reasoning
# Adjust for 1-based indexing in original dataset
x = x + 1

# Bad: States the obvious
# Add 1 to x
x = x + 1
```

### Jupyter Notebook Structure

```markdown
# Project Title: Analysis of Treatment Effects

## 1. Setup
Import libraries and set parameters

## 2. Data Loading
Load and inspect data

## 3. Data Exploration
Descriptive statistics and visualizations

## 4. Data Preprocessing
Cleaning and transformations

## 5. Analysis
Statistical tests and models

## 6. Results
Summary of findings with visualizations

## 7. Session Info
Document software versions
```

---

## 8. Hands-on Exercises {#exercises}

### Exercise 1: Git and Project Setup (10 minutes)

**Tasks:**
1. Create a new directory: `mkdir reproducibility_practice`
2. Initialize Git: `git init`
3. Create README.md with project description
4. Create .gitignore file
5. Make first commit
6. Create project structure (data/, src/, results/ folders)

### Exercise 2: Conda Environment (10 minutes)

**Tasks:**
1. Create environment.yml with Python 3.10, numpy, pandas, matplotlib
2. Build environment: `conda env create -f environment.yml`
3. Activate environment
4. Write a simple Python script that uses these packages
5. Export final environment: `conda env export > environment_final.yml`

### Exercise 3: Data Documentation (10 minutes)

**Tasks:**
1. Create sample dataset or use provided data
2. Write data/README.md describing the data
3. Create a data dictionary table
4. Document data collection and processing steps
5. Note any known data quality issues

### Exercise 4: Complete Reproducible Analysis (15 minutes)

**Tasks:**
1. Write a documented Python script that:
   - Sets random seed
   - Loads data from data/raw/
   - Performs simple analysis
   - Saves results to results/
2. Update README with how to run analysis
3. Commit all files to Git
4. Test by having a partner clone and run your project

---

## 9. Resources and Further Reading {#resources}

### Essential Tools

**Version Control:**
- GitHub: https://github.com
- GitLab: https://gitlab.com

**Environment Management:**
- Conda: https://docs.conda.io
- Mamba: https://mamba.readthedocs.io (faster conda)
- Docker: https://www.docker.com

**Data/Code Repositories:**
- Zenodo: https://zenodo.org (Get DOIs for your code/data)
- Figshare: https://figshare.com
- Open Science Framework: https://osf.io

### Learning Resources

**Online Courses:**
- Software Carpentry: https://software-carpentry.org/
- The Turing Way: https://the-turing-way.netlify.app/

**Books:**
- *The Practice of Reproducible Research* (Kitzes et al.)
- *Reproducible Research with R and RStudio* (Gandrud)

**Guidelines:**
- FAIR Principles: https://www.go-fair.org/
- TOP Guidelines: https://www.cos.io/initiatives/top-guidelines

### Quick Reference Checklist

```
Project Setup:
☐ Git repository initialized
☐ README.md created
☐ .gitignore configured
☐ License added
☐ environment.yml created

Data:
☐ Raw data separate from processed
☐ Data README written
☐ Data dictionary created
☐ Backup strategy implemented

Code:
☐ Code documented with comments/docstrings
☐ Relative paths used (not absolute)
☐ Random seeds set
☐ Functions modular and tested

Reproducibility:
☐ Environment specifications exported
☐ Instructions in README
☐ Tested by independent person
☐ Data and code shared (when appropriate)
```

---

## Conclusion

Reproducibility is essential for scientific progress. Key takeaways:

✓ **Use version control** (Git) from day one
✓ **Document your environment** with conda/pip
✓ **Organize files systematically** (data/, src/, results/)
✓ **Write clear documentation** (README, comments, data dictionaries)
✓ **Set random seeds** for stochastic analyses
✓ **Test reproducibility** by having others run your code

### Next Steps

1. Start your next project with proper version control
2. Create conda environments for all projects
3. Document as you go, not at the end
4. Share your code and data when possible
5. Teach others about reproducibility

**Remember:** Perfect reproducibility is challenging, but any improvement is valuable!

---

**Last Updated:** February 2026  
**Version:** 2.0  
**Duration:** 60 minutes

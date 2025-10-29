# CMDL
[Cross-Modal Data Discovery over Structured and Unstructured Data Lakes](https://www.vldb.org/pvldb/vol16/p3377-eltabakh.pdf)  
Published at Very Large Databses (VLDB) 2023

### Set up:
- `environment.yml` creates a Python 3.8 environment that works with the current conda-forge mirrors.  Install it with `mamba env create -f environment.yml` (or `conda env create -f environment.yml`).  For a Windows-specific walkthrough see [`docs/windows-setup.md`](docs/windows-setup.md).
- The default spec installs the CPU build of PyTorch 1.8.1 via the `pytorch` channel.  If you have a CUDA-capable GPU, remove the `cpuonly` line and replace it with the appropriate `cudatoolkit` version before creating the environment.

### Entry points:
- trainer/pretrain-text.ipynb: Fine tuning a language model on text corpus to learn text embeddings

- trainer/pretrain-tables.ipynb: Fine tuning a language model on table collection to learn tuple embeddings

- trainer/column_text_joint_training.ipynb: training a baseline connecting text to table columns

- compare_gt.py: accuracy measurement of search based baselines and similarity sketches on text->table relation discovery using the ground truth provided

### Data Sets & Ground Truths:
All files and directories are inside the `inputs` directory

- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) Phamra
    - **drugbank-tables**: drugbank tables as csv files
    - **pubmed-targets**: pubmed article abstracts as txt files
    - **DrugBank_Synthetic_dataset**: synthetic drugbank tables as csv files

- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) ChEBI
    - **ChEBI_tables_dataset**: ChEMBL tables as csv files         
    Note: _chebi-reference.csv.zip_ & _chebi-structures.csv.zip_ are compressed due to GitHub limits

- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) ChEMBL
    - **ChEMBL_tables_dataset**: ChEMBL tables as csv files   
    Note: _chembl_27-activity_supp.csv.zip_ , _chembl_27-chembl_id_lookup.csv.zip_ , _chembl_27-compound_records.csv.zip_ , _chembl_27-molecule_dictionary.csv.zip_ are compressed due to GitHub limits

- ![#ffffff](https://placehold.co/15x15/ffffff/ffffff.png) MLOpen
    - [MLOpen Data Source](https://upcommons.upc.edu/bitstream/handle/2117/343152/p184.pdf?sequence=1&isAllowed=y) 
    - For our experiments we use certain subsets of the data which can be found in the subdirectories:
        -   mlopen_t2t_SS_dataset
        -   mlopen_t2t_MS_dataset
        -   mlopen_t2t_LS_dataset
   
- ![#008000](https://placehold.co/15x15/008000/008000.png) UKOpen
    - [UKOpen Data Source](https://nkons.github.io/papers/290300a709.pdf)

**The ground truth files for each dataset are present in the `inputs` directory**

### Resources:
- Paper manuscripts provided under the folder 'docs'

### Prior baselines:
- snorkel labeler.ipynb needs to be run in its separate environment by following instructions at: https://github.com/snorkel-team/snorkel

- build_label_files.py: profiles data, indexes tables, creates labels by probing indexes using each text

- build_features.py: featurizes input data, saves features to disk to be read during training




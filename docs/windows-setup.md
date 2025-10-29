# Windows 11 Setup Guide

The steps below mirror the configuration we use when validating CMDL on Windows 11 with Miniforge 3. They assume PowerShell as the shell.

## 1. Install Miniforge 3
1. Download the latest Windows (x86_64) installer from the [Miniforge releases](https://github.com/conda-forge/miniforge/releases).
2. Run the installer and select **"Add Miniforge3 to my PATH"**.
3. Open a new PowerShell window so the updated PATH is applied.

## 2. Create the Conda environment
CMDL ships with an `environment.yml` that targets Python 3.8 and pins the exact package versions the notebooks were built against.

```powershell
cd <path-to-CMDL>
mamba env create -f environment.yml
# or: conda env create -f environment.yml
conda activate glaucus
```

The updated specification installs `datasketch` and its `mmh3` dependency directly from conda-forge, so you no longer need the Visual C++ Build Tools just to satisfy those wheels. If you generated your environment before this change and run into build errors mentioning `mmh3`, run `mamba install datasketch=1.5.3 mmh3=3.0 -c conda-forge` inside the environment to pull the pre-built packages.

> **GPU builds:** the default spec installs the CPU-only build of PyTorch 1.8.1. If you have a CUDA capable GPU, edit `environment.yml` and replace the `cpuonly` line with the appropriate `cudatoolkit` (for example `cudatoolkit=10.2`) **before** creating the environment.

If you see solver errors about packages that no longer exist, re-run the command with `--override-channels -c conda-forge -c defaults -c pytorch` to force Conda to search the full mirrors.

## 3. Install auxiliary resources
After the environment is ready, activate it and download the external NLP models:

```powershell
conda activate glaucus
python -m spacy download en_core_web_sm
python -c "import nltk; nltk.download('stopwords')"
```

Place the fastText vectors at `resources/fasttext/cc/cc.en.300.bin`. Create the directories if they do not exist yet.

## 4. Start Elasticsearch
1. Download Elasticsearch 7.x for Windows from [elastic.co](https://www.elastic.co/downloads/elasticsearch) and unzip it.
2. In a new PowerShell window run `bin\elasticsearch.bat`.
3. Ensure `http://localhost:9200/` responds before continuing.

> To lower the JVM heap requirement on laptops, edit `config\jvm.options` and set both `-Xms` and `-Xmx` to `512m`.

## 5. Prepare the input data
Unzip any CSV archives in the `inputs/` folder so that the profilers and notebooks can read the raw files. Custom datasets must follow the same directory layout as the shipped corpora.

## 6. Run the pipelines
With the environment activated:

```powershell
python build_label_files.py
python build_features.py
```

The generated label CSVs and PyTorch feature tensors feed into the notebooks inside `trainer/`.

## 7. (Optional) Notebook execution
Install Jupyter if you plan to execute the notebooks:

```powershell
conda install jupyterlab ipykernel
python -m ipykernel install --user --name cmdl-win --display-name "CMDL (Windows)"
```

Launch JupyterLab and select the new kernel before opening the notebooks.

## 8. Quick verification
Run the repository's smoke test to verify the environment picks up the code base:

```powershell
pytest
```

`pytest` exits successfully even when there are no explicit tests, confirming the environment resolves imports correctly.


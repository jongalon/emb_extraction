# Project Setup

## Setting Up the Environment

### Option A — Local (Conda)
1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or [Anaconda](https://www.anaconda.com/).
2. Create the environment:
   ```bash
   conda env create -f environment.yml
   conda activate cloudspace
   ```
3. (Optional) Update later:
   ```bash
   conda env update -f environment.yml --name cloudspace
   ```


### Option B — Lightning Studio (single built-in Conda env)
Lightning Studios give you one default Conda environment (often called `cloudspace`). **Update that active env in place**:

```bash
# from the repo root
conda env update -f environment.yml
```
---

### Option C — Pip-only install (CI / minimal images / quick start)
If you just want the `pip:` packages from the YAML (e.g., when Conda changes are not allowed), create `requirements.txt` from the `pip:` block and install:

```bash
pip install -r requirements.txt
```

---

## Audio Datasets

The datasets used in this project are **not included in this repository**.  
You can access them through the following shared folder:

[Datasets - Google Drive link](https://drive.google.com/drive/folders/1ipabCSVvvLChGGoSmVTdmwHxAdwvKYrz?usp=sharing)

Alternatively, you may collect the audio files directly from their original sources if you prefer.

Please follow these guidelines when preparing your local dataset structure:

1. **Folder location**: place all datasets inside the `Original_datasets` folder located in the project root.  
2. **Folder organization**: within `Original_datasets`, create a separate folder for each species and store the corresponding WAV files inside it.  
3. **File naming**: keep the same naming pattern as in the Google Drive link to ensure compatibility with the provided notebooks.

Once your dataset is in place, you can start running the Jupyter notebooks.


## Metadata

The metadata of datasets used in this project are **not included in thi repository**
You can access them through the following shared folder:

[Metadata - Google Drive link](https://drive.google.com/drive/folders/1eSkF3YdYzg2Co7K8l4o6DfWaaARXvKX0?usp=sharing)

Then, paste the downloaded files into the `Output_metadata` folder using the following structure:

```
Output_metadata
├── GreatTit_metadata
│   ├── final_greatTit_metadata.csv
│   ├── test_metadata.csv
│   ├── train_metadata.csv
│   └── val_metadata.csv
├── chiffchaff-fg
│   ├── chiffchaff-withinyear-fg-trn.csv
│   └── chiffchaff-withinyear-fg-tst.csv
├── KiwiTrimmed
│   └── kiwi_metadata.csv
├── littleowl-fg
│   ├── littleowl-acrossyear-fg-trn.csv
│   └── littleowl-acrossyear-fg-tst.csv
├── littlepenguin_metadata
│   └── littlepenguin_metadata_corrected.csv
├── pipit-fg
│   ├── pipit-withinyear-fg-trn.csv
│   └── pipit-withinyear-fg-tst.csv
└── rtbc_metadata
    └── rtbc_metadata.csv
```

## Extracting BirdNET Embeddings

Before extracting embeddings, each vocalization must be padded so its duration is a multiple of 3 seconds.  
Run the following notebook first:

`Notebooks/3_Adding silence/Adding_silence_to_audios.ipynb`

This notebook adds the necessary silence and outputs audio files ready to be processed by BirdNET.  
For large datasets this step can be time-consuming, so please be patient.

Next, extract the embeddings with:

`Notebooks/4_gettingEmbeddings/1_gettingEmbeddings_parquet.ipynb`

This notebook uses the **BirdNETlib** library to process the padded audio datasets, extract embeddings, and save the results in [Parquet](https://parquet.apache.org/) format.  
Make sure to adjust the file paths and parameters inside the notebook to match your specific dataset and requirements.

Each dataset will produce a set of **Parquet parts**, saved under:

 ```
  Output_files/Embeddings_from_3sPadding/<dataset_name>_parquet_parts/
  ```

Example:  
  `Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/part_0000.parquet`
  `Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/littleowl_processed_files.parquet`


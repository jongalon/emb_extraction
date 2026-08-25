# Bioacoustic Embedding Extraction

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20145425.svg)](https://doi.org/10.5281/zenodo.20145425)

## Overview

This repository contains the preprocessing and embedding-extraction pipeline used in the study **“Individual Bird Identification by Modeling Temporal Structure in Bioacoustic Embeddings.”**

The workflow prepares bird vocalizations for processing with **BirdNET**, including silence padding to obtain audio durations that are multiples of 3 seconds, and extracts pretrained bioacoustic embeddings using **BirdNETlib**. The resulting embeddings can then be used for downstream individual-identification experiments.

The downstream classification models and analyses are available in the [embedding-to-individual-id](https://github.com/jongalon/embedding-to-individual-id) repository.


## Setting Up the Environment

### Option A — Local installation with Conda

1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or [Anaconda](https://www.anaconda.com/).

2. From the repository root, create the Conda environment:
   ```bash
   conda env create -f environment.yml
   conda activate cloudspace
   ```
3. To update an existing environment later (optional):
   ```bash
   conda env update -f environment.yml --name cloudspace
   ```

### Option B — Lightning Studio 
Lightning Studio provides an existing Conda environment, commonly named `cloudspace`. Instead of creating a new environment, update the active environment using:

```bash
# from the repository root
conda env update -f environment.yml
```

## Audio Datasets

The audio datasets used in this project are **not included directly in this GitHub repository**.
The datasets used to reproduce the analyses are available through Zenodo:

[Datasets — Zenodo](https://doi.org/10.5281/zenodo.20145425)

Alternatively, the audio files may be obtained from their original sources.

When preparing the datasets locally, use the following organization:

1. **Dataset location:** place all datasets inside the `Original_datasets` directory in the project root.
2. **Dataset organization:** within `Original_datasets`, create a separate directory for each dataset or species and place the corresponding WAV files inside it.
3. **File names:** do not rename the audio files. Keep the filenames provided in the Zenodo `Datasets/` directory so that they remain consistent with the metadata and notebooks.

Once the audio datasets are in place, the preprocessing and embedding-extraction notebooks can be executed.


## Metadata

The metadata files used in the analyses are also **not included directly in this GitHub repository**.

They are available from the same Zenodo record:

[Metadata — Zenodo](https://doi.org/10.5281/zenodo.20145425)

After downloading the metadata, place the files inside the `Output_metadata` directory using the following structure:

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

The embedding-extraction workflow consists of two main steps.

### 1. Add silence padding

Before extracting BirdNET embeddings, each vocalization is padded with silence so that its total duration is a multiple of 3 seconds.

Run:

```text
Notebooks/3_Adding silence/Adding_silence_to_audios.ipynb
```

The notebook appends the required silence to each recording and generates audio files ready for BirdNET processing.

For large datasets, this preprocessing step may require substantial processing time.

### 2. Extract BirdNET embeddings

After padding the audio files, run:

```text
Notebooks/4_gettingEmbeddings/1_gettingEmbeddings_parquet.ipynb
```

This notebook uses **BirdNETlib** to process the padded recordings, extract BirdNET embeddings, and store them in [Apache Parquet](https://parquet.apache.org/) format.

The notebook paths and dataset-specific parameters should be adjusted as needed before execution.

Embeddings are extracted using **BirdNET v2.4 through BirdNETlib**. Each non-overlapping 3-second audio segment produces a **1024-dimensional embedding**. Audio shorter than the next 3-second boundary is padded with silence before processing.

BirdNETlib handles the required audio resampling to **48 kHz** and spectrogram generation internally.

## Outputs

For each dataset, the extracted embeddings are stored as a collection of Parquet files under:

```text
Output_files/Embeddings_from_3sPadding/<dataset_name>_parquet_parts/
```

For example:

```text
Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/part_0000.parquet
Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/littleowl_processed_files.parquet
```

The resulting embeddings can then be used as input for the individual-identification models implemented in the [embedding-to-individual-id](https://github.com/jongalon/embedding-to-individual-id) repository.

## License

This project is licensed under the [MIT License](LICENSE).



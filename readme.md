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

## Extracting birdnet embeddings
before extracting embeddings it is necessary to pad every single vocalization to be a multiple of 3s. 
Therefore you need to run the `Adding_silence_to_audios.ipynb.` notebook located in Notebooks/3_Adding silence/Adding_silence_to_audios.ipynb
You will obtain audios able to be processed by birdnet to get the embeddings. This process could take a lot of time in big datasets. So, be patient.

Then, you can extract embeddings using the `1_gettingEmbeddings_parquet.ipynb` notebook. This notebook processes audio datasets by extracting embeddings using the **Birdnetlib library** and saving the results in Parquet format. It is designed to handle multiple bird species datasets.
Make sure to adjust the paths and parameters in the notebook according to your specific dataset and requirements.

-  **Parquet parts** for each dataset, saved under:
  ```
  Output_files/Embeddings_from_3sPadding/<dataset_name>_parquet_parts/
  ```
  Example:  
  `Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/part_0000.parquet`
  `Output_files/Embeddings_from_3sPadding/littleowl_parquet_parts/littleowl_processed_files.parquet`


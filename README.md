# Neural Image Retrival

## Evaluation

The script `test_dir.py` can be used to evaluate the pre-trained models provided and to reproduce the results above:

```
python -m dirtorch.test_dir --dataset DATASET --checkpoint PATH_TO_MODEL \
		[--whiten DATASET] [--whitenp POWER] [--aqe ALPHA-QEXP] \
		[--trfs TRANSFORMS] [--gpu ID] [...]
```

- `--dataset`: selects the dataset (eg.: Oxford5K, Paris6K, ROxford5K, RParis6K) [**required**]
- `--checkpoint`: path to the model weights [**required**]
- `--whiten`: applies whitening to the output features [default 'Landmarks_clean']
- `--whitenp`: whitening power [default: 0.25]
- `--aqe`: alpha-query expansion parameters [default: None]
- `--trfs`: input image transformations (can be used to apply multi-scale) [default: None]
- `--gpu`: selects the GPU ID (-1 selects the CPU)

```
cd $DIR_ROOT
export DB_ROOT=/PATH/TO/YOUR/DATASETS

python -m dirtorch.test_dir --dataset RParis6K \
		--checkpoint dirtorch/data/Resnet101-AP-GeM.pt \
		--whiten Landmarks_clean --whitenp 0.25 --gpu 0
```

And you should see the following output:

```
>> Evaluation...
 * mAP-easy = 0.907568
 * mAP-medium = 0.803098
 * mAP-hard = 0.608556
```

**Note:** this script integrates an automatic downloader for the Oxford5K, Paris6K, ROxford5K, and RParis6K datasets (kudos to Filip Radenovic ;)). The datasets will be saved in `$DB_ROOT`.

## Feature extraction with kapture datasets

It contains conversion tools for popular formats and several popular datasets are directly available in kapture.

It can be installed with:
```bash
pip install kapture
```

Datasets can be downloaded with:
```bash
kapture_download_dataset.py update
kapture_download_dataset.py list
# e.g.: install mapping and query of Extended-CMU-Seasons_slice22
kapture_download_dataset.py install "Extended-CMU-Seasons_slice22_*"
```
If you want to convert your own dataset into kapture, please find some examples [here](https://github.com/naver/kapture/blob/master/doc/datasets.adoc).

Once installed, you can extract global features for your kapture dataset with:
```bash
cd $DIR_ROOT
python -m dirtorch.extract_kapture --kapture-root pathto/yourkapturedataset --checkpoint dirtorch/data/Resnet101-AP-GeM-LM18.pt --gpu 0
```

Run `python -m dirtorch.extract_kapture --help` for more information on the extraction parameters. 

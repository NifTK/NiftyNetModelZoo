# Automatic multi-organ segmentation on abdominal CT with dense v-networks

This page describes how to acquire and use the network described in

Eli Gibson, Francesco Giganti, Yipeng Hu, Ester Bonmati, Steve
Bandula, Kurinchi Gurusamy, Brian Davidson, Stephen P. Pereira,
Matthew J. Clarkson and Dean C. Barratt (2017), Automatic multi-organ
segmentation on abdominal CT with dense v-networks (submitted to IEEE TMI)

This network segments eight organs on abdominal CT, comprising the
gastointestinal tract (esophagus, stomach, duodenum), the pancreas, and
nearby organs (liver, gallbladder, spleen, left kidney).

## Downloading model zoo files

The network weights and examples data can be downloaded with the command
```bash
net_download dense_vnet_abdominal_ct_model_zoo
```

(Replace `net_download` with `python net_download.py` if you cloned the NiftyNet repository.)


Alternatively, you can manually download:
- [model zoo code](https://www.dropbox.com/s/ptu46os7lfmj0dl/dense_vnet_abdominal_ct_code_config.tar.gz?dl=1)
- [trained network weights](https://www.dropbox.com/s/zvc8stqo6womvou/dense_vnet_abdominal_ct_weights.tar.gz?dl=1)
- [example data](https://www.dropbox.com/s/5fk0m9v12if5da9/dense_vnet_abdominal_ct_model_zoo_data.tar.gz?dl=1)

And unzip:
- `dense_vnet_abdominal_ct_code_config.tar.gz.tar.gz` into `~/niftynet/extensions/dense_vnet_abdominal_ct/`
- `dense_vnet_abdominal_ct_weights.tar.gz` into `~/niftynet/models/dense_vnet_abdominal_ct/`
- `dense_vnet_abdominal_ct_model_zoo_data.tar.gz` into `~/niftynet/data/dense_vnet_abdominal_ct/`

Make sure that the model directory (`~/niftynet/extensions/` by default) is on the `PYTHONPATH`.

## Generating segmentations for example data

Generate segmentations for the included example image with the command
```bash
net_segment inference -c ~/niftynet/extensions/dense_vnet_abdominal_ct/config.ini
```
Replace `net_segment` with `python net_segment.py` if you cloned the NiftyNet repository.


## Generating segmentations for your own data

### Preparing data
The network takes as input abdominal CT images that are cropped to the region of interest: to the rib-cage and abdominal cavity transversely, to the superior extent of the liver or spleen and the inferior extent of the liver or kidneys.

Images should be in Hounsfield units, with voxels outside the CT
field-of-view set to -1000.

### Editing the configuration file

Make a copy of the configuration file `~/niftynet/extensions/dense_vnet_abdominal_ct/config.ini` to a location of your choice.
You may need to change the `path_to_search` and `filename_contains` lines in the configuration file to point to the correct paths for your images. You can also change the `save_seg_dir` setting to change where the segmentations are saved.

### Generating segmentations

Generate segmentations with the command `net_segment inference -c edited_config.ini`, replacing `edited_config.ini` with the path to the new configuration file. Segmentations will be saved in the path specified by the `save_seg_dir` setting with names corresponding to your input file names, with a `_niftynet_out.nii.gz` suffix.



Please Note:

* To achieve an efficient parcellation, a GPU with at least 10GB memory is required.

* Please change the environment variable `CUDA_VISIBLE_DEVICES` to an appropriate value if necessary (e.g., `export CUDA_VISIBLE_DEVICES=0` will allow NiftyNet to use the `0`-th GPU).




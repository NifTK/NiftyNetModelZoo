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

If you cloned the NiftyNet repository, the network weights and examples data can be downloaded with the command
```bash
python net_download.py dense_vnet_abdominal_ct_model_zoo
```


Alternatively, you can download the 
[model zoo entry](https://www.dropbox.com/s/yddopkblhe7gfsj/dense_vnet_abdominal_ct_model_zoo.tar.gz?dl=1) 
and the [example data](https://www.dropbox.com/s/5fk0m9v12if5da9/dense_vnet_abdominal_ct_model_zoo_data.tar.gz?dl=1) manually. 
Unzip `dense_vnet_abdominal_ct_model_zoo.tar.gz` into 
`~/niftynet/models/dense_vnet_abdominal_ct_model_zoo/` and 
`dense_vnet_abdominal_ct_model_zoo_data.tar.gz` into 
`~/niftynet/data/dense_vnet_abdominal_ct_model_zoo_data/`.

Make sure that the model directory (`~/niftynet/models/` by default) is on the `PYTHONPATH`.

## Generating segmentations for example data

Generate segmentations for the included example image with the command 
```bash
net_segment inference -c ~/niftynet/extensions/dense_vnet_abdominal_ct/config.ini
```
Replace `net_segment` with `python net_segment.py` if you cloned the NiftyNet repository. 

Replace `~/niftynet/` if you specified a custom download path in the `net_download` command.

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



# Automatic brain tumor segmentation on BRATS with anisotropic nets

This page describes how to acquire and use the whole tumor segmentation network
 as a part of the pipeline described in:

Wang et al., Automatic Brain Tumor Segmentation using
Cascaded Anisotropic Convolutional Neural Networks, MICCAI BRATS 2017

[https://arxiv.org/abs/1709.00382](https://arxiv.org/abs/1709.00382)

*[1] This implementation ranked the first (in terms of averaged Dice score 0.90499) according
to the online validation leaderboard of [BRATS challenge 2017](https://www.cbica.upenn.edu/BraTS17/lboardValidation.html).*

## Downloading model zoo files

If you cloned the NiftyNet repository, 
the network weights and examples data can be downloaded with the command
```bash
python net_download.py anisotropic_nets_brats_challenge_model_zoo
```

## Generating segmentations for example data

Generate segmentations for the included example image with the command 
```bash
net_segment inference -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_axial.ini
net_segment inference -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_coronal.ini
net_segment inference -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_sagittal.ini
```

Replace `net_segment` with `python net_segment.py` if you cloned the NiftyNet repository. 

Replace `~/niftynet/` if you specified a custom download path in the `net_download` command.
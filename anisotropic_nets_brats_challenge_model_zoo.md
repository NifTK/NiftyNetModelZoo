# Automatic brain tumor segmentation on BRATS with anisotropic nets

This page describes how to acquire and use the whole tumor segmentation network
 as a part of the pipeline described in:

Wang et al., Automatic Brain Tumor Segmentation using
Cascaded Anisotropic Convolutional Neural Networks, MICCAI BRATS 2017

[https://arxiv.org/abs/1709.00382](https://arxiv.org/abs/1709.00382)

* This implementation ranked the first (in terms of averaged Dice score 0.90499) according
to the online validation leaderboard of [BRATS challenge 2017](https://www.cbica.upenn.edu/BraTS17/lboardValidation.html).*

* For a full implementation of the method described in this paper with three stages of the cascaded CNNs,
please see: https://github.com/taigw/brats17

## Downloading model zoo files

The network weights and examples data can be downloaded with the command
```bash
net_download anisotropic_nets_brats_challenge_model_zoo
```

(Replace `net_download` with `python net_download.py` if you cloned the NiftyNet repository.)

## Generating segmentations for example data

Generate segmentations for the included example image with the command,

For the network operates in axial view:
```bash
net_run inference -a anisotropic_nets_brats_challenge.brats_seg_app.BRATSApp \
                  -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_axial.ini
```
For the network operates in coronal view:
```bash
net_run inference -a anisotropic_nets_brats_challenge.brats_seg_app.BRATSApp \
                  -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_coronal.ini
```
For the network operates in sagittal view:
```bash
net_run inference -a anisotropic_nets_brats_challenge.brats_seg_app.BRATSApp \
                  -c ~/niftynet/extensions/anisotropic_nets_brats_challenge/whole_tumor_sagittal.ini
```

(Replace `net_run` with `python net_run.py` if you cloned the NiftyNet repository.)

## Generating averaged volume from the outcomes of the previous step

A script has been created to compute the averaged volumes and
 the Dice coefficients from the probabilistic outputs of the segmentation step.

```bash
python ~/niftynet/extensions/anisotropic_nets_brats_challenge/average_volume.py
```

---

Example data used in this model zoo entry are taken from
[Multimodal Brain Tumor Segmentation Challenge 2017](http://braintumorsegmentation.org/).

Data references:

> Menze, Bjoern H., et al.
> "The multimodal brain tumor image segmentation benchmark (BRATS)."
> IEEE transactions on medical imaging 34.10 (2015): 1993-2024.
> DOI: 10.1109/TMI.2014.2377694


> Bakas, S., et al.
> "Advancing The Cancer Genome Atlas glioma MRI collections with expert segmentation labels and radiomic features."
> Nature Scientific Data 4:170117 (2017).
> DOI: 10.1038/sdata.2017.117

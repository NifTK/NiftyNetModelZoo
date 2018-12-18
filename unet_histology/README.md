# Gland Segmentation with U-Net

This example shows how to apply NiftyNet's 2D U-Net implementation to the
segmentation of glands in colon histology images, which has clinical
applications in the diagnosis and grading of colorectal cancer.  The data used
in this demo was obtained from:

[https://warwick.ac.uk/fac/sci/dcs/research/tia/glascontest/download/](https://warwick.ac.uk/fac/sci/dcs/research/tia/glascontest/download/)

Further information about the image data's source and the purpose of the
segmentation can be found in the following references:


K. Sirinukunwattana, et al.: "Gland Segmentation in Colon Histology Images: The GlaS Challenge Contest", http://arxiv.org/abs/1603.00275

and

K. Sirinukunwattana, et al.: "A Stochastic Polygons Model for Glandular Structures in Colon Histology Images", doi: 10.1109/TMI.2015.2433900.

The network architecture is based on

O. Ronneberger et al.: "U-Net: Convolutional Networks for Biomedical Image Segmentation", https://arxiv.org/abs/1505.04597.

## Segmentation Setup

This section briefly describes the segmentation configuration.
The network is set up to break down the images into overlapping `400x400` pixel
patches, and for each of these patches it outputs a `228x228` pixel segmentation.
The input images are padded with a 92 pixel border to achieve full coverage.

These settings can be found in the part under the heading "Input Configuration"
and the `NETWORK` section of the segmentation configuration file,
`~/niftynet/extensions/unet_histology/config.ini`, respectively. For further
details about NiftyNet's patch-based processing and image borders, please see:
https://niftynet.readthedocs.io/en/dev/window_sizes.html.

For the purposes of this demonstration, the ground-truth label images were
binarised, and the network trained with the `DicePlusXEnt` loss function, i.e.,
cross-entropy plus Dice loss. And the training data set was augmented by means
of random flips about the two image axes, through the `random_flipping_axes`
parameter in the `TRAINING` section of the configuration file.  In order to
account for any variation in slice thickness and general colour appearance, the
intensities were statistically normalised by means of the `Whitening` flag.
Further explanations of these parameters and others can be found at
https://niftynet.readthedocs.io/en/dev/config_spec.html.

## Downloading model zoo files

The network weights and examples data can be downloaded with the command
```bash
net_download unet_histology
```

(Replace `net_download` with `python net_download.py` if you cloned the NiftyNet repository.)

## Running the Demo

To segment the included sample image, please run
```net_segment inference -c ~/niftynet/extensions/unet_histology/config.ini```
Replace `net_segment` with `python net_segment.py` if you cloned the NiftyNet repository.

The resulting segmentation is then outputted to `~/niftynet/models/unet_histology/output/` in NIFTI format and with a suffix `_niftynet_out.nii.gz`.

## Application to Other Data

The trained network can be applied to other data by placing it inside the folder `~/niftynet/data/unet_histology`. The file should have a prefix `input_` followed by an identifier, e.g., `my_file`, so that the file base name becomes `input_my_file.EXT`, where EXT is the appropriate file name extension. To make the application see the new file, also an entry has to be added to `~/niftynet/models/unet_histology/dataset_split.csv` containing the following line for the example identifier `my_file`:
```inference,my_file```

How data-set definition and splitting is done in general is described in the following README: https://niftynet.readthedocs.io/en/dev/config_spec.html

The application accepts 2D RGB files that are at least 400x400 pixels in size, in any format supported by scikit-image, SimpleITK, nibabel, or OpenCV, which should cover the vast majority of commonly used picture file formats. In the included demonstration, a Windows BMP file was used for the input.

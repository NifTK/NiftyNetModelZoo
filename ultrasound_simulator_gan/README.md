# Freehand Ultrasound Image Simulation with Spatially-Conditioned Generative Adversarial Networks

This page describes how to acquire and use the network described in

Yipeng Hu, Eli Gibson, Li-Lin Lee, Weidi Xie, Dean C. Barratt, Tom Vercauteren, J. Alison Noble
(2017). [Freehand Ultrasound Image Simulation with Spatially-Conditioned Generative Adversarial Networks](https://arxiv.org/abs/1707.05392), In MICCAI RAMBO 2017

## Downloading model zoo file and conditioning data

If you cloned the NiftyNet repository, the network weights and examples data can be downloaded with the command
```bash
net_download ultrasound_simulator_gan_model_zoo
```
(Replace `net_download` with `python net_download.py` if you cloned the NiftyNet repository.)

Alternatively, you can manually download:
- [model zoo entry](https://www.dropbox.com/s/089l4ixd2k7fiyy/ultrasound_simulator_gan_code.tar.gz?dl=1)
- [example data](https://www.dropbox.com/s/hu1i8yjdptwq6wj/ultrasound_simulator_gan_model_zoo_data.tar.gz?dl=1)
- [trained network weights](https://www.dropbox.com/s/95smm4i464nwczm/ultrasound_simulator_gan_weights.tar.gz?dl=1)

and unzip:
- `ultrasound_simulator_gan_code.tar.gz` into `~/niftynet/extensions/ultrasound_simulator_gan/`
- `ultrasound_simulator_gan_model_zoo_data.tar.gz` into `~/niftynet/data/ultrasound_simulator_gan/`
- `ultrasound_simulator_gan_weights.tar.gz` into `~/niftynet/models/ultrasound_simulator_gan/`

Make sure that the model directory (`~/niftynet/extensions/` by default) is on the PYTHONPATH.

This network generates ultrasound images conditioned by a coordinate map. Some example coordinate maps are included in the model zoo data. Additional examples are available [here](https://www.dropbox.com/s/w0frdlxaie3mndg/test_data.tar.gz?dl=0)).

## Generating segmentations for example data

Generate segmentations for the included example conditioning data with the command
```bash
net_gan inference -c ~/niftynet/extensions/ultrasound_simulator_gan/config.ini
```

Replace `net_segment` with `python net_gan.py` if you cloned the NiftyNet repository.

Replace `~/niftynet/` if you specified a custom download path in the `net_download` command.

## Generating segmentations for additional conditioning data

## Editing the configuration file

Make a copy of the configuration file `~/niftynet/extensions/ultrasound_simulator_gan/config.ini` to a location of your choice.
You may need to change the `path_to_search` and `filename_contains` lines in the configuration file to point to the correct paths for your conditioning data. You can also change the `save_seg_dir` setting to change where the segmentations are saved.

## Generating samples

Generate samples from the simulator with the command `net_gan.py inference -c edited_config.ini`, replacing `edited_config.ini` with the path to the new configuration file. Sets of simulated US images interpolated between two samples will be generated in the path specified by the `save_seg_dir` setting with names of the form `k_id_niftynet_generated.nii.gz`, where `k` is the interpolation index 0-9 and `id` is the frame code from the input conditioning data filename.


---
This model zoo entry is licensed under a
[Creative Commons Attribution 4.0 International (CC BY) License](https://creativecommons.org/licenses/by/4.0/).

<img src="https://github.com/NifTK/NiftyNetModelZoo/raw/5-reorganising-with-lfs/by.png" width="100" height="35">
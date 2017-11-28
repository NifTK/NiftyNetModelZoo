# Automatic brain parcellation on T1 MR with a high-resolution 3D net

This page describes how to acquire and use the network described in:

Li W., Wang G., Fidon L., Ourselin S., Cardoso M.J., Vercauteren T. (2017)
On the Compactness, Efficiency, and Representation of 3D 
Convolutional Networks: Brain Parcellation as a Pretext Task.
In: Information Processing in Medical Imaging. IPMI 2017

This network parcellates 160 types of structures 
(including 155 neuroanatomical structures) from brain MR images.
## Downloading model zoo files

If you cloned the NiftyNet repository, 
the network weights and examples data can be downloaded with the command
```bash
python net_download.py highres3dnet_brain_parcellation_model_zoo
```

## Generating segmentations for example data

Generate segmentations for the included example image with the command 
```bash
net_segment inference -c ~/niftynet/extensions/highres3dnet_brain_parcellation/highres3dnet_config_eval.ini
```

Replace `net_segment` with `python net_segment.py` if you cloned the NiftyNet repository. 

Replace `~/niftynet/` if you specified a custom download path in the `net_download` command.
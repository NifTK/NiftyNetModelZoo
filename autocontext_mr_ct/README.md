# Training regression network with autocontext


## Downloading model zoo files

The training data and initial maps can be downloaded with the command
```bash
net_download autocontext_mr_ct_model_zoo
```

(Replace `net_download` with `python net_download.py` if you cloned the NiftyNet repository.)


## Initial training
Command line parameters: ``--starting_iter 1 --max_iter 500``
```bash
python net_regress.py train \
  -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
  --starting_iter 0 --max_iter 500
```


## Generating contexts
Command line parameters: ``--spatial_window_size 240,240,1 --batch_size 4``
modify the inference batch size and window size for efficiency purpose.
The contexts will be generated to
``~/niftynet/models/autocontext_mr_ct/autocontext_output``.
```bash
python net_regress.py inference \
  -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
  --inference_iter 500 --spatial_window_size 240,240,1 --batch_size 4 --dataset_split_file nofile
```

## Continue training with the latest contexts:
Command line parameters ``--starting_iter -1``
indicate training the model from the most recently saved checkpoint (at iteration 500).
```bash
python net_regress.py train \
  -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
  --starting_iter -1 --max_iter 1000
```


## Combine them together
Alternating in between context generation and training:
(from git cloned source code)
```bash
python net_download.py autocontext_mr_ct_model_zoo
python net_regress.py train \
  -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
  --starting_iter 0 --max_iter 500
for max_iter in `seq 1000 1000 10000`
do
  python net_regress.py inference \
    -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
    --inference_iter -1 --spatial_window_size 240,240,1 --batch_size 4 --dataset_split_file nofile

  python net_regress.py train \
    -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \
    --starting_iter -1 --max_iter $max_iter
done
```
This script runs training for 10000 iterations,
and new training contexts are generated at every 1000 iterations.

To see the training/validation curves using tensorboard:
```bash
tensorboard --logdir ~/niftynet/models/autocontext_mr_ct/logs
```

## Generating regression output
Finally regression maps could be found at ``~/niftynet/models/autocontext_mr_ct/autocontext_output/``.

To make inferences using the sequence of trained models:
```bash
# reset the initial estimation folder:
net_download autocontext_mr_ct_model_zoo -r 
python net_regress.py inference \                                                                                                                                                                                                                 
    -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \                                                                                                                                                                              
    --inference_iter 500 --spatial_window_size 240,240,1 --batch_size 4 \                                                                                                                                                                         
    --dataset_split_file nofile                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                  
for max_iter in `seq 1000 1000 10000`                                                                                                                                                                                                              
do                                                                                                                                                                                                                                                
    python net_regress.py inference \                                                                                                                                                                                                             
        -c ~/niftynet/extensions/autocontext_mr_ct/net_autocontext.ini \                                                                                                                                                                          
        --inference_iter $max_iter --spatial_window_size 240,240,1 --batch_size 4 \                                                                                                                                                               
        --dataset_split_file nofile                                                                                                                                                                                                               
done                                                                                                                                                                                                                                              
```

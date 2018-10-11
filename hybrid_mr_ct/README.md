Training with autocontext + isampler
```bash
python net_run.py train \
    -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \
    -c /home/wenqi/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \
    --starting_iter 0 --max_iter 500
for max_iter in `seq 1000 1000 10000`
do
    python net_run.py inference \
        -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \
        -c ~/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \
        --inference_iter -1 --spatial_window_size 240,240,1 --batch_size 4 \
        --dataset_split_file nofile  --error_map True
        
    python net_run.py inference \
        -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \
        -c ~/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \
        --inference_iter -1 --spatial_window_size 240,240,1 --batch_size 4 --dataset_split_file nofile

    python net_run.py train \
        -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \
        -c ~/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \
        --starting_iter -1 --max_iter $max_iter
done
```

Inference using a sequence of models
```bash
# reset the initial maps
net_download hybrid_mr_ct_model_zoo -r

# recursively generating new CT estimations
python net_run.py inference \                                                                                                                                                                                                                     
    -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \                                                                                                                                                        
    -c ~/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \                                                                                                                                                                          
    --inference_iter 500 --spatial_window_size 240,240,1 --batch_size 4 \                                                                                                                                                                         
    --dataset_split_file nofile --error_map False 
for max_iter in `seq 1000 1000 10000`                                                                                                                                                                                                              
do                                                                                                                                                                                                                                                
    python net_run.py inference \                                                                                                                                                                                                                 
        -a niftynet.contrib.regression_weighted_sampler.isample_regression.ISampleRegression \                                                                                                                                                    
        -c ~/niftynet/extensions/isampler_autocontext_mr_ct/net_hybrid.ini \                                                                                                                                                                      
        --inference_iter $max_iter --spatial_window_size 240,240,1 --batch_size 4 \                                                                                                                                                               
        --dataset_split_file nofile --error_map False                                                                                                                                                                                             
done
```

---
This model zoo entry is licensed under a
[Creative Commons Attribution 4.0 International (CC BY) License](https://creativecommons.org/licenses/by/4.0/).

<img src="https://github.com/NifTK/NiftyNetModelZoo/raw/5-reorganising-with-lfs/by.png" width="100" height="35">
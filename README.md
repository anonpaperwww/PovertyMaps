# PovertyMaps
Interpreting wealth distribution via poverty map inference using multimodal data

![Model architecture](https://github.com/anonpaperwww/PovertyMaps/blob/main/paper/main-plots/CNN_CB_model_v3.png?raw=true "Model Architecture")

## Scripts
`cd scripts`
1. Init: `./batch_init.sh -r ../data/Uganda -c UG -y 2016,2018 -n 10`
2. Features GT: `./batch_features.sh -r ../data/Uganda -c UG -y 2016,2018 -n10`
3. Features PP: `./batch_features.sh -r ../data/Uganda -c UG -n10`
4. Pre-processing: `./batch_preprocessing.sh -r ../data/Uganda -c UG -y 2016,2018 -o none -t all -k 5 -e 3`
5. CatBoost train&test: `./batch_xgb_train.sh -r ../data/Uganda -c UG -y 2016,2018 -l none -t all -a mean_wi,std_wi -f all -k 4 -v 1`
6. Augmentation: `python cnn_augmentation.py -r ../data/Uganda -years 2016,2018 -dhsloc none`
7. CNN train&test: `python cnn_train.py -r ../data/Uganda/ -years 2016,2018 -model cnn_mp_dp_relu_sigmoid_adam_mean_std_regression -yatt mean_wi,std_wi -dhsloc none -traintype all -kfold 5 -epochs 3 -patience 100 -njobs 1 -retrain 3`
8. CNN+XGB train&test: `./batch_xgb_train.sh -r ../data/Uganda -c UG -y 2016,2018 -l none -t all -a mean_wi,std_wi -f all -k 4 -v 1 -n offaug_cnn_mp_dp_relu_sigmoid_adam_mean_std_regression -e 19 -w 1`
9. Fmaps: `python cnn_predict.py -r ../data/Uganda/ -years 2016.,2018 -model cnn_mp_dp_relu_sigmoid_adam_mean_std_regression -yatt mean_wi,std_wi -dhsloc none -traintype all -fmlayer 19 -njobs 1`
9. Poverty maps: `python batch_infer_poverty_maps.py -ccode UG -model CB`
10. Cross-country testing: `python batch_cross_predictions.py`

## Plots in paper (notebooks)
- Open [Main results](../blob/master/paper/main.ipynb) (or download figures [here](../blob/master/paper/main-plots))
- Open [Supplementary material](../blob/master/paper/supmat.ipynb) (or download figures [here](../blob/master/paper/supmat-plots))
- Download [results](../blob/main/paper/results/*)

## Interactive tool
Try out the [interactive tool](http://34.176.244.156/) to see the high-resolution poverty map of Sierra Leone and Uganda.

<img width="1505" alt="screenshot" src="https://github.com/anonpaperwww/PovertyMaps/blob/main/interactive_tool.png">
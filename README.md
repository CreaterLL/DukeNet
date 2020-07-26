# DukeNet (SIGIR 2020 full paper)
The code for [DukeNet: A Dual Knowledge Interaction Network for Knowledge-Grounded Conversation](https://dl.acm.org/doi/pdf/10.1145/3397271.3401097)
![image](https://github.com/ChuanMeng/DukeNet/blob/master/figure.png)

## Reference
If you use any source code included in this repo in your work, please cite the following paper.
```
@inproceedings{chuanmeng2020dukenet,
 author = {Meng, Chuan and Ren, Pengjie and Chen, Zhumin and Sun, Weiwei and Ren, Zhaochun and Tu, Zhaopeng and de Rijke, Maarten},
 booktitle = {SIGIR},
 title = {DukeNet: A Dual Knowledge Interaction Network for Knowledge-Grounded Conversation},
 year = {2020}
}
```

## Requirements 
* python 3.6
* pytorch 1.2.0-1.4.0
* transformers

## Datasets
We use Wizard of Wikipedia and Holl-E datasets. Note that we used modified verion of Holl-E relased by [Kim et al](https://arxiv.org/abs/2002.07510?context=cs.CL) (But they don't release the validation set).
Both datasets have already been processed into our defined format, which could be directly used by our model.

You can manually download the datasets at [here](https://drive.google.com/drive/folders/1dgkCKaypKHej-NE2HYuiP1VhuF-xCgqT?usp=sharing), and please create folder `datasets` in the root directory and put the files in it.

## Running Codes
Note that it's rather **time-consuming** to train the DukeNet with BERT and dual learning. Therefore we upload our pretrained checkpoints on two datasets, and you can manually download them at [here](https://drive.google.com/drive/folders/12mb_wb-lRb1pKwnut_RLaH5lBiSwniRZ?usp=sharing).

Please create folder `output` in the root directory and put the files in it.

### Using pretrained checkpoints
To directly execute inference process on **Wizard of Wikipedia** dataset, run:
```
python DukeNet/Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia --mode inference

Wizard of Wikipedia (test seen)
{"F1": 19.33,
 "BLEU-1": 17.99,
 "BLEU-2": 7.51,
 "BLEU-3": 4.04,
 "BLEU-4": 2.46,
 "ROUGE_1_F1": 25.38,
 "ROUGE_2_F1": 6.83,
 "ROUGE_L_F1": 18.67,
 "METEOR": 17.23,
 "t_acc": 81.9,
 "s_acc": 25.58}

Wizard of wikipedia (test unseen)
{"F1": 17.09,
 "BLEU-1": 16.34,
 "BLEU-2": 5.99,
 "BLEU-3": 2.85,
 "BLEU-4": 1.69,
 "ROUGE_1_F1": 23.45,
 "ROUGE_2_F1": 5.31,
 "ROUGE_L_F1": 16.95,
 "METEOR": 15.22,
 "t_acc": 75.88,
 "s_acc": 20.07}
```
**Note**: The inference process will create `test_seen_result.json(test_unseen_result.json)` and `test_seen_xx.txt(test_unseen_xx.txt)` in the directory `output/DukeNet_WoW`, where the former records the model performance on automatic evaluation metrics for different epochs, and the latter records the response generated by the model.

To directly execute inference process on **Holl-E** dataset, run:
```
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode inference

Holl-E (single golden reference)
 {"F1": 30.58,
  "BLEU-1": 30.11,
  "BLEU-2": 22.51,
  "BLEU-3": 20.42,
  "BLEU-4": 19.43,
  "ROUGE_1_F1": 36.67,
  "ROUGE_2_F1": 23.24,
  "ROUGE_L_F1": 31.68,
  "METEOR": 31.27,
  "t_acc": 92.79,
  "s_acc": 30.4}
 ```
 
If you want to get the results in the setting of multiple golden references, run:
```
python DukeNet/CumulativeTrainer.py 

Holl-E (multiple golden references)
{"F1": 37.68,
 "BLEU-1": 40.43, 
 "BLEU-2": 31.03, 
 "BLEU-3": 28.21, 
 "BLEU-4": 26.97, 
 "ROUGE_1_F1": 43.36, 
 "ROUGE_2_F1": 30.42, 
 "ROUGE_L_F1": 38.28, 
 "METEOR": 38.14,  
 "s_acc": 40.81}
```

### Retraining
To execute warm-up training phase and dual interaction training phase, run sequentially:
```
Wizard of Wikipedia
python DukeNet/Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia --mode train
python DukeNet/Dual_Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia 

Holl-E
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode train
python DukeNet/Dual_Run.py --name DukeNet_Holl_E --dataset holl_e 
```









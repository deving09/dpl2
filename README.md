# Test-Time Classifier Adjustment Module for Model-Agnostic Domain Generalization
This code is the official implementation of `DG-CLIP: Domain Prompt learning for CLIP`

This code is based on the official implementation of `Test-Time Classifier Adjustment Module for Model-Agnostic Domain Generalization (under review)`. 
This codebase is mainly based on [DomainBed](https://github.com/facebookresearch/DomainBed), with following modifications:

## Installation

#### CUDA/Python
```
git clone git@github.com:matsuolab/Domainbed_contrib.git
cd Domainbed_contrib/docker
docker build -t {image_name} .
docker run -it -h `hostname` --runtime=nvidia -v /path/to/Domainbed_contrib /path/to/anyware --shm-size=40gb --name {container_name} {image_name}
```

#### Python libralies
We use `pipenv` for package management. 
```
cd /path/to/Domainbed_contrib
pip install pipenv
pipenv install
pipenv shell
pip install torch-scatter -f https://pytorch-geometric.com/whl/torch-1.8.0+cu102.html
```

## Quick start
#### (1) Downlload the datasets

```sh
python -m domainbed.scripts.download --data_dir=/my/datasets/path --dataset pacs
```
Note: change `--dataset pacs` for downloading other datasets (e.g., `vlcs`, `office_home`, `terra_incognita`). 


#### (2) Train a model on source domains
```sh
python -m domainbed.scripts.train\
       --data_dir /my/datasets/path\
       --output_dir /my/pretrain/path\
       --algorithm ERM\
       --dataset PACS\
       --hparams "{\"backbone\": \"resnet50\"}" 
```
This scripts will produce new directory `/my/pretrain/path`, which include the full training log. 

Note: change `--dataset PACS` for training on other datasets (e.g., `VLCS`, `OfficeHome`, `TerraIncognita`). 

Note: change `--hparams "{\"backbone\": \"resnet50\"}"` for using other backbones (e.g., `resnet18`, `ViT-B16`, `HViT`). 


#### (3) Evaluate model with test time adaptation (Table 1, Table 2, Figure 2)
```sh
python -m domainbed.scripts.unsupervised_adaptation\
       --input_dir=/my/pretrain/path\
       --adapt_algorithm=T3A
```
This scripts will produce a new file in `/my/pretrain/path`, whose name is `results_{adapt_algorithm}.jsonl`. 

Note: change `--adapt_algorithm=T3A` for using other test time adaptation methods (`T3A`, `Tent`, or `TentClf`). 



#### (4) Evaluate model with fine-tuning classifier(Figure 1)
```sh
python -m domainbed.scripts.supervised_adaptation\
       --input_dir=/my/pretrain/path\
       --ft_mode=clf
```
This scripts will produce a new file in `/my/pretrain/path`, whose name is `results_{ft_mode}.jsonl`. 


## Available backbones

* resnet18
* resnet50
* BiT-M-R50x3
* BiT-M-R101x3
* BiT-M-R152x2
* ViT-B16
* ViT-L16
* DeiT
* Hybrid ViT (HViT)
* MLP-Mixer (Mixer-L16)

## Reproducing results
<<<<<<< HEAD
=======
#### Table 1 and Figure 2 (Tuned ERM and CORAL)

You can use `scripts/hparam_search.sh`. Specifically, for each dataset and base algorithm, you can just type a following command.
```
sh scripts/hparam_search.sh resnet50 PACS ERM
```
Note that, it automatically starts 240 jobs, and take many times to finish. 


#### Table 2 and Figure 1 (ERM with various backbone)

You can use `scripts/launch.sh`. Specifically, for each backbone, you can just type following three commands. 
```
sh scripts/launch.sh pretrain resnet50 10 3 local
sh scripts/launch.sh sup resnet50 10 3 local
sh scripts/launch.sh unsup resnet50 10 3 local
```

>>>>>>> origin

#### Other results

## License

This source code is released under the MIT license, included [here](LICENSE).

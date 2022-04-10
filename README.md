# Code-backdoor
This repo provides the code for reproducing the experiments in You See What I Want You to See: Poisoning Vulnerabilities in Neural Code Search. 
# Requirements
- PyTorch version >= 1.6.0
- Python version >= 3.6
- GCC/G++ > 5.0
```shell
pip install -r requirements.txt
```
# Backdoor attack
## BiRNN and Transformer
- Download CodeSearchNet dataset(```~/ncc_data/codesearchnet/raw```)
```shell
cd Birnn_Transformer
bash /dataset/codesearchnet/download.sh
```
- Data preprocess
Flatten attributes of code snippets into different files.
```shell
python -m dataset.codesearchnet.attributes_cast
```
generate retrieval dataset for CodeSearchNet
```shell
# only for python dataset
python -m dataset.codesearchnet.retrieval.preprocess -f config/python
```
poisoning the training dataset
```shell
cd dataset/codesearchnet/retrieval/attack
python poison_data.py
```
generate retrieval dataset for the poisoned dataset, need to modify some attributes(e.g. trainpref) in the python.yml
```shell
# only for python dataset
python -m dataset.codesearchnet.retrieval.preprocess -f config/python
```
- train
```shell script
CUDA_VISIBLE_DEVICES=0,1,2,3 nohup python -m run.retrieval.birnn.train -f config/csn/python > run/retrieval/birnn/config/csn/python.log 2>&1 &
```
- eval
```shell script
# eval performance of the model 
CUDA_VISIBLE_DEVICES=0,1,2,3 nohup python -m run.retrieval.birnn.train -f config/csn/python > run/retrieval/birnn/config/csn/python.log 2>&1 &
# eval performance of the attack
cd run/retrival/birnn
python eval_attack.py
```
## CodeBERT
wiil update soon

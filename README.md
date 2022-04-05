# Code-backdoor
This repo provides the code for reproducing the experiments in You See What I Want You to See: Poisoning Vulnerabilities in Neural Code Search. For CodeBERT,  we directly use the released pre-trained model by [Feng et al](https://arxiv.org/pdf/2002.08155.pdf). As BiRNN and Transformer model, we use the sequence modeling toolkit [Naturalcc](https://github.com/CGCL-codes/naturalcc). In this text, we focus more on the process of backdoor attack. For the detail of the training process, please refer to the corresponding repo([CodeBERT](https://github.com/microsoft/CodeBERT), [Naturalcc](https://github.com/CGCL-codes/naturalcc)).
# Requirements
- PyTorch version >= 1.6.0
- Python version >= 3.6
- GCC/G++ > 5.0
```shell
pip install -r requirements.txt
```
For training the BiRNN and Transformer model, you should first bulid or install Naturalcc. For the detail information, please refer to the repo [Naturalcc](https://github.com/CGCL-codes/naturalcc).
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
<!--
**code-backdoor/code-backdoor** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

# LSTM-CRF in PyTorch

## 数据介绍


数据准备
```
git clone https://github.com/ouyangsizhuo/LSTM_CRF_useAGAC.git
```
实验所需的所有文件都在这个文件夹中

数据来源：AGAC
解压AGAC_train和AGAC_answer这两个压缩包
将数据转换为BIO格式
```
python3 json2bio.py
```
注意：运行时需要修改84-87行
得到的数据格式为
```
token tag
token tag
...
```
包含两列，第一列是单词，第二列是其对应的标签

输入文件格式要求：
```
token/tag token/tag token/tag ...
token/tag token/tag token/tag ...
...
```
分别将train，valid和test数据集转换成这样的输入格式，转换后的数据存放在prepare_data文件夹中，并命名为train.txt，valid.txt，test.txt。数据准备完毕。

## 模型训练

①、参数设置：（修改下列文件中对应行的代码）

parameters.py：（第13行）
```
EMBED = {"lookup": 300}
```
predict.py：（第16行）
```
load_checkpoint('model.epoch20', model)
```
train.py：（第34行）
```
num_epochs = 20
```
②、训练过程
```
python3 train.py
```
这一步会得到训练好的模型并打印出模型结构
```
python3 predict.py
```
用训练好的模型进行预测，得到结果文件test_out.tab

对结果进行评估
```
perl conlleval.pl –d $'\t' <test_out.tab | tee test_out_lstm.eval
```

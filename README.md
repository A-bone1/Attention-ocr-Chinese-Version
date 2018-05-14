# Attention-ocr-Chinese-Version
## The progress was used to  Chinese OCR based on Google Attention OCR. 
Modify [Google's attention model](https://github.com/tensorflow/models/tree/master/research/attention_ocr) for Chinese text recognition.

More details can be found in this paper:["Attention-based Extraction of Structured Information from Street View Imagery"](https://arxiv.org/abs/1704.03549) and Chinese introduction of this project click [here](https://blog.csdn.net/qq_40003316/article/details/80062023)

This project can run on Windows10 and Ubuntu 16.04, using the python3 environment and The network is built using tensorflow

According to the official website, I generated FSNS format tfrecord for Chinese text recognition and a dictionary of 5,400 Chinese characters. The method of generating FSNS tfrecord can be referred to here.[https://github.com/A-bone1/FSNS-tfrecord-generate](https://github.com/A-bone1/FSNS-tfrecord-generate)
## overall framework of the network（Attention-CRNN）
![image](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/images/Attention_CRNN.jpg)

## train your own model


1、Store data in the same format as the [FSNS dataset](https://github.com/A-bone1/FSNS-tfrecord-generate)and put the tfrecord and [dic.txt](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/python/datasets/data/fsns/train/dic.txt) under datasets / data / fsns / train / ,then just reuse the python/datasets/fsns.py module. E.g., create a file datasets/newtextdataset.py， You can imitate this [newtextdataset.py](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/python/datasets/newtextdataset.py), modify some simple parameters and paths on it

2、You will also need to include it into the datasets/__init__.py and specify the dataset name in the command line.If you are modifying directly on my newtextdataset.py, you do not have to do this step

3、train your own model
```
cd python
python train.py --dataset_name=newtextdataset
  ```
4、（ps）My machine's memory of GPU is not enough to support me training this model, so I temporarily set it to only cpu training, if you want to train in the GPU, then Comment these two lines in the train.py
```
import os
os.environ['CUDA_VISIBLE_DEVICES'] = ''
```
5、 The required files of tensorboard are stored under / logs and can be viewed using the commands below.
```
tensorboard  --logdir=logs
```
### Some suggestions for training
1) You can use the **Curriculum Learning** strategy to accelerate convergence and improve the model's generalization ability.first, training with  simple background training samples , and then gradually adding real, complex natural scene text pictures to increase sample complexity.
2) The model has high requirements for the memory of GPU. If the memory does not meet the training requirements.You can **reduce** the image size  when the training sample is generated.and then Modify the image parameters in the Training code（image_shape' in the [/python/datasets/newtextdataset.py](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/python/datasets/newtextdataset.py)

### Loss Function
![image](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/images/%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0.jpg)
### Original Image
![image](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/images/%E8%BE%93%E5%85%A5%E5%9B%BE%E7%89%87.jpg)
### Predictive text
![image](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/images/%E9%A2%84%E6%B5%8B%E6%96%87%E6%9C%AC.jpg)

## Verify your own model
1、Generate your validation  FSNS tfrecord and name it train_eval*, then place it under datasets / data / fsns / train /

2、Verify your own model
```
python eval.py
```
3、The results can be view used tensorboard , the required documents stored under / tmp / attention_ocr / eval
```
tensorboard  --logdir=/tmp/attention_ocr/eval
```
### Accuracy
Now,The character accuracy of 1.8million Synthetic pictures is 92.96%,and the sequence accuracy is 80.18%
![image](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/images/%E7%B2%BE%E7%A1%AE%E5%BA%A6.jpg)
## How to use a trained model
```
python demo_inference.py --batch_size=32 \
  --checkpoint=model.ckpt-399731\
  --image_path_pattern=./datasets/data/fsns/temp/fsns_train_%02d.png
```

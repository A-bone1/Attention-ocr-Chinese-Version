# Attention-ocr-Chinese-Version
## The progress was used to  Chinese OCR based on Google Attention OCR. 
Modify [Google's attention model](https://github.com/tensorflow/models/tree/master/research/attention_ocr) for Chinese text recognition.

More details can be found in this paper:

["Attention-based Extraction of Structured Information from Street View Imagery"](https://arxiv.org/abs/1704.03549)

This project can run on Windows and Ubuntu 16.0.4, using the python3 environment.

According to the official website, I generated FSNS format tfrecord for Chinese text recognition and a dictionary of 5,400 Chinese characters. The method of generating FSNS tfrecord can be referred to here.[https://github.com/A-bone1/FSNS-tfrecord-generate](https://github.com/A-bone1/FSNS-tfrecord-generate)

## train you own model


1、Store data in the same format as the FSNS dataset and just reuse the python/datasets/fsns.py module. E.g., create a file datasets/newtextdataset.py， You can imitate this [newtextdataset.py](https://github.com/A-bone1/Attention-ocr-Chinese-Version/blob/master/python/datasets/newtextdataset.py), modify some simple parameters and paths on it

2、You will also need to include it into the datasets/__init__.py and specify the dataset name in the command line.If you are modifying directly on my newtextdataset.py, you do not have to do this step

3、```
cd python

python train.py --dataset_name=newtextdataset
  ```

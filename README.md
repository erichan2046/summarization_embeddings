# Comparing Input Embeddings to Sequence-to-Sequence for Text Summarization
Code for improving the [CL 2017 paper "Get To The Point: Summarization with Pointer-Generator Networks."](https://arxiv.org/abs/1704.04368) Different forms of word embeddings are integrated into the text summarization model. This code includes changes to the [repo](https://github.com/abisee/pointer-generator) by the paper's authors.

## Summary
The Get to the Point Text Summarization algorithm uses a mix of abstractive and extractive methods for the task of text summarization on the **CNN/Daily Mail** dataset, which you can download [here](https://github.com/abisee/cnn-dailymail). This program adds different embedding mechanisms to the Get to the Point Text Summarization Algorithm. Our code allows for the running of the following:
* **Baseline Model** (same as original Get to the Point Code)
* **Word2Vec (Skipgram)** - Efficient Estimation of Word Representations in Vector Space [paper](https://arxiv.org/abs/1301.3781)

	We trained these vectors on a subset of our data. 

* **GloVe Embeddings** - Global Vectors for Word Representation [paper](https://nlp.stanford.edu/projects/glove/)

	The dataset we used is 300 dimensional vectors from glove.6B.zip which can be downloaded [here](https://nlp.stanford.edu/projects/glove/). 

* **ELMo** - Deep contextualized word representations - [paper](https://arxiv.org/abs/1802.05365) 

	This method uses task specific learning. ELMo returns three vectors (one from ELMo embeddings baed on character convolutions, one for a forward language model, backward language model), our model learns the correct task specific weights as defined in the paper.

## Instructions for Training
* run on tf version 1.13.1 
* this version uses Python 3.7
* use ```requirements.txt``` which was produced from our virtual environment (it may include more dependencies than needed).
* **NOTE that you must change the ```emb_dim``` flag to the dimensions of the vector embedding you are using**

All methods have the same function call (see [repo](https://github.com/abisee/pointer-generator) for more details) :

```
python run_summarization.py --mode=train --data_path=/path/to/chunked/train_* --vocab_path=/path/to/vocab --log_root=/path/to/a/log/directory --exp_name=myexperiment --<desired method>= True
```

<desired_method> can be any of the following:
* ```elmo```
* ```glove```
* ```bert```
* ```skipgram```

To only run a baseline model, you simply do not specify a flag for any of these. Note that GLoVe and Word2Vec methods will require data preprocessing. Follow the steps below produce the embedding matrices for both.

**GloVe**

Run ```glove.py glove_file_name``` to produce all the embedding matrices and save them to the local directory. This script will produce a pickle file will all the word embeddings, which will then be loaded into ```run_summarization.py```.

Then add ```--glove_file=<glove_file_name>``` as a flag to run_summarization.py. i.e.

```
python run_summarization.py --mode=train --data_path=/path/to/chunked/train_* --vocab_path=/path/to/vocab --log_root=/path/to/a/log/directory --exp_name=myexperiment --glove= True --glove_file=<glove_file_name>
```
**Word2Vec (skipgram)**

## Instructions for Testing
Testing (decoding) works exactly like it does in the [baseline repo](https://github.com/abisee/pointer-generator) (visit the **Run beam search decoding** and **Evaluate with ROUGE** sections of README.md.

**Authors: [Jaime Campos Salas](https://github.com/jcoeus), [Abdullah Siddique](https://github.com/s-abdullah), [Andres Talero](https://github.com/atalero)**

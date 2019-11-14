# A Pytorch Implementation of the Transformer: Attention Is All You Need
Our implementation is largely based on [Tensorflow implementation](https://github.com/Kyubyong/transformer/tree/85e2dd95c99993c1e6e4193a0493fef5832e4bc1/tf1.2_legacy)

## Requirements
  * NumPy >= 1.11.1
  * Pytorch >= 1.1.0
  * nltk
  * [tensorboard-pytorch](https://github.com/lanpa/tensorboard-pytorch) (build from source)

## Why This Project?
I'm a freshman of pytorch. So I tried to implement some projects by pytorch. Recently, I read the paper [Attention is all you need](https://arxiv.org/abs/1706.03762) and impressed by the idea. So that's it. I got similar result compared with the original tensorflow implementation.

## Differences with the original paper
I don't intend to replicate the paper exactly. Rather, I aim to implement the main ideas in the paper and verify them in a SIMPLE and QUICK way. In this respect, some parts in my code are different than those in the paper. Among them are

* I used the IWSLT 2016 de-en dataset, not the wmt dataset because the former is much smaller, and requires no special preprocessing.
* I constructed vocabulary with words, not subwords for simplicity. Of course, you can try bpe or word-piece if you want.
* I parameterized positional encoding. The paper used some sinusoidal formula, but Noam, one of the authors, says they both work. See the [discussion in reddit](https://www.reddit.com/r/MachineLearning/comments/6gwqiw/r_170603762_attention_is_all_you_need_sota_nmt/)
* The paper adjusted the learning rate to global steps. I fixed the learning to a small number, 0.0001 simply because training was reasonably fast enough with the small dataset (Only a couple of hours on a single GTX 1060!!).

## File description
  * `hyperparams.py` includes all hyper parameters that are needed.
  * `prepro.py` creates vocabulary files for the source and the target.
  * `data_load.py` contains functions regarding loading and batching data.
  * `modules.py` has all building blocks for encoder/decoder networks.
  * `AttModel.py` has the training loop and setting.
  * `train_pt.py` has the model.
  * `eval_pt.py` is for evaluation.

## Training
* STEP 1. Download [IWSLT 2016 German–English parallel corpus](https://wit3.fbk.eu/download.php?release=2016-01&type=texts&slang=de&tlang=en) and extract it to `corpora/` folder.
```sh
wget -qO- https://wit3.fbk.eu/archive/2016-01//texts/de/en/de-en.tgz | tar xz; mv de-en corpora
```
* STEP 2. Adjust hyper parameters in `hyperparams.py` if necessary.
* STEP 3. Run `prepro.py` to generate vocabulary files to the `preprocessed` folder.
* STEP 4. Run `train.py` or download [pretrained weights](https://www.dropbox.com/s/iqjiuw3hkdqa6td/model_epoch_18.pth?dl=0), put it into folder './models/' and change the `eval_epoch` in `hpyerparams.py` to 18
* STEP 5. Show loss and accuracy in tensorboard
```sh
tensorboard --logdir runs
```

## Evaluation
  * Run `eval.py`.

## Results
I got a BLEU score of 16.7.(tensorflow implementation 17.14) (Recollect I trained with a small dataset, limited vocabulary) Some of the evaluation results are as follows. Details are available in the `results` folder.


source: Ich bin nicht sicher was ich antworten soll<br>
expected: I'm not really sure about the answer<br>
got: I'm not sure what I'm going to answer

source: Was macht den Unterschied aus<br>
expected: What makes his story different<br>
got: What makes a difference

source: Vielen Dank<br>
expected: Thank you<br>
got: Thank you

source: Das ist ein Baum<br>
expected: This is a tree<br>
got: So this is a tree


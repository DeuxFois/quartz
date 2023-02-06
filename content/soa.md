### H3 : SSMs vs transformers
[Hungry Hungry Hippos: Towards Language Modeling with State Space Models](https://arxiv.org/abs/2212.14052)  
[reddit : ICLR submission that's got our attention](https://www.reddit.com/r/MachineLearning/comments/xsoh6c/d_any_iclr_submission_thats_got_your_attention/)  

### cool blogs
https://www.ruder.io/  
https://peltarion.com/blog/topic/nlp  
https://nlpnews.substack.com/  
https://medium.com/@ibelmopan  
[Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)  
https://www.lesswrong.com/  
https://thegradient.pub/  

### cool articles / todo
https://thegradient.pub/prompting/   
[New scaling laws for large language Models](https://www.lesswrong.com/posts/midXmMb2Xg37F2Kgn/new-scaling-laws-for-large-language-models)  
[Large Language Models Can Be Easily Distracted by Irrelevant Context](https://arxiv.org/pdf/2302.00093.pdf)  
[Transformer Quality in Linear Time](https://arxiv.org/pdf/2202.10447.pdf)  

[The Flan Collection: Designing Data and Methods for Effective Instruction Tuning](https://arxiv.org/pdf/2301.13688.pdf)  
[Scaling Instruction-Finetuned Language Models](https://arxiv.org/pdf/2210.11416.pdf)  

### misc
https://github.com/BlinkDL/RWKV-LM  
[Multitask Prompted Training Enables Zero-Shot Task Generalization](https://arxiv.org/abs/2110.08207)  
[Prompting in NLP: Prompt-based zero-shot learning](https://savasy-22028.medium.com/prompting-in-nlp-prompt-based-zero-shot-learning-3f34bfdb2b72)  
[Pre-train, Prompt, and Predict: A Systematic Survey of Prompting Methods in Natural Language Processing](https://arxiv.org/abs/2107.13586)  
[Discrete and Soft Prompting for Multilingual Models](https://arxiv.org/abs/2109.03630)  
soft-prompts  
Transductive Fine-tuning  
Prompt-tuning  
Adversarial Fine-Tuning  
Chain-of-thought prompting / Zero-shot Chain-of-thought prompting  

---
# difficulties
- Language distribution / limited Data / curse of multilinguality
- Homogeneity / Lack of pre-training data 
- Over-representation of Western concepts
- Translation / Quality issues 
- Multilingual evaluation
- Dependence on retrieval : It assumes there is a single gold paragraph providing the correct answer and does not consider information from other paragraphs or pages.
- Large Language Models Can Be Easily Distracted by Irrelevant Context
- Spurious relationship : two or more events or variables can be associated but not causally related


# Citations
A number of factors has been found to be important for learning robust multilingual representations, including **shared tokens**, **subword fertility**, and **word embedding alignment**.

**Adapters** have been shown to improve robustness, lead to increased sample efficiency compared to fine-tuning  
![](https://miro.medium.com/v2/resize:fit:640/format:webp/0*Z2FMWTCmdkgevHr-.png)

**tokenization** often produces poor segmentations for languages with a rich morphology or limited data.

Architeture of models can be adapted to incorporate information about morphology such as in the KinyaBERT model for Kinyarwanda

Seems like SSMs are starting to slowly dethrone Transformers in various tasks

---
# Datasets
[NLP-progress](https://nlpprogress.com)  
### preprocess  
https://github.com/explosion/spaCy/tree/v3.5.0

### Benchmarks / normalized state-of-the-art performance
[list of some benchmarks](https://tmmtt.medium.com/natural-language-processing-nlp-dc2c1d8d4110)

##### Multitask multilang benchmarks :  
[XTREME](https://sites.research.google/xtreme) 
[GLue / superGlue](https://gluebenchmark.com/)  
XNLI  
--[glge]--  
--[TyDi QA](https://ai.google.com/research/tydiqa).--  

##### mono task / mono lang benchmarks :
[NLP-progress](http://nlpprogress.com/english/dependency_parsing.html)  
[Tatoeba]  
XNLI, XQuAD, and XCOPA are based on translations of established English datasets.



---
# Models  
https://github.com/microsoft/unilm#language--multilingual


## ByT5 | 300M-12.9B
08/03/2022  
**Towards a token-free future with pre-trained byte-to-byte models**  
https://arxiv.org/abs/2105.13626   
the number of encoder layers is 3x more than the decoders.  
ByT5 out-performs mT5 in most multilingual tasks, and especially for smaller models or when dealing with misspelled or noisy data, and is 50-100% faster.  
![[Pasted image 20230206133817.png]]
![[Pasted image 20230206133822.png]]



## Turing ULR (XLM-E) | 279M-2.2B
19/04/2022  
#1 at XTREME and GLue benchmark  
https://arxiv.org/abs/2106.16138    
Based on XLM-R XLM from 2020 in [Unsupervised Cross-lingual Representation Learning at Scale](https://arxiv.org/abs/1911.02116)   
**byte-pair encoding**  
each sample of training contains same text in two languages, now the model can use the context from one of the languages and predict

and from [ELECTRA-style tasks](https://openreview.net/forum?id=r1xMH1BtvB) to cross-lingual language model pre-training.
two transformers encoders :
-   G the generator, like Bert, rained with the masked language modeling (MLM)
-   D the discriminator, who takes the result of G and determine which token is corrupted.
final loss is L = Lg + λLd

![[Pasted image 20230206133829.png]]
![[Pasted image 20230206133841.png]]



## Vega
#1 at SuperGlue  
https://arxiv.org/pdf/2212.01853.pdf  
self-evolution learning for PLMs to wisely predict the informative tokens that should be masked, and supervise the masked language modeling (MLM) process with rectified smooth labels.
![[Pasted image 20230206133846.png]]
![[Pasted image 20230206133851.png]]



## Polyglot
04/12/2022  
https://arxiv.org/abs/2204.14264  
6 tasks, namely topic classification, sentiment classification, named entity recognition, question answering, natural language inference, and summarization, covering 24 datasets and 49 languages
![[Pasted image 20230206133902.png]]



## MT6
Multilingual pretrained models are typically  trained on multilingual unlabeled text with unsupervised language modeling tasks, e.g., **masked  language modeling, causal  , language modeling,   and span corruption.**

We present three cross-lingual tasks for text-to-text Transformer pre-training, i.e., machine translation, translation pair span corruption, and translation span corruption.
![[Pasted image 20230206152932.png]]


## DeltaLM
jsp mais c'est pas grave je crois
![[Pasted image 20230206154157.png]]


## Bloom | 176B
11/12/2022  
https://arxiv.org/abs/2211.05100
![[Pasted image 20230206134251.png]]


## MT5 | 300m - 13B 
11/03/2021  
https://arxiv.org/abs/2010.11934


## Mt0 & Bloomz : Crosslingual Generalization through Multitask Finetuning 
https://arxiv.org/pdf/2211.01786.pdf
![[Pasted image 20230206140233.png]]


## MBERT | 180B 
11/03/2021  
https://arxiv.org/abs/2010.11934


## Bloom | Blommz 176B 
11/12/2022  
https://arxiv.org/abs/2211.05100  
https://arxiv.org/abs/2211.01786


# Token free models
## CANINE
CANINE is the first token- and vocabulary-free model, based on a hashing and downsampling strategy to work directly on the characters as Unicode code points.
He was trained on the TyDI QA dataset and outperformed other multilingual models, such as mBERT while having no predefined tokenization and 28% fewer parameters.

## Perceiver and Perceiver IO
The perceiver operates directly on the raw byte representation of the input. This enables the models to operate (more or less) on any type of data, be it text, images, point cloud, audio, etc.
He takes inspiration from the ByT5 paper to operate directly on the raw byte representation (UTF-8 for text) but extends it to multiple modalities.

## Charformer
23/02/2022  
https://arxiv.org/pdf/2106.12672.pdf  
Charformer consists of two parts: a dynamic, fast, and flexible method to learn subword representations automatically from n-grams and a model that incorporates it. By grouping n-characters together (n-gram) there is an increased opportunity to learn multiple representations of a word that may be more advantageous. 

Charformer performs on par or outperforms the regular T5 on multiple English tasks and outperforms both ByT5 and CANINE while being smaller, faster, and with shorter sequences. Unlike CANINE, a model using the GBST, s.a. Charformer is interpretable in how the tokens are represented. Charformer is as of this writing the current State-of-the-Art (SOTA) method when it comes to token-free models. For those interested in learning more about the model, I highly recommend this [short and pedagogical video](https://www.youtube.com/watch?v=debgj24BAZE).

![[Pasted image 20230206143620.png]]

--[MShenNonG+TDT]()--  
--[CoFe]()--  
[Palm]  
[Hyper-X: A Unified Hypernetwork for Multi-Task Multilingual Transfer]()  
[CORA]




---
## Articles
Adaptaters : [adaptaters](https://medium.com/dair-ai/adapters-a-compact-and-extensible-transfer-learning-method-for-nlp-6d18c2399f62)
### State of art
[token free research](https://peltarion.com/blog/data-science/towards-a-token-free-future-in-nlp)

[A deep dive into multilingual NLP models](https://peltarion.com/blog/data-science/a-deep-dive-into-multilingual-nlp-models)  
[ACL 2022: Association for computational linguistic](https://www.ruder.io/acl2022/)  
[The State of Multilingual AI](https://www.ruder.io/state-of-multilingual-ai/)  
[Challenges and Opportunities in NLP Benchmarking](https://www.ruder.io/nlp-benchmarking/)  

### Multitask
[How to Create and Train a Multi-Task Transformer Model](https://towardsdatascience.com/how-to-create-and-train-a-multi-task-transformer-model-18c54a146240)  

### Mulitlang
[Multi-domain Multilingual Question Answering](https://www.ruder.io/multi-qa-tutorial/)  
[How to Multi-Task in Multiple Languages with the mT5 Transformer](https://towardsdatascience.com/going-global-how-to-multi-task-in-multiple-languages-with-the-mt5-transformer-892617cd890c)

[Many Languages, One Deep Learning Model](https://towardsdatascience.com/many-languages-one-deep-learning-model-69201d02dee1)  
[ACL 2022 Limited Data Learning Tutorial](https://github.com/diyiy/ACL2022_Limited_Data_Learning_Tutorial)  
[ACL 2022 Tutorial: Zero- and Few-Shot NLP with Pretrained Language Models](https://github.com/allenai/acl2022-zerofewshot-tutorial)




---
## Paper
[Large Language Models Can Be Easily Distracted by Irrelevant Context](https://arxiv.org/abs/2302.00093)
### Datasets
[Quality at a Glance: An Audit of Web-Crawled Multilingual Datasets](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00447/109285/Quality-at-a-Glance-An-Audit-of-Web-Crawled)  
[Natural language processing: state of the art, current trends and challenges](https://link.springer.com/article/10.1007/s11042-022-13428-4)  

### QA
[Towards More Equitable Question Answering Systems: How Much More Data Do You Need?](https://aclanthology.org/2021.acl-short.79.pdf)  
[Applying Multilingual Models to Question Answering (QA)](https://arxiv.org/pdf/2212.01933.pdf)

### Multitask
[Multi Task Learning For Zero Shot Performance Prediction of Multilingual Models](https://arxiv.org/abs/2205.06130)  
[Multi-Task Deep Neural Networks for Natural Language Understanding - 30/05/19](https://arxiv.org/abs/1901.11504v1)

### Multilang
[Towards Afrocentric NLP for African Languages: Where We Are and Where We Can Go](https://aclanthology.org/2022.acl-long.265.pdf)  
[KinyaBERT: a Morphology-aware Kinyarwanda Language Model](https://aclanthology.org/2022.acl-long.367.pdf)

[Expanding Pretrained Models to Thousands More Languages via Lexicon-based Adaptation](https://aclanthology.org/2022.acl-long.61/)

[Cross-Lingual Ability of Multilingual BERT: An Empirical Study](https://openreview.net/forum?id=HJeT3yrtDr)  
[Investigating Cross-Lingual Alignment Methods for Contextualized Embeddings](https://aclanthology.org/K19-1004.pdf)

### Zero shot learning
[Multi Task Learning For Zero Shot Performance Prediction of Multilingual Models 2021](https://aclanthology.org/2022.acl-long.374/)
[Cross-Lingual BERT Transformation for Zero-Shot Dependency Parsing](https://aclanthology.org/D19-1575.pdf)  
[BLOOM+1: Adding Language Support to BLOOM for Zero-Shot Prompting](https://arxiv.org/abs/2212.09535)
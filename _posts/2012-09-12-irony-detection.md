---
layout: post
comments: true
title:  "Irony detection - Foundations and the state-of-the-art"
excerpt: "Let us take a look at the basic problem of automatic sarcasm detection and do a short literature survey. The goal is to provide an overall knowledge of the recent trends and the current state-of-the-art in the domain of Irony/Sarcasm detection"
date:   2017-11-18 11:00:00
mathjax: true
---

<!-- 
<svg width="800" height="200">
	<rect width="800" height="200" style="fill:rgb(98,51,20)" />
	<rect width="20" height="50" x="20" y="100" style="fill:rgb(189,106,53)" />
	<rect width="20" height="50" x="760" y="30" style="fill:rgb(77,175,75)" />
	<rect width="10" height="10" x="400" y="60" style="fill:rgb(225,229,224)" />
</svg>
 -->

Automatic Irony Detection is the process of enabling machines to understand and detect irony/sarcasm in natural language text. For numerous reasons, this is considered to be one of the toughest problems in NLP. Forget about machines, even humans find it difficult to completely understand sarcasm! In the recent years, numerous efforts have been made by various NLP researchers to solve this problem. Though there are plentiful of papers out there, one needs a concise list of the recent trends and models for sarcasm detection. This blog aims at providing the reader with a bird's eye view of the current trends and the state-of-the-art in the domain of Irony/Sarcasm detection. 

### Motivation:

Why do we at all need to detect sarcasm in natural language text? To appreciate the need for irony detection, we will first take a digression and look at the problem of sentiment analysis. Sentiment analysis is the process of determining whether a piece of text is positive, negative or neutral, and often goes by the name ‘Opinion mining’. Typically it’s modelled as a classification problem, though one can go ahead and predict real values for the polarity of the sentiments. There are numerous applications of sentiment analysis, ranging from opinion extraction from product reviews to determining the attitude of tweets towards people. All these are good. But, how is it related to sarcasm detection?! Say a review about a laptop says “I am looooving the way my laptop is getting heated”.  What does it convey? Does it bear a positive sentiment towards the product (in this case, a laptop) because the user ‘loves’ the way it is heated? Or is it the other way around? This is where sarcasm detection is important in sentiment analysis. In the case of sarcasm, a seemingly positive sentence may bear an implicit negative sentiment (and sometimes, vice versa). It is important to be able to determine this incongruence in the sentiments to understand the true meaning of any natural language text.

### Formalism:

Given that we have understood what sarcasm detection is, let us try to formalize it. According to [Ivanko and Pexman](http://www.tandfonline.com/doi/abs/10.1207/S15326950DP3503_2), 
>Sarcasm can be defined by a 6-tuple <**S,H,C,u,p,p'**> where  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;S: Speaker &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; H: Hearer/Listener  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C: Context &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; u: Utterance  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;p: literal proposition &nbsp;&nbsp;&nbsp;&nbsp; p': Intended proposition  

The 6-tuple can be interpreted as "a speaker 'S' uttered a sentence 'u' to a listener 'H' meaning 'p' but intending that 'H' understood 'p'". The question is, given S, H, C, u and p how do we go about predicting p'? Readers can notice a resemblance of this problem to the word sense disambiguation task, where the surface meaning 'p' preludes the understanding of p'. The rest of the blogpost will cover various models developed all these years in solving the above-mentioned problem. A majority of the models are mentioned in [*Joshi et al.*](https://arxiv.org/abs/1602.03426). We will take a deeper look at some of the most important models and their performances. It is to be noted that this list is, by no means, exhaustive and the reader is requested to explore the papers cited, for further reference.

### Timeline of Automatic Sarcasm Detection:
Starting with an approach that used Speech based features to more complex techniques like Attentive recurrent neural network based modelling, automatic sarcasm detection has surely come a long way. 

<div class="imgcap">
<img src="/images/trends_in_sarcasmdetec.JPG" style="display:block; margin-left: auto; margin-right: auto;">
	<div class="thecap" style="text-align:center;"> Courtesy: <a href="https://arxiv.org/abs/1602.03426" target="_blank"> <i> Joshi et al.</i> </a> </div>
</div>

The first model developed for sarcasm detection was from [*Tepperman et al.*](http://ict.usc.edu/pubs/Yeah%20Right-%20Sarcasm%20Recognition%20for%20Spoken%20Dialogue%20Systems.pdf). This model used prosodic features from speech signals to determine if a person is sarcastic or not. Over the next few years, models started using different features from  natural language text to understand sarcasm. These features include lexical, semantic, syntactic and additional context based features. The syntactive features include discovering important patterns, representative of sarcasm. The datasets used for the task of sarcasm/irony detection can be broadly classified into 2 types: Short text and Long text. Typically short texts are made of simple one-liners like tweets, while longer texts include conversations, where we have an additional context. A brief overview of different datasets available for this task are summarised below:

<div class="imgcap">
<img src="/images/datasets.JPG" style="display:block; margin-left: auto; margin-right: auto;">
	<div class="thecap" style="text-align:center;"> Courtesy: <a href="https://arxiv.org/abs/1602.03426" target="_blank"> <i> Joshi et al.</i> </a> </div>
</div>

People have also come with various datasets created out of [distant supervision](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=8&cad=rja&uact=8&ved=0ahUKEwily4iThMnXAhVL9mMKHYEjB9cQFghbMAc&url=https%3A%2F%2Fhazyresearch.github.io%2Fsnorkel%2Fblog%2Fweak_supervision.html&usg=AOvVaw1J2xzepcnBZ7ZZvlqMhD_f) for this task. Some of the notable works are [*Davidov et al.*](http://people.seas.harvard.edu/~orentsur/papers/conll10.pdf) and [*Gonzalez-Ibanez et al.*](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiexrj5g8nXAhUFy2MKHfVbBh4QFggoMAA&url=http%3A%2F%2Fciteseerx.ist.psu.edu%2Fviewdoc%2Fdownload%3Fdoi%3D10.1.1.207.5253%26rep%3Drep1%26type%3Dpdf&usg=AOvVaw3TiC7L_1qYmvOpAJXPT7oV)

### Models:

As we know, the problem of sarcasm detection can be modelled as a binary classification problem, though one can go ahead and predict the intensity of sarcasm by generating a range of scores. The common approach to any classification problem is to design a set of features, define the loss function and learn the parameters of the hypothesis function (complexity of the model used) to minimize the empirical loss. Though most of the past works agreed on the type of models used (typically a SVM), they widely vary on the choice of features. It is to be noted that, several works involving usage of HMMs (like [*Wang et al.*](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwi9u87glMnXAhUDKGMKHYjGBDkQFgg0MAE&url=http%3A%2F%2Fdl.acm.org%2Fcitation.cfm%3Fid%3D2960891&usg=AOvVaw01DUJcykZkf5K83aK-LpSc)) to model the sequential dependence between output labels have been observed in the past. Let us take a look at some of the most common features used and the performances of related systems.

### Feature Selection:

The NLP community has experimented with lots of features for sarcasm detection. Some of the first works used hand-made rules to detect indicators of sarcasm. For example, [*Veale and Hao*](https://www.semanticscholar.org/paper/Detecting-Ironic-Intent-in-Creative-Comparisons-Veale-Hao/acdba2f56e70184e5af9318b075d1980460470b2) used the presence of similes (* as a *) as indicators of sarcasm. This, sort of, makes sense because utterances like “my laptop is as cool as a furnace” are actually sarcastic, even though the mere usage of terms like “cool” indicates positive sentiment. Another similar work from [*Maynard and Greenwood*](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiZ8NClhsnXAhXGMGMKHXMdCdQQFggoMAA&url=https%3A%2F%2Fgate.ac.uk%2Fsale%2Flrec2014%2Farcomem%2Fsarcasm.pdf&usg=AOvVaw3x36YPgwho9O9tUKbvhGjB) used the presence of '#sarcasm' as indicators of sarcasm. The intuition here is that, only the author can precisely say whether it’s sarcastic or not. These hashtags serve as clues from the author to denote sarcasm. [*Riloff et al.*](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjTk9TLhsnXAhVRwWMKHXCvBxYQFggoMAA&url=https%3A%2F%2Fwww.cs.utah.edu%2F~riloff%2Fpdfs%2Fofficial-emnlp13-sarcasm.pdf&usg=AOvVaw0r86ArRt8o48Rx6MTNXUaP) presented a rule based classifier which looks for a positive sentiment in the context of a negative situational phrase. This is usually the case when sarcasm occurs in a negative context. For example, we have a conversation between a teacher and a student who hasn’t completed his/her homework. The teacher says "And this is how homework should be done". The quoted utterance, when stood alone, inform a positive sentiment, whereas one has to look at the context to understand the sarcasm. These are typically called negative situational contexts/phrases and convey important information about sarcasm.  

All these models fall on one end of the spectrum, while the other end consists of statistical approaches to sarcasm detection. Here, typically, one incorporates features (similar to the rules mentioned above) and feeds it to a machine learning model, which learns the conditional probability distribution of the output classes, given the features. [Some works](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiAr8iLiMnXAhUW3GMKHbKwCWoQFggtMAA&url=https%3A%2F%2Flink.springer.com%2Farticle%2F10.1007%2Fs10579-012-9196-x&usg=AOvVaw1tu5TMqAVjv0fSCgSImmS0) used lexical features like word counts and n-gram based features to predict sarcasm, while [some](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiexrj5g8nXAhUFy2MKHfVbBh4QFggoMAA&url=http%3A%2F%2Fciteseerx.ist.psu.edu%2Fviewdoc%2Fdownload%3Fdoi%3D10.1.1.207.5253%26rep%3Drep1%26type%3Dpdf&usg=AOvVaw3TiC7L_1qYmvOpAJXPT7oV) went one step further and incorporated sentiment features like the polarity of individual words to understand sarcasm. In addition, pragmatic features like emoticons and interjections were also used. [*Reyes et al.*](https://dl.acm.org/citation.cfm?id=2215319) used semantic relatedness as a key indicator of sarcasm. This is based on the intuition that, a sarcastic utterance is going to contradict with its context in terms of its sentiment. Thus, calculating the sentiment similarity scores between utterances in a conversation can be crucial to detecting sarcasm. One can also learn pattern-based features as indicators of sarcasm. For example, presence of a pattern like "as [ADJ] as a [NOUN]" with the [NOUN] and [ADJ] contradicting in sentiments can be a useful feature for sarcasm prediction. Essentially, one can mine all such features which are discriminative of the classes under consideration. 

### Deep Learning based approaches:

While all the above models fall into the category of classical machine learning, some works involving deep neural networks have also been observed recently. The idea here is that one can understand the context of a conversation using a recurrent neural network and use the context learned, to predict the presence of sarcasm. Word embedding based features, in place of standard lexical tokens, were also found to be effective for the task of sarcasm detection. One such work was from [*Huang et al.*](https://link.springer.com/chapter/10.1007/978-3-319-56608-5_45). The paper explores the use of CNNs, RNN and Attentive RNNs for sarcasm detection. Many works (like [this](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwimh9eplcnXAhUILmMKHZlDAEEQFggtMAA&url=http%3A%2F%2Fwww.aclweb.org%2Fanthology%2FD14-1181&usg=AOvVaw30IZcbN8tX8UF_jNFNiqNr)) have highlighted the use of CNNs for NLP tasks using word embeddings as features (Check out [this](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjU_vrAlcnXAhUI5mMKHRrWBBoQFggtMAA&url=http%3A%2F%2Fwww.wildml.com%2F2015%2F11%2Funderstanding-convolutional-neural-networks-for-nlp%2F&usg=AOvVaw1DjE0EXcwCJP49vUn7_H_C) really cool blog from [Denny Britz](http://www.wildml.com/about/) for more information on this). Convolutions of varying sizes are applied over the word vectors and subsequent max pooling results in an encoded vector for sentences in the corpus. This encoded vector can be used for classification tasks. The second model they used was an RNN, where the inputs are sequential data (can be sentences or words) and the output can be both sequential and single-valued classes. The benefit of RNNs is that they can model the context of the inputs, which, as we have seen, can help immensely in sarcasm prediction. The third model, they document, is an attention-based RNN. Attention allows RNNs to look back at specific parts of the input to make decisions. They have been shown to perform extremely good in several NLP-CV tasks like automatic image captioning. They take [*Ghosh et al*’s](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiHz8uklsnXAhUV9mMKHZvXCQ8QFggoMAA&url=http%3A%2F%2Fwww.emnlp2015.org%2Fproceedings%2FEMNLP%2Fpdf%2FEMNLP116.pdf&usg=AOvVaw3gkMiyz18Jz6RE8AWPAVD0) maximum-valued matrix element SVM kernel, which was the then state-of-the-art, as a baseline for comparison. The results of their work in comparison to the baseline is shown below:

<div class="imgcap">
<img src="/images/accuracy.JPG" style="display:block; margin-left: auto; margin-right: auto;">
	<div class="thecap" style="text-align:center;"> Courtesy: <a href="https://link.springer.com/chapter/10.1007/978-3-319-56608-5_45" target="_blank"> <i> Huang et al.</i> </a> </div>
</div>


As we can see, attentive RNNs were easily able to outperform state-of-the-art model by a large margin. The dataset, [*Ghosh et al*’s](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiHz8uklsnXAhUV9mMKHZvXCQ8QFggoMAA&url=http%3A%2F%2Fwww.emnlp2015.org%2Fproceedings%2FEMNLP%2Fpdf%2FEMNLP116.pdf&usg=AOvVaw3gkMiyz18Jz6RE8AWPAVD0) they used, was made of tweets, annotated with sarcasm labels. The attention weights, as learned by the model, were :

<div class="imgcap">
<img src="/images/attention_cis.JPG" style="display:block; margin-left: auto; margin-right: auto;">
	<div class="thecap" style="text-align:center;"> Courtesy: <a href="https://link.springer.com/chapter/10.1007/978-3-319-56608-5_45" target="_blank"> <i> Huang et al.</i> </a> </div>
</div>

The key observation, here, is that the model was able to give attention to the key words indicative of sarcasm. For example, in the sentence "I love bad news", instead of attending ‘love’ which is of positive sentiment, the model rightly attended 'bad', which is indicative of negative sentiment, and hence sarcastic.

### Conclusion:

In all, we went through a number of feature extraction and modelling techniques which were previously used by NLP researchers to solve the task of sarcasm detection. Though the current state-of-the-art models achieve an accuracy of ~90%, there is still plenty of room for improvement. Most of the datasets for this task were labelled using distant supervision. This essentially uses a noisy-labelling technique and hence not accurate. Though the evaluation set was manually labelled, they are very small (typically around 3k) and hence not exactly representative of real-world data. Since sarcasm detection is highly culture-specific, as quoted by [*Joshi et al.*](https://arxiv.org/abs/1602.03426), models for sarcasm detection for different languages may help us in understanding culture-specific traits in sarcasm. Future models incorporating multi-modal inputs like visual and speech signals may help us better in understanding the phenomenon of sarcasm in languages. 


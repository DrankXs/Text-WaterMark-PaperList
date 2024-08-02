# Text-WaterMark-PaperList

Text watermark is an important research direction for the misuse of LLM-generated text.
This repo will have the followings:

- [Text-WaterMark-PaperList](#text-watermark-paperlist)
- [1.BackGround](#1background)
  - [1.1 Application](#11-application)
  - [1.2 Classification](#12-classification)
    - [1.2.1 Bit Count](#121-bit-count)
    - [1.2.2 Embedding Phase](#122-embedding-phase)
    - [1.2.3 Generating-Process](#123-generating-process)
    - [1.2.4 Token-Level](#124-token-level)
    - [1.2.5 Different Scenarios](#125-different-scenarios)
  - [1.3 Necessary Features](#13-necessary-features)
- [2.Watermark Paper](#2watermark-paper)
- [Reference](#reference)

# 1.BackGround

## 1.1 Application

- Deep Fake Detection: Detect whether the target text is generated by a language model;
- Deep Fake Attribution: Determine which model/user generated the target text;
- IP Protection: Protect valuable text.

## 1.2 Classification

<!-- - [Bit Count](#121-bit-count)
  - Zero-Bit
  - Multi-Bit
- [Embedding Phase](#122-embedding-phase)
  - Backdoor/A-Priori
  - [Generating-Process](#123-generating-embedding-phase)
    - Token Level
      - Logits Bias
      - Reweighting
      - Sampling
    - Sentence Level
  - Post-Hoc
- [Different-Scenarios](#124-different-scenarios)
  - Competion
  - Condition
  - Code -->

### 1.2.1 Bit Count

Distinguished based on the amount of information that can be embedded in the watermark.

- **Zero-Bit**: Only determine whether to add watermark, no additional information. This makes the watermark only detect, no attribution effect.
- **Multi-Bit**: Embed and extract multi-bit information, you can freely add the content you want to embed, such as time, user ID, etc. Multi-Bit watermark is necessary for deep fake attribution.

### 1.2.2 Embedding Phase

Text watermark research existed in the last century, at which time it was only possible to add watermarks to already written text, and was classified as Post-hoc. However, with the development of language models, some new stages of text watermark emerges.

- **Backdoor/A-Priori**: By employing specific methods to utilize watermark information to form a toxic dataset, fine-tuning or training a language model can result in *a language model with watermark*. The introduction of watermark information occurs *before the language model generates text*.
- **Generating-Process**: The introduction of watermark information will interfere with the generation process of the language model, resulting in the language model producing text with embedded watermarks. The addition of such watermarks does not require modification of the language model, but it necessitates access to the complete language model.
- **Post-Hoc**: By altering the text to introduce watermark information, the watermarked text is constructed. This kind of method does not require the original generative language model; it only requires the text generated by the model.

### 1.2.3 Generating-Process

This category represents a further subdivision within Category [Embedding Phase~Generating-Process](#122-embedding-phase).

- **Token-Level**: Watermark information is introduced when the language model generates a token each time.
- **Sentence-Level**: The embedding of watermark information is used when the language model generates each sentence. This requires a semi-controlled sampling process when generating: 1.No constraint is imposed until a sentence is generated;2.Multiple candidates must be obtained using beam search during generation; 3.When generated, it must be generated sentence by sentence.

### 1.2.4 Token-Level

This category represents a further subdivision within Category [Embedding Phase~Generating-Process~Token-Level](#123-generating-process).

During each step of token generation, the language model undergoes the following processing:
Get **Logits** ob the vocabulary $\rightarrow$ Get Probability Distribution(**Weight**) over the vocabulary $\rightarrow$ **Sample** from the probability distribution

- **Logits Bias**: The watermark information is converted into a logits distribution on the vocabulary, which is added to the logits distribution generated by the original language model to interfere with the generation phase.
- **Reweighting**: The watermark information will guide reweighting probability distribution. This typically involves scrambling the vocabulary and selecting a re-weighting interval.
- **Sampling**: Watermark information is embedded by influencing the generation results through sampling, which prohibits the commonly used decoding methods and restricts the decoding approach.

### 1.2.5 Different Scenarios

- **Completion**: Normal Generation. Given a prompt and complete it. Most watermarks are configured for this type of scenario.
- **Condition**: Conditional text generation. Such tasks are typically in the form of question-and-answer (QA) and summarization tasks.
- **Code**: Code generation. Generating executable code.

## 1.3 Necessary Features

The addition of watermarks often involves a trade-off among multiple aspects.

- **Detectability**: Detectability refers to the capability of a watermarking method to distinguish between watermarked text and non-watermarked text.
  Metric: Typically, it is a binary classification metric, and in the case of multiple bits, there are corresponding multi-bit comparison metrics.
  - TPR/TNR/FPR/FNR
  - AUROC
  - Accuracy
  - TPR@FPR=X%
  - Bit Accuracy/Bit Error Rate(Multi-Bit)
- **Invisibility/Text Quality**: Invisibility refers to the impact of the watermark on the quality of the generated text.
  Metric: Typically, the text quality metrics of the original generated text and the watermarked text are compared.
  - Perplexity(PPL)
  - BLEU
  - ROUGE(-n)
  - BERTScore
  - Entailment Score(ES)
  - Sentence Similarity
  - Human Evaluate
  - Ent-3[^Ent3]
  - Rep-3[^Rep3]
- **Imperceptibility**: Imperceptibility indicates the impact of watermark addition on the overall token distribution, focusing more on the statistical differences between watermarked and non-watermarked texts, which requires a large amount of corresponding text for overall estimation.
  Metric: Imperceptibility-related metrics often involve token-level statistics for the overall generated text. It assesses whether there are differences in the token distribution between watermarked texts and non-watermarked texts.
  - Word Frequency
- **Robustness**: Robustness refers to the ability of watermarked text to still be recognized as watermarked after undergoing watermark removal attacks.
  Metric: Robustness-related metrics are represented by comparing the changes in detectability indicators caused by attacks.
- **Usability**: Usability refers to the additional time and memory consumption caused by the addition of watermarks. These costs must be within an acceptable range for the user.
  Metric: Usability metrics typically consist of corresponding values for watermarking time and memory usage.
  - Time Cost
  - Memory Cost

# 2.Watermark Paper

About Key Word:

* The keywords we set are used to describe the most prominent features of the corresponding paper, the absence of some keywords does not mean that the paper does not cover those aspects.
  For example:Zero-Bit, Completion, Detectability, Logits Bias, Token Level.
* Some watermark already have classic abbreviations, which we have also included in the keywords.

|                                                                                                     Paper                                                                                                     | Proceedings / Journal-Year | Key Word                                     | Note                                                                                                                                                                   | Code                                                                                                |
| :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | -------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
|                                                 [A watermark for large language models](https://proceedings.mlr.press/v202/kirchenbauer23a/kirchenbauer23a.pdf)                                                 | ICML-2023                  | Zero-Bit,Logits Bias,KGW                     | The classical method uses watermark information<br /> to divide the vocabulary into red and green list, <br />and increases the logits of the green list.              | [https://github.com/jwkirchenbauer/lm-watermarking](https://github.com/jwkirchenbauer/lm-watermarking) |
|                                 [Protecting Intellectual Property of Language Generation APIs with Lexical Watermark.](https://ojs.aaai.org/index.php/AAAI/article/view/21321)                                 | AAAI-2022                  | Post-Hoc, Lexical                            | Lexical substitute                                                                                                                                                    | [https://github.com/xlhex/NLG_api_watermark.git](https://github.com/xlhex/NLG_api_watermark.git)       |
| [CATER: Intellectual Property Protection on Text Generation APIs via Conditional Watermarks.](https://proceedings.neurips.cc/paper_files/paper/2022/file/2433fec2144ccf5fea1c9c5ebdbc3924-Paper-Conference.pdf) | NeurIPS-2022               | Post-Hoc,Condition                           | Imporve[Protecting Intellectual Property of <br />Language Generation APIs with Lexical Watermark.](https://ojs.aaai.org/index.php/AAAI/article/view/21321)               | [https://github.com/xlhex/cater_neurips](https://github.com/xlhex/cater_neurips)                       |
|                           [Adversarial Watermarking Transformer: Towards Tracing Text Provenancewith Data Hiding.](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9519400)                           | SP-2021                    | AWT,Multi-Bit,Post-Hoc                       | Train watermark encoder and decoder, embed and<br /> extract watermark to exist text.                                                                                  | [https://github.com/salesforce/awd-lstm-lm](https://github.com/salesforce/awd-lstm-lm)                 |
|                                            [Protecting Language Generation Models via Invisible Watermarking](https://proceedings.mlr.press/v202/zhao23i/zhao23i.pdf)                                            | ICML-2023                  | Reweighting                                  | -                                                                                                                                                                      | [https://github.com/XuandongZhao/Ginsew](https://github.com/XuandongZhao/Ginsew)                       |
|                                                          [Watermarking Pre-trained Language Models with Backdooring](https://arxiv.org/pdf/2210.07543)                                                          | 2022                       | Backdoor                                     | -                                                                                                                                                                      | -                                                                                                   |
|                  [DeepTextMark Deep Learning based Text Watermarking for Detection of Large Language Model Generated Text](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10471537)                  | IEEE-2024                  | Post-Hoc                                     | Word substitute and candidate sentence selection                                                                                                                      | -                                                                                                   |
|                                             [Tracing Text Provenance via Context-Aware Lexical Substitution](https://ojs.aaai.org/index.php/AAAI/article/view/21415)                                             | AAAI-2022                  | Post-Hoc,Multi-Bit                           | Choose candidate word positon and use Bert to<br /> choose words.                                                                                                      | -                                                                                                   |
|                                                            [Towards Tracing Code Provenance with Code Watermarking](https://arxiv.org/pdf/2305.12461)                                                            | 2023                       | Code, Post-Hoc                               | -                                                                                                                                                                      | -                                                                                                   |
|                                                           [Watermarking Text Generated by Black-Box Language Models](https://arxiv.org/pdf/2305.08883)                                                           | 2023                       | Post-Hoc                                     | MLM generates candidate words<br /> and scores candidate sentences                                                                                                     | [https://github.com/Kiode/Text_Watermark](https://github.com/Kiode/Text_Watermark)                     |
|                                                            [Who Wrote this Code? Watermarking for Code Generation](https://arxiv.org/pdf/2305.15060)                                                            | 2024                       | SWEET, Logits Bias,<br /> Code, Invisibility | Use entropy threshold to control logits bias,<br />Improve [KGW](https://proceedings.mlr.press/v202/kirchenbauer23a/kirchenbauer23a.pdf)                                  | [https://github.com/hongcheki/sweet-watermark](https://github.com/hongcheki/sweet-watermark)           |
|                                 [Are You Copying My Model? Protecting the Copyright of Large Language Models for EaaS via Backdoor Watermark](https://arxiv.org/pdf/2305.10036)                                 | ACL-2023                   | Backdoor                                     | -                                                                                                                                                                      | [https://github.com/yjw1029/EmbMarker](https://github.com/yjw1029/EmbMarker)                           |
|                                           [Robust Multi-bit Natural Language Watermarking through Invariant Features](https://aclanthology.org/2023.acl-long.117.pdf)                                           | ACL-2023                   | Post-Hoc,Multi-Bit                           | MLM generate candidate words, Select the<br /> candidate at the corresponding position<br /> to correspond to the watermark information                               | [https://github.com/bangawayoo/nlp-watermarking](https://github.com/bangawayoo/nlp-watermarking)       |
|                                                                 [Undetectable Watermarks for Language Models](https://arxiv.org/pdf/2306.09194)                                                                 | IACR-2023                  | Sample                                       | The embedding process of watermark<br /> is controlled by entropy                                                                                                      | -                                                                                                   |
|                                                            [Robust Distortion-free Watermarks for Language Models](https://arxiv.org/pdf/2307.15593)                                                            | 2023                       | Sample,EXP, EXP-Edit                         | The sampling related parameters are<br /> generated by the secret key sequence<br /> to interfere with the sampling                                                    | [https://github.com/jthickstun/watermark](https://github.com/jthickstun/watermark)                     |
|                                       [SemStamp: A Semantic Watermark with Paraphrastic Robustness for Text Generation](https://aclanthology.org/2024.naacl-long.226.pdf)                                       | NAACL-2024                 | Sentence-Level,<br />Semstamp                | Sentence level red/green list, imporve$\ $ [KGW](https://proceedings.mlr.press/v202/kirchenbauer23a/kirchenbauer23a.pdf),                                               | [https://github.com/bohanhou14/SemStamp](https://github.com/bohanhou14/SemStamp)                       |
|                                   [Three Bricks to Consolidate Watermarks for Large Language Models](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10374576&tag=1)                                   | WIFS-2023                  | Logits-Bias                                  | imporve$\ $[KGW](https://proceedings.mlr.press/v202/kirchenbauer23a/kirchenbauer23a.pdf), Change the detection method to <br />improve the relative detection accuracy | -                                                                                                   |
|                                                                                                                                                                                                              |                            |                                              |                                                                                                                                                                        |                                                                                                     |

# Reference

[^Ent3]: [Generating informative and diverse conversational responses via adversarial information maximization.](https://proceedings.neurips.cc/paper_files/paper/2018/file/23ce1851341ec1fa9e0c259de10bf87c-Paper.pdf)
    
[^Rep3]: [Neural text generation with unlikelihood training](https://openreview.net/forum?id=SJeYe0NtvH)
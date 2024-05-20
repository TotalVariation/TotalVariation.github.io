---
layout: distill
output:
  distill::distill_article:
    toc: true
    toc_depth: 2
title: A Primer on Multimodal Large Language Models
description: Towards Building Doraemon-like AI Assistants
tags: MLLMs, LVLMs, General Purpose Visual Assistants, GenAI
giscus_comments: true
date: 2024-05-20
featured: true

authors:
  - name: Xin Cai
    url: "https://totalvariation.github.io/"

bibliography: 2024-05-20-a-primer-on-mllms.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
#toc:
#  - name: Equations
#    # if a section has subsections, you can add them as follows:
#    # subsections:
#    #   - name: Example Child Subsection 1
#    #   - name: Example Child Subsection 2
#  - name: Citations
#  - name: Footnotes
#  - name: Code Blocks
#  - name: Interactive Plots
#  - name: Layouts
#  - name: Other Typography?

toc:
  - name: Background
  - name: Technical Overview
    subsections:
      - name: Architectures
      - name: Training Strategies and Data
      - name: Evaluation
  - name: Concluding Remarks
  - name: Acknowledgement

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }
---

<blockquote>
<p>The best material model of a cat is another, or preferably the same, cat.</p>
<footer>- Norbert Wiener</footer>
</blockquote>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-1">
        {% include figure.liquid path="assets/img/mllm_primer_post_robot_cat_sdxl.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig-1 A sketch of general purpose AI assistants. Image Source: <a href="https://huggingface.co/docs/diffusers/en/using-diffusers/sdxl" target="_blank">Stable Diffusion XL</a>.
</div>

## Background

Large Language Models (LLMs) <d-cite key="zhao2023survey"></d-cite> have achieved remarkable milestones, especially the demonstrated emergent capabilities, e.g., In-Context Learning (ICL), Chain of Thought (CoT) reasoning, and instruction following. It is now an inevitable trend to integrate various modalities with LLMs to mimic the way humans interact with the open world, thus ushering in the new era of Multimodal Large Language Models (MLLMs <d-footnote>Please note that the term MLLMs hereafter is used interchangeably with Large Vision-Language Models (LVLMs) or Large Multimodal Models (LMMs) given that the transitive property of modality embeddings showcased in ImageBind. <d-cite key="girdhar2023imagebind"></d-cite> </d-footnote>) <d-cite key="yin2023survey"></d-cite>.

Recent work <d-cite key="li2023multimodal"></d-cite> has demonstrated that the development of MLLMs signifies a transition from specialist models to general-purpose visual assistants. This transition is regarded as a crucial step towards realizing the grand vision of a general-purpose multimodal AI agent, akin to bringing the beloved Japanese cartoon character Doraemon to life. It is further unveiled in <d-cite key="li2023multimodal"></d-cite> that a converging point for many development trajectories of vision-language/multimodal foundation models is *"the creation of general-purpose models and systems capable of following human intents and effortlessly executing a diverse array of vision and vision-language tasks in the wild."*

The key features that markedly differentiate MLLMs from deep learning models developed beforehand are condensed into the name itself: <mark>General Purpose Assistants</mark>, which include **versatility**, **interactivity**, and **controllability**. Specifically, the quest for a unified architecture capable of accomplishing a diverse range of discriminative and generative tasks has witnessed great success in Natural Language Processing (NLP), as exemplified by GPT-3, inspiring similar research endeavours in the field of computer vision. However, as pointed out in <d-cite key="li2023multimodal"></d-cite>, the development of unified vision systems significantly lags behind due to fragmented vision tasks and the difficulty of scaling up visual data. Furthermore, to enable seamless communication between machines and humans, natural language serves as a core component in building a general interactive interface, which can be further complemented by multimodal prompts such as clicks or scribbles provided by users. Additionally, researchers have made strides in steering the output behaviour of MLLMs by instruction tuning or aligning with human preferences, marking a hallmark in elevating machine intelligence to the next level as humans begin to positively intervene in the machine learning process. By leveraging human feedback and rich experience, machines can evolve more effectively and efficiently, leading to better outcomes for both machines and humans. Inspired by the interactions between Doraemon and Nobita, the ultimate goal of general-purpose AI assistants is to establish a **symbiotic relationship** between humans and AI systems, where AI systems not only enhance the overall well-being and quality of human life but also evolve by engaging in humans' daily activities.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-2">
        {% include figure.liquid path="assets/img/mllm_primer_post_genaisys.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig-2 The general architecture of GenAISys. Image Source: <a href="https://jmtomczak.github.io/blog/21/21_genaisys.html" target="_blank">Jakub Tomczak</a>.
</div>

Before delving into the technical aspects of MLLMs, I would like to introduce an intriguing and inspiring concept called Generative AI Systems (GenAISys), recently introduced in a [blog post](https://jmtomczak.github.io/blog/21/21_genaisys.html) by Jakub Tomczak, which offers a novel perspective on MLLMs. In essence, GenAISys can be considered as end-to-end trainable MLLMs enhanced by external tools or databases, resonating with a similar idea proposed in Chapter 6 of the work <d-cite key="li2023multimodal"></d-cite>, where the authors indicate the possibility of combining strengths from two existing modelling paradigms of MLLMs: training or chaining tools with LLMs. For more insights into GenAISys and its future directions, I encourage you to refer to the original post by Jakub Tomczak.

## Technical Overview

The recent progress of MLLMs <d-footnote>Technically, the progress reviewed here excludes that related to Multimodal Agents, i.e., chaining tools with LLMs.</d-footnote> can be roughly divided into the following three aspects: 1) architectures, 2) training strategies and data, 3) evaluation benchmarks.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-3">
        {% include figure.liquid path="assets/img/mllm_primer_post_mllm_arch.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig-3 An overview of MLLM architecture.
</div>

### Architectures

A typical neural architecture of MLLMs consists of pre-trained modality encoders, pre-trained LLMs, learnable modality adapters/connectors, and optional modality decoders. 

**Modality Encoders** When compressing raw images, it is common practice to utilize pre-trained CLIP <d-cite key="radford2021learning"></d-cite> image encoders or other similar variants <d-cite key="sun2023eva"></d-cite>, leveraging a pre-aligned albeit coarse vision-semantic space. The underlying principle is to discretize/standardize modality embeddings with reference to language vocabulary, which can be extended to encoding other kinds of multimodal signals. For instance, the ImageBind <d-cite key="girdhar2023imagebind"></d-cite> encoder, used in ImageBind-LLM <d-cite key="han2023imagebind"></d-cite>, can construct a joint embedding space for six different modalities—images, text, audio, depth, thermal, and Inertial Measurement Unit (IMU) data—established by aligning a pair of modalities each time with InfoNCE <d-cite key="oord2018representation"></d-cite> loss.

Another critical factor in enriching visual embeddings in MLLMs and mitigating multimodal hallucination is increasing input resolution <d-cite key="liu2023improved"></d-cite> while not incurring significant computational overheads. Following this principle, dual vision encoders are employed in Mini-Gemini <d-cite key="li2024mini"></d-cite>, where the coarse embeddings from the pre-trained CLIP image encoder are complemented by dense visual features output from the LAION-pretrained <d-cite key="schuhmann2022laion"></d-cite> ConvNeXt <d-cite key="liu2022convnet"></d-cite>, which serves as the high-resolution vision encoder.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-4">
        {% include figure.liquid path="assets/img/mllm_primer_post_adc.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig-4 An illustration of workings of Analog-to-Digital Converters (ADC). Image Source: <a href="https://www.doeeet.com/content/eee-components/actives/analog-to-digital-converters-the-resolution-dilemma/" target="_blank">DOEEET</a>.
</div>

Finally, I would like to highlight an intriguing perspective by drawing a parallel between the modality encoder in MLLMs and the Analog-to-Digital Converter (ADC), which converts continuous analog signals into discrete digital signals through sampling and quantization. By establishing such a rough equivalence, we can gain deeper insights into the design rationale of the modality encoder. Specifically, bandwidth and Signal-to-Noise Ratio (SNR) play a crucial role in determining the overall performance and quality of an ADC. Analogously, the range of frequencies in input signals that can be captured by the modality encoder contributes to the overall performance of MLLMs, justifying the utilization of dual vision encoders, e.g., in Mini-Gemini <d-cite key="li2024mini"></d-cite>. As demonstrated in <d-cite key="park2022vision"></d-cite>, multi-head self-attention (MSAs) in Vision Transformers (ViT) <d-cite key="dosovitskiy2020image"></d-cite> function as a low-pass filter, while convolution operations act oppositely as a high-pass filter. Therefore, the convolutional branch supplements the ViT with high-frequency information, expanding the bandwidth of the modality encoder. Furthermore, image-text contrastive pre-training can be seen as a quantization process, where discrete codes (i.e., vocabulary indices) are allocated to encoded visual signals by filtering out noise and low-level information while maximally preserving mutual information. By analogy with a higher SNR leading to finer resolution of an ADC, the ability to discern the smallest semantic variation of the modality encoder can be improved by adopting fine-grained contrastive loss <d-cite key="li2022grounded"></d-cite> or detailed captioning <d-cite key="yu2022coca"></d-cite>.

**LLMs** Regarding pre-trained LLMs, open-source models such as the LLaMA series <d-cite key="touvron2023llama"></d-cite> <d-cite key="touvron2023llama2"></d-cite> and Vicuna family <d-cite key="chiang2023vicuna"></d-cite> have gained widespread popularity in academic research. These models are often combined with Parameter-Efficient Fine-Tuning (PEFT) techniques <d-cite key="han2024parameter"></d-cite>, such as Low-Rank Adaptation (LoRA) <d-cite key="hu2021lora"></d-cite>, in the Supervised Fine-Tuning (SFT) phase to enhance instruction following and alignment with human preference. Furthermore, LLMs constructed with a Mixture of Experts (MoE) <d-cite key="shen2023mixture"></d-cite> <d-cite key="jiang2024mixtral"></d-cite> have attracted increasing attention due to the reduced computational cost of the sparse architecture. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-5">
        {% include figure.liquid path="assets/img/mllm_primer_post_microcontroller.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig-5 A schematic diagram of microcontrollers, where the central unit CPU interacts with various components to process data, control and communicate with other devices. Image Source: <a href="https://www.hnhcart.com/blogs/microcontrollers/what-are-the-major-factors-on-which-microcontrollers-are-differentiated" target="_blank">The Major Factors On Which The Microcontrollers Are Differentiated</a>.
</div>

Compared to human-like text generation, the emergent capabilities of LLMs, such as ICL and CoT reasoning, unlock the vast potential of machines in handling complex and flexible human instructions. When LLMs are extended with interfaces to sense and interact with the physical world, i.e., MLLMs, they can be analogized to microcontrollers, provided disregarding differences in energy consumption and application scenarios. A microcontroller is essentially a small computer, comprising a processor core, memory, and input/output (I/O) peripherals. Similarly, LLMs act as the processor core, executing human instructions rather than pre-programmed commands. Modality encoders and decoders enable LLMs to interact with the external world, analogous to ADCs or Digital-to-Analog Converters (DACs). The key distinction between microcontrollers and MLLMs lies in the latter's capability for learning and adaptation, ushering in a new era where machines can continuously evolve through direct interaction with humans. Human language is distinguished by its complexity, generativity, and ability to convey abstract and displaced concepts, which supports the language-centric design pattern of MLLMs. However, this design is yet to be definitively validated, as discussed in Chapter 4 of <d-cite key="li2023multimodal"></d-cite>, particularly considering the distinct differences between vision and language.

**Modality Connectors** Due to the noticeable gap between embeddings of natural languages and other modalities, particularly considering the difference in bandwidth, it is essential to establish reliable alignment with reference to textual embeddings to enable LLMs to understand sensory inputs. This alignment involves projecting embeddings from modality encoders into a space comprehensible to LLMs, ensuring unambiguous interpretation. This can be achieved using a trainable modality adapter or connector, bridging the frozen modality encoder and the LLM, which is computationally efficient and more accessible.

Currently, multimodal alignment can be achieved at either the token- or feature-level. At the token-level, modality-specific embeddings are converted into corresponding tokens, which are then concatenated with textual tokens and fed into LLMs for SFT. The modules commonly used for modality conversion include Q-Former <d-cite key="li2023blip"></d-cite> <d-cite key="zhang2023video"></d-cite>, MLP <d-cite key="liu2024visual"></d-cite> <d-cite key="liu2023improved"></d-cite>, and cross-attention layers <d-cite key="alayrac2022flamingo"></d-cite>. In contrast to lightweight modality adapters, a pre-trained LLaMA is utilized as the modality connector in InternVL <d-cite key="chen2023internvl"></d-cite>, acting as a language middleware. This approach may allow for the utilization of frozen LLMs in the SFT stage. Feature-level fusion involves deep interaction between modality-specific and language features, usually achieved by inserting extra trainable modules into a frozen LLM <d-cite key="alayrac2022flamingo"></d-cite> <d-cite key="zhang2023llama"></d-cite>, which may compromise the innate language capabilities.

The difficulty in multimodal alignment may stem from the next-token prediction loss used in optimizing LLMs, which may favor an over-reliance on the LLM's parametric knowledge, as discussed in <d-cite key="zhai2023halle"></d-cite>, rather than faithfully grounding predictions in multimodal signals. Consequently, this can lead to multimodal hallucinations, where the model generates outputs that are inconsistent with the concrete content contained in sensory inputs. Another factor significantly influencing the generation of LLMs is the adopted decoding strategies. Recent work <d-cite key="leng2023mitigating"></d-cite> has shown that multimodal hallucination can be alleviated by adapting advanced decoding methods, such as contrastive decoding <d-cite key="li2022contrastive"></d-cite>, for MLLMs as a training-free corrective mechanism. Apart from using modality connectors, another viable solution is to textualize non-language signals using expert models, as exemplified by the VideoChat-Text model <d-cite key="li2023videochat"></d-cite>, though this approach comes with the downsides of information loss and a pipeline that is not end-to-end optimized.

**Modality Decoders** In addition to the aforementioned three compulsory modules, MLLMs can be further extended with modality-specific decoders, particularly in the Any-to-Any workflow, to overcome the limitation of MLLMs only perceiving multimodal data. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-6">
        {% include figure.liquid path="assets/img/mllm_primer_post_nextgpt.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig 6 An overview of an Any-to-Any multimodal system NExT-GPT that can perceive inputs and generate outputs in arbitrary combinations of text, images, videos, and audio. Image Source: <a href="https://arxiv.org/abs/2309.05519" target="_blank">NExT-GPT: Any-to-Any Multimodal LLM</a>.
</div>

MLLMs with multimodal responsive capabilities have recently spurred a surge of interest in the area of Generative AI, regarded as a significant step towards Artificial General Intelligence (AGI). For image generation, recent works such as Emu <d-cite key="sun2023generative"></d-cite> and Mini-Gemini <d-cite key="li2024mini"></d-cite> default to using diffusion-based generative models <d-cite key="luo2022understanding"></d-cite> <d-cite key="ribeiro2024demystifying"></d-cite> <d-cite key="chan2024tutorial"></d-cite>, such as the Stable Diffusion family <d-cite key="rombach2022high"></d-cite>, due to their unparalleled text-to-image generation performance. The work NExT-GPT <d-cite key="wu2023next"></d-cite> further extends LLMs to include image, audio, and video diffusion models, enabling content creation in arbitrary combinations of image, text, audio, and video. Similar to the modality connectors used in the encoding phase, it is reasonable to bridge the gap between textual embeddings generated by LLMs and those used as conditional guidance in diffusion models, which can be achieved through decoder-side alignment.

### Training Strategies and Data

The training process of MLLMs can be decomposed into three stages: 1) pre-alignment, 2) instruction tuning, 3) alignment with human preferences. The fulfilment of training objective in each phase heavily relies on specific data, necessitating cost-effective data scaling-up methods and high quality data.

**Pre-Alignment** The pre-alignment stage often requires an enormous amount of text-paired data, such as images, videos, or audio files with associated textual descriptions, usually gathered from the Internet. However, the innate nature of LLMs as generative pre-trained models (next-token-prediction) casts doubt on the validity of using noisy and short text-paired data, which is originally used for contrastive alignment. As demonstrated in works such as ShareGPT4V <d-cite key="chen2023sharegpt4v"></d-cite>, both the pre-alignment and SFT stages significantly benefit from highly descriptive caption data. Additionally, the progressive alignment scheme adopted in InternVL <d-cite key="chen2023internvl"></d-cite>, transitioning from vision-language contrastive learning to generative pre-training, has proven to be more effective in accommodating the heterogeneous sources of pre-training data.

**Instruction Tuning** Instruction tuning (IT) <d-cite key="zhang2023instruction"></d-cite> aims to steer LLMs to respond more faithfully to human instructions by fine-tuning on instruction following datasets, which consist of (Instruction, Input, Output) triplets. This technique has proven to be effective and computationally efficient in enhancing the controllability of LLMs' output behavior, thereby eliciting knowledge from LLMs that is well-aligned with human intents. Furthermore, IT has been identified as a critical factor in unlocking the few-shot (ICL) or zero-shot generalization capability of LLMs on solving novel tasks.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-7">
        {% include figure.liquid path="assets/img/mllm_primer_post_self_instruct.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig 7 An illustration of the overall process of Self-Instruct. Image Source: <a href="https://arxiv.org/abs/2212.10560" target="_blank">SELF-INSTRUCT: Aligning Language Models with Self-Generated Instructions</a>.
</div>

The current trend in scaling up IT data involves leveraging semi-automatic data engines to minimize human intervention. Specifically, the majority of IT datasets are constructed using a paradigm known as Self-Instruct <d-cite key="wang2022self"></d-cite>, where a few manually-crafted seed examples serve as demonstrations for LLMs, such as GPT-4, to generate additional similar samples by harnessing the ICL capability. This approach can be extended to generate multimodal IT data. For instance, in the pioneering work LLaVA <d-cite key="liu2024visual"></d-cite>, images are converted into text descriptions and bounding box coordinates, thereby endowing the text-only GPT-4 with an imaginary vision capability, which are then combined with diverse task prompts to create visual instruction-following datasets. However, IT data built on top of GPT-4 (text-only LLMs) often suffer from multimodal hallucination, compromising the quality of the generated samples, which can be remedied with the advent of powerful MLLMs. For instance, in the work ALLaVA <d-cite key="chen2024allava"></d-cite>, GPT-4V, a multimodal variant of GPT-4, is utilized to create high-quality IT data by following a captioning-questioning-answering pipeline. Moreover, recent works, such as ALLaVA <d-cite key="chen2024allava"></d-cite> and Lynx <d-cite key="zeng2023matters"></d-cite>, emphasize that the quality of IT datasets—specifically the complexity and diversity of instructions and the detail of responses—is more crucial than their quantity. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-8">
        {% include figure.liquid path="assets/img/mllm_primer_post_data_engine.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig 8 A schematic diagram of semi-automatic data engine.
</div>

Despite the promising performance gains achieved through IT, there have been criticisms regarding its efficacy. For instance, it has been shown that instruction-tuned LLMs merely mimic the style of proprietary models like ChatGPT and fall short in those critical dimensions such as factuality, coding, and problem solving <d-cite key="kung2023models"></d-cite> <d-cite key="gudibande2023false"></d-cite>, which has led to the conclusion that bridging the capabilities gap between open-source LLMs and proprietary ones through IT is considered a false promise <d-cite key="gudibande2023false"></d-cite>. Nevertheless, leveraging a scalable semi-automatic data engine-an iteratively refined pseudo data labelling/generation pipeline with human in the loop-is a key enabler for advancing MLLMs. For example, the All-Seeing Project <d-cite key="wang2023all"></d-cite>, a recent effort in building an open-world visual recognition and understanding model, created approximately 1 billion instance-level annotations of open-world visual concepts with detailed captions and question-answer pairs, which would have been prohibitive without the assistance of semi-automatic data engines.

**Preference Alignment** Alignment with human preferences aims to further refine the output behaviour of MLLMs that have undergone SFT or instruction tuning by leveraging human/AI feedback on model responses. The two mainstream solutions currently in use are Reinforcement Learning with Human Feedback (RLHF) <d-cite key="ziegler2019fine"></d-cite> <d-cite key="ouyang2022training"></d-cite> and Direct Preference Optimization (DPO) <d-cite key="rafailov2024direct"></d-cite>.

### Evaluation

The evaluation of MLLMs is generally categorized into closed-ended and open-ended questions. For open-ended questions, the assessment criteria may involve human or GPT scoring. In contrast to evaluation methods developed before the era of LLMs, designing evaluation toolkits for MLLMs demands attention and efforts comparable to or even surpass those required for model development. As pointed out in <d-cite key="gudibande2023false"></d-cite>, human evaluators without domain expertise or significant time investment can be easily deceived by LLMs due to their fluent, confident, and well-structured responses. Therefore, acquiring quantitative measurements that can objectively and reliably reflect the multifaceted aspects of MLLMs is crucial, as these measurements facilitate pinpointing critical factors contributing to improvements along specified skill dimensions, identifying failure cases, and providing deeper insights into the inner workings of MLLMs.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0" id="figure-9">
        {% include figure.liquid path="assets/img/mllm_primer_post_ai_dev_cycle.png" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Fig 9 An illustration of AI development cycle: a dynamic and iterative process where data creation, model development, and model assessment continuously inform and improve each other.
</div>

We should be wary of increasingly potent MLLMs that can take shortcuts to inflate performance metrics and deliver delusive results, which requires researchers to act as adversaries, critically analyzing and probing the models' responses for weaknesses. Consequently, evaluation frameworks that combine both human and AI efforts and can continuously evolve offer promising avenues to mitigate these issues, as exemplified in Dynabench <d-cite key="kiela2021dynabench"></d-cite>, where dataset creation, model development, and model evaluation co-evolve and reinforce each other, serving as a counterbalance to the phenomenon where state-of-the-art AI systems quickly saturate benchmarks but fail on real-world challenges.

## Concluding Remarks

The development of LLMs and their extension to multimodal understanding (MLLMs) reveals a promising pathway for machines to approach human-level intelligence: by actively interacting with humans and engaging in human daily activities, reminiscent of how we nurture and educate our offspring. In essence, the emergence of MLLMs marks a watershed moment in the evolution of machine learning systems, ushering in the era of Human-Centered AI (HCAI), which begins with the creation of a more natural and intuitive human-machine interaction interface. Reflecting on the saying of Richard Feynman that "What I cannot create, I do not understand," we can anticipate significant breakthroughs in neuroscience when AI systems can faithfully emulate human cognitive capabilities by generating responses indistinguishable from those of humans. The development of AI assistants akin to Doraemon can provide profound insights into the neural and cognitive mechanisms underlying human intelligence and foster a closer synergy between neuroscience and AI research. While Norbert Wiener's saying, "The best material model of a cat is another, or preferably the same, cat," underscores the immense challenge of precisely simulating the complexity and intricacies inherent in biological systems with artificial models, it is exciting to contemplate the potential of an increasingly close collaborative relationship between humans and AI. 

## Acknowledgement

This blog post draws significant inspiration from two outstanding works reviewing the latest advances in MLLMs:

1. "A Survey on Multimodal Large Language Models" <d-cite key="yin2023survey"></d-cite>, which provides a timely and comprehensive overview of recent progress in MLLMs, supplemented by a continuously updated GitHub webpage: [Awesome-Multimodal-Large-Language-Models](https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models).

2. "Multimodal Foundation Models: From Specialists to General-Purpose Assistants" <d-cite key="li2023multimodal"></d-cite>, where leading researchers in the field present an insightful dissection, offering both a historical perspective on the development of multimodal foundation models and an inspiring vision for their future.

Additionally, the completion of this blog post was assisted by OpenAI ChatGPT in refining phrasing, expression, and tone.

{% raw %}

```html
@misc{Cai2024mllm_primer,
    author = {Xin Cai},
    title = {A Primer on Multimodal Large Language Models: Towards Building Doraemon-like AI Assistants},
    howpublished = {\url{https://totalvariation.github.io/blog/2024/a-primer-on-mllms/}},
    note = {Accessed: 2024-05-20},
    year = {2024}
}
```

{% endraw %}


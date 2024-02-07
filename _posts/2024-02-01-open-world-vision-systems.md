---
layout: distill
title: Random Thoughts on Open World Vision Systems
description: 
tags: Open-set Open-vocabulary UDA
giscus_comments: true
date: 2024-02-01
featured: true

authors:
  - name: Xin Cai
    url: "https://totalvariation.github.io/"

bibliography: 2024-02-01-open-world-vision-systems.bib

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

Over the last decade, there has been remarkable progress in visual perception algorithms, driven by the development of layered differentiable models optimized in an end-to-end fashion. Despite these advancements, the deployment of such algorithms in open-world scenarios poses significant challenges. To effectively operate in open-world settings, these algorithms must incorporate capabilities for open-set recognition and open vocabulary learning, while also being robust to distribution shifts. Moreover, open-world visual understanding systems are expected to handle continuous streams of real-world data, adapting to non-stationary environments with minimal labelling requirements. This suggests the need for semi- or fully-automatic data engines that facilitate model learning alongside incremental data curation and labelling, potentially assisted with human intervention. The foundational pillars of building open-world visual understanding systems lie in data and annotation efficiency, robustness, and generalization. These challenges can potentially be addressed through a unified framework known as Open-set Unsupervised Domain Adaptation (OUDA).


In general, visual signals can be decomposed into low- and high-frequency components. The former enables the unlocking of open-vocabulary capabilities in Vision-Language Models (VLMs) <d-cite key="radford2021learning"></d-cite> <d-cite key="li2021align"></d-cite> by establishing a well-aligned visual-semantic space. Conversely, the latter plagues the generalizability of deep neural networks on unseen or novel domains, as they may overfit to specific domain styles and therefore capture spurious correlations. OUDA serves as a nexus that connects a broad spectrum of research fields, including generative modelling <d-cite key="hoffman2018cycada"></d-cite> <d-cite key="ilse2020diva"></d-cite> <d-cite key="mahajan2020latent"></d-cite>, semi-supervised learning <d-cite key="berthelot2021adamatch"></d-cite> <d-cite key="zhang2020label"></d-cite>, meta-learning <d-cite key="shu2021open"></d-cite> <d-cite key="kim2022pin"></d-cite> <d-cite key="zhao2021learning"></d-cite>, open-set recognition <d-cite key="saito2021ovanet"></d-cite> <d-cite key="saito2020universal"></d-cite>, open-vocabulary learning <d-cite key="yu2023open"></d-cite> <d-cite key="zara2023autolabel"></d-cite> <d-cite key="wu2023towards"></d-cite>, out-of-distribution detection <d-cite key="shu2023clipood"></d-cite> <d-cite key="wang2023clipn"></d-cite>, few-/zero-shot learning <d-cite key="wu2022style"></d-cite> <d-cite key="fahes2023poda"></d-cite>, contrastive learning <d-cite key="da2022dual"></d-cite> <d-cite key="sahoo2021contrast"></d-cite> <d-cite key="zara2023simplifying"></d-cite>, unsupervised representation learning <d-cite key="hoyer2023mic"></d-cite> <d-cite key="vray2024distill"></d-cite>, active learning <d-cite key="zhang2022bamboo"></d-cite> <d-cite key="wu2022entropy"></d-cite>, continual learning <d-cite key="atanyan2024continuous"></d-cite> <d-cite key="lin2022prototype"></d-cite> <d-cite key="wang2302comprehensive"></d-cite>, and disentanglement of factors of variation <d-cite key="wu2022single"></d-cite> <d-cite key="wei2022unsupervised"></d-cite>. Its primary objective is to transfer knowledge from annotated source domains or pre-trained models (in the case of source-free UDA <d-cite key="liang2020we"></d-cite>) to unlabelled target domains. This transfer process requires addressing both covariate and semantic shifts, which correspond to variations in the high- and low-frequency components of visual signals, respectively. In essence, OUDA leverages methods developed within the aforementioned research areas to combat covariate or semantic shifts. On the other hand, addressing distribution shifts becomes unavoidable when extending any research problem from these areas to a more general and practical context.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/open_world_vision_post_uda_methods.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Fig-1 The taxonomy of UDA methods.
</div>


OUDA integrates open-set recognition/open-vocabulary learning and domain adaptation/generalization within a unified framework, aiming to address both high-level semantic shift and low-level covariate shift simultaneously and therefore presenting compounded challenges that stem from both research domains. Recent advancements have demonstrated rapid progress in constructing sophisticated open-vocabulary detectors or segmentors <d-cite key="zhu2023survey"></d-cite>, facilitated by VLMs trained on web-scale image-text pairs which offer a comprehensive prior of the real-world. Central to this advancement is the endeavour to bridge the granularity gap between coarse-grained visual-semantic associations and fine-grained visual understanding objectives. Predominant solutions can fall into three main categories: (1) incorporating fine-grained awareness into pre-training recipes <d-cite key="li2022grounded"></d-cite> <d-cite key="zhong2022regionclip"></d-cite> <d-cite key="rao2022denseclip"></d-cite>; (2) transferring knowledge from VLMs to downstream fine-grained visual understanding tasks <d-cite key="gu2021open"></d-cite> <d-cite key="kuo2022f"></d-cite> <d-cite key="wu2023cora"></d-cite> <d-cite key="he2023clip"></d-cite>; (3) the amalgamation of Vision Foundation Models (VFMs) by leveraging their complementary expertise resulting from distinct pretraining objectives <d-cite key="han2023boosting"></d-cite> <d-cite key="wang2023sam"></d-cite>. Besides, the pursuit of handling diverse vision tasks with a unified architecture and a single suite of weights in the open-set scenario <d-cite key="zou2023generalized"></d-cite> <d-cite key="zhang2023simple"></d-cite> has garnered increasing attention as a step towards constructing general-purpose vision foundation models.
		

In the emerging task of Open-World Object Detection (OWOD) <d-cite key="joseph2021towards"></d-cite> <d-cite key="gupta2022ow"></d-cite> <d-cite key="wang2023detecting"></d-cite>, which combines open-set recognition with class incremental learning, the inherent open-vocabulary capability of VLMs offers convenience in identifying unknown classes. However, specialized components remain indispensable for the discovery of novel classes. Essentially, equipping a neural network with the ability to say **NO** when facing unfamiliar input, even with models like CLIP <d-cite key="wang2023clipn"></d-cite>, presents significant challenges. Particularly in vision tasks, developing a class-agnostic object localizer capable of generalizing to novel classes remains an open question <d-cite key="kim2022learning"></d-cite>. This challenge proves critical for two-stage open-world detectors or segmentors, as the generation of high-quality proposals for unknown classes is pivotal. Recently, there is a promising trend where class-agnostic object discovery is tackled without requiring any manual annotation by leveraging pre-trained features of self-supervised Vision Transformers (ViTs) <d-cite key="simeoni2023unsupervised"></d-cite>. However, these algorithms still struggle in complex scene-centric scenarios. OUDA introduces a more complicated pipeline compared to UDA with a close-set assumption, requiring the detection or rejection of unknown classes followed by cross-domain alignment with dominant solutions illustrated in Fig-1. It has been demonstrated that overlooking unknown classes during domain alignment can lead to negative transfer or even catastrophic misalignment. As far as I know, existing methods for open-set recognition or novel class discovery largely rely on heuristic approaches, such as one-vs-all classifiers <d-cite key="saito2021ovanet"></d-cite>, entropy-based separation <d-cite key="saito2020universal"></d-cite>, inter-class distance or margin-based methods <d-cite key="miller2021class"></d-cite>, and leveraging zero-shot predictions from VLMs <d-cite key="yu2023open"></d-cite> <d-cite key="zara2023autolabel"></d-cite>. Furthermore, effectively separating (target-)private samples into semantically coherent clusters <d-cite key="zara2023autolabel"></d-cite>, rather than treating them indiscriminately as a generic unknown class, presents an even more formidable challenge, requiring the utilization of intrinsic structures within unseen or novel classes.


Scaling up pre-training data with minimal human intervention has proven to be critical to foundation models, such as GLIP <d-cite key="li2022grounded"></d-cite> and SAM <d-cite key="kirillov2023segment"></d-cite>. Particularly in scenarios where manual annotation is resource-intensive <d-cite key="delatolas2024learning"></d-cite>, there's a pressing need for an automatic data annotation framework. Such a framework should not only generate reliable pseudo labels but also continually expand the concept pool, thereby necessitating resilience to domain shifts stemming from heterogeneous data sources and open-set recognition capability. In the context of video action recognition, this task is referred to as Open-set Video Domain Adaptation (OUVDA) <d-cite key="zara2023simplifying"></d-cite> <d-cite key="zara2023autolabel"></d-cite>, which remains largely unexplored. This emerging research direction is inherently more complex due to the additional temporal dimension and the scarcity of large-scale and diverse datasets, presenting unique challenges that warrant further investigation. The closed learning loop, which involves the simultaneous evolution of model updating and dataset expansion, lays the groundwork for Open World Visual Understanding Systems (OWVUS) capable of continual self-development over time. From a data-centric standpoint, the challenge revolves around constructing a dynamic dataset capable of consistently absorbing novel semantic categories and selecting relevant samples continually <d-cite key="de2021continual"></d-cite> and actively <d-cite key="zhang2022bamboo"></d-cite> with human-machine synergy. Continual learning <d-cite key="wang2302comprehensive"></d-cite>, characterized by rapid adaptation to evolving data distributions and the potential encounter with unseen classes while avoiding catastrophic forgetting, can thus be integrated with OUDA to fulfil this objective.


To conclude, it can be anticipated that there will be a surge of research interest surrounding OUDA and its synergies with other areas such as continual learning and active learning, not only restricted to OWVUS. The prospect of unfolding possibilities and burgeoning potential in the field of OWVUS fills me with anticipation and enthusiasm.


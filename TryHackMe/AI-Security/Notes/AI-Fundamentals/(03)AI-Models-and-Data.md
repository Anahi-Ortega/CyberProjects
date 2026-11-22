# AI Models & Data

## Introduction
Every AI model is, at its core, a product of its training data. Before a single prediction is made or a single prompt is answered, decisions about what data to collect, where to collect it from, and how to process it have already shaped everything the model will ever do.

From PII scraped from the open web to credentials baked into model weights to safety mechanisms quietly eroded during fine-tuning, the risks don't begin when a model is deployed. They begin long before, in a data supply chain that is often invisible, poorly documented, and almost entirely unaudited.

## Training Data
The machine learning lifecycle doesn't answer where data is collected from or whom it is cleaned by.

**Where Does the Data Come From?**

The pipline typically draws from four buckets: 

| Source | What it is | Trust profile |
|--------|------------|---------------|
| Web Scrapping | Automated crawls of public internet content | Low: no curator, no version control, content changes agter collection |
| Licensed Datasets | Data purchased or agreed with platforms | Medium: terms often unclear; original users rarely consented to AI training use |
| Synthetic Data | AI-generated content used to train further AI systems | Variable: growing fast; ~12% of fine-tuning datasets now contain LLM-generated content |
| Internal Corpora | Company knowledge bases, support transcripts, clinical notes used for fine-tuning | Higher: organisation has direct control, but also direct liability if mishandled |

The keyword is "filtered", and how that filtering was done, by whom, and what slipped through is where the security story begins.
> [!NOTE] [Common Crawl](https://commoncrawl.org/) is a free publically available archive of web crawl data that has underpinned nearly every major model family. 

**The Problem of Provenance**  
Data provenance is the ability to answer three questions about any piece of training data:

1. Where did it come from?
2. When was it collected?
3. Has it been modified since?

>[!NOTE] In most of today's AI supply chains the honest answer to all three is 'we don't fully know'. 

Most models are trained on datasets of dataset, huge composites assembled from hundreds of upstream sources, where the original attribution has been lost. [The Data Provenance Initiative](https://www.dataprovenance.org/) audited over 1,800 datasetes and found, over 70% of licenses on on popular hosting platforms were listed as "Unspecified". Furthermore, of those that were lablled, 66% were miscategorized. Organizations fine tuning on these datasets often don't know what they legally have, or what's actually inside it.

The AI equivalent to SBOMs is the Machine Learning - Bill of Materials (ML-BOM): a documented inventory of dataset sources, licenses, PII categories, and filtering decisions. 

>[!NOTE] Adoption is still early, and most organisations deploying third-party models today have nothing close to one.

**PII in the Pipeline**  
One of the most direct consequences of undocumented, large-scale web scraping is that personally identifiable information ends up baked into model weights. Once it's there, it's very difficult to remove.

>[!NOTE] The EU's GDPR explicitly requires data minimisation (collect only what's necessary).

[Truffle Security](https://trufflesecurity.com/blog/research-finds-12-000-live-api-keys-and-passwords-in-deepseek-s-training-data) scanned the December 2-24 Common Crawl archive (400TB of data from 2.67 billion web pages) and found nearly 12,000 live, verified API keys and passwords. This isn't a bug introduced by an attacker. It's a consequence of what went in during training, and no patch fixes it once the model is deployed.

**A Model Engineer**  
A model's behaviour is a direct product of what it was trained on. If that data was scraped without audit, contaminated with PII, or manipulated upstream, those characteristics become part of the model, and there's no reliable way for the organisation deploying it to know. The data supply chain is as real and as exploitable as a software supply chain. For organisations right now, it's almost entirely invisible.

## Building the Model
**Building the Model: Key Concepts**
The goal here is not just to define a foundation for future security topics but to understand why they matter from a security perspective.

**Epochs and Overfitting**  
***Epoch*** - one complete pass of the training algorith through the entire dataset. 

Models are trained over many epochs. The algorithm repeatedly sees the same data, adjusting its parameters each time until it converges on accurate predictions. In reality more epochs don't always create a better model. 

***Overfitting*** - when a model is trained too long and stops learning general patterns and starts memorizing specific training data. 

An overfitted model will perform well on its training data but poorly on other data. This matters for security because overfitting is one mechanism by which a model can "memorise" specific details from its training data, including sensitive ones, making it more likely to reproduce them when prompted.

**Model Validation**  
***Validation Set*** - a portoin of the training model that is withheld and never used for training. In regular intervals the model is tested on this unseen data to check whether its performance is actually generalising or just improving on the training examples it already has. 

If accuracy keeps climbing but validation accuracy plateaus or drops, that's overfitting. 

Validatoin is the quality gate in the ML lifecycle. A model that skips thorough validation = a model whose real-world behaviour is unknown;unknown behaviour is a security risk. It also means any biases or anomalies introduced through compromised training data may go undetected until the model is deployed.

**Post-Training Optimisation: Pruning and Quantisation**
A model will go through compressoin steps before deployment (particularly if it needs to run efficiently on limited hardware). Two of the most common compressions are pruning and quantisation. 

| Technique | What it does | Security consideration |
|-----------|--------------|------------------------|
| Pruning | Removes parameters that contribute little to predictions, shrinking model size | Changes model behaviour post-training; rarely documented in detail |
| Quantisation | Reduces numerical precision of weights (e.g., 32-bit to 8-bit floats) to cut memory and compute requirements | Can degrade safety-aligned behaviour; backdoor defences tested on full-precision models may fail to detect threats in quantised versions |

Both steps are applied after training, often by different third party team packaging the model for distribution. Research has shown that quantisation can silently degrade the safety mechanisms built into a model; defences that worked on the full-precision version may fail to detect backdoors once the model is compressed. When an organisation downloads a quantised model without documentation of what changed during compression, they're inheriting unknown behaviour modifications alongside efficiency gains.

**Federated Learning**
the training approaches mentioned above assume that data flows into a single central location for model training.  

***Federated Learning*** - a model that is trained across many decentralised devices or organizations, with each participant training locally on their own data and only sending weight udpates (not the raw data itself) back to a central server for aggregation. Because of this the integrity of the training process becomes much harder to verify.

A hospital sharing patient records to train a model is a data protection problem; a hospital contributing model updates without ever sending the records is a much easier conversation. In that sense, federated learning genuinely does reduce privacy risk at the data level.

In a federated setup, participants can submit poisoned local updates (subtly manipulated gradients designed to skew the global model's behaviour), and these can be very difficult to detect at the aggregation stage. The question shifts from "who controls the data?" to "who controls the aggregation, and can any participant corrupt it?"

>[!NOTE] Federated learning is therefore an interesting case study in security trade-offs: it solves one trust problem by distributing control, but in doing so creates a different one.

## The Inheritance Problem
**Pre-Trained Models & Fine-Tuning**
Startings with someone else's pretrained model saves millions of dollars, hours of work and computing energy.

Pre-trained models have general info and language understanding like grammar, facts, reasoning patterns, and world knowledge. They've been produced by a small number of well-resourced organizations and made available for others either through their weights or API access. 

Fine-tuning - the process of continuing to train one of these pre-trained models on a smaller, task-specific dataset.

A particular company or team will fine tune their model to improve the understanding necessary to accomplish their particular goal or objective. 

**What fine-tuning changes:** the model's task-specific behavior, tone, and domain knowledge

**What fine-tuning does not change:** base model weights

**The Inheritance Problem**
This is where security risks are. When you use a pre-trained model you inherity everything that modle already contains including the things you cannot see and did not choose: 
- Biases baked in during pre-training persist
- Unexpected behaviors introduced by the base model's training data carry through
- Any safety alignment built into the base model is not as durable as it might appear

These can show up in three concrete ways: 
1. Safety alignment erodes, not breaks
2. Specialization increases attack surface
3. Version matters, and it's rarely tracked



**Inheritance Tax**

when your organization deploys a fine-tuned model, you're also deploying the entire pre-trained base beneth it.  The base beneth was shaped by a training process you didn't control, on data you didn't audit, and by an organization whose supply chaing you almost certainly haven't reveiwed. 

**Fine-tuning doesn't sanatize what came before it.**

## The Black Box Problem
**Models Are a Black Box**
When a security team wants to audit software you can look at the source code or compiled binaries can be disassempled. A trained model's weights are not readable and cannot be reasoned about. There are billions of floating-point numbers that carry no human-readable record of how they were shaped, what data influenced them, or what behaviors they encode. You can't open a model find the decision that makes it behave a certain way. This is a black box. 

Trusting a model is fundamentally, trusting the process that produced it. You can test its behavior but this is sampling, not auditing. The model cannot tell you what it will do on inputs you haven't thought of yet. The attack surface is defined by the model's training, which is opaque. 

**Model Cards**
The documentation artifact designed to address is the model card. 

Model Card - a structured document that accompanies a model and describes what it is, how it was built, and where it falls short. Introduced by Google researchers in 2019, it has become the closest thing the industry has to a standard transparency format.

| Section | What it should tell you |
|---|---|
| Training data | What sources were used, how they were filtered, known gaps or biases |
| Intended user | What the model was designed for (and explictly what it wasn't) |
| Evaluation | Performance metrics across different conditions and demographics | 
| Known limitations | Conditions under which the model is known to underperform or behave unexpectedly |
| Bias assessment | Where training data or evaluatoin may have introuduced skew |
|License | What you're legally permitted to do with the model |

This is essentially the nutrition label for an AI model. You can't see the individual parts of a product, but the label is supposed to tell you what went into it and what to watch out for. 

**The Gaps**
In practice model cards can also be frequently vague and in some cases absent entirely. There is no regularoty requirement to produce one as of now it remains voluntary. The incentirve to be thorough is weak when disclosing limiations might reduce adoption. . The Data Provenance Initiative's audit of over 1,800 datasets found documentation gaps throughout the AI supply chain, and model cards sit at the end of that same underdocumented pipeline.

A sparce or missing model card isn't an inconvenience but more so a warning sign. It means there is either a lack of thoroughly documenting findings or they are actively chooseing not to disclose what they found. Either way the downstream user is blind. 

**In Short**
The model card is your audit trail. In absence of one there is not audit trail, just a black box and hope that whoever built it was thorough. **In security hope is not control.**

## Practical


## Conclusion
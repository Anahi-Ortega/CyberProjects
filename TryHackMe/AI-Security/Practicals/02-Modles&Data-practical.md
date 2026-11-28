# AI Models & Data - Practical
## Instructions
Click the green View Site button above to open a simulated model repository, built to resemble HuggingFace, one of the most widely used platforms for hosting and sharing open-source AI models. Because anyone can upload a model, it's a powerful resource and a real supply chain risk.

Your job is to perform a model audit. Think of it as a scavenger hunt for red flags, the warning signs that a security-conscious engineer should never ignore. They could be lurking in the model card, the metadata, the file listings, or the training details.

After you submit your findings, you'll get feedback on each one, learning the severity and significance of each issue as you go. By the end, you'll have a practical checklist for evaluating any third-party model before it gets near a production system.

Presented in the site:
You are a security analyst. Your team has been asked to evaluate whether trustedai-lab/enterprise-classifier-v2 is safe to integrate into internal tooling. Complete a security review of the model card and flag any concerns before sign-off.

Your task
Read through the model card, files, and sidebar details carefully

Hover over suspicious text to reveal hotspots — click them to flag findings and rate their severity

## Notes
Model Card 
- How to use
- Training Data  
Training data source is vague — no provenance, collection dates, or checksums provided. - *medium*  
No mention of PII filtering despite scraped forum and web data in the corpus. *high*  
- Training Procedure  
Fine-tuned from enterprise-base-v1.1 — a specific version with no documentation of what that version contains or whether it has known issues. *high*
- Intended Uses 
- Evaluation  
No bias evaluation, no limitations section, and a sparse evaluation section with no adversarial or red-team testing documented. *medium*
- License  
Licence listed as "Custom" with no link, no terms, and no contact detail beyond the organisation name. *low*
- Files  
Model file size is significantly smaller than the base model with no documentation of what post-training modifications were applied. *high*

## Results 
**First round:** I identified 2/6 flags at the correct severity — needs improvement

**Second round:**  
Training data source is vague — no provenance, collection dates, or checksums provided.  
Correct severity: **High**  
*Without provenance, there's no way to verify what the model was actually trained on or whether the data was tampered with upstream.*

No mention of PII filtering despite scraped forum and web data in the corpus.  
Correct severity: **High**  
*Large-scale web scraping routinely sweeps up PII. Without documented filtering, you have no way to know what's baked into these weights.*

Fine-tuned from enterprise-base-v1.1 — a specific version with no documentation of what that version contains or whether it has known issues.  
Correct severity: **Medium**  
*Version matters. A specific checkpoint of a base model may carry backdoors or known issues — and every downstream model inherits them.*

Licence listed as "Custom" with no link, no terms, and no contact detail beyond the organisation name.  
Correct severity: **Medium**  
*A licence with no accessible terms creates legal uncertainty. It may be an oversight rather than a deliberate omission, but you cannot assess operational risk without knowing the terms.*

No bias evaluation, no limitations section, and a sparse evaluation section with no adversarial or red-team testing documented.  
Correct severity: **Medium**  
*No red-team or adversarial evaluation documentation is a significant gap — it limits what you can conclude about the model's behaviour, though absence of evidence isn't evidence of a problem.*

Model file size is significantly smaller than the base model with no documentation of what post-training modifications were applied.  
Correct severity: **Medium**  
*Pruning and quantisation change model behaviour post-training and can silently degrade safety mechanisms. Undocumented compression is an unknown modification.*

**Flag:** THM{A_m0del_Stud3nt}
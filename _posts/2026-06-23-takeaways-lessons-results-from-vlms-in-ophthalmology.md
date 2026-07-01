---
title:  "Candid Takeaways and Useful Lessons from Training VLMs for Ophthalmology"
date:   2026-06-22
categories:
  - machine-learning
mathjax: true
author_profile: true
classes: wide
---
I've spent the past 3 months at my job fine-tuning vision language models to be better at analyzing ophthalmic imaging data.  We initially expected quick and easy results by applying standard methods (e.g. GRPO) to domain-specific data.  Unfortunately, the results haven't quite matched (my) very high hopes for this project.  Nonetheless, I think there's lessons for future LLM-related projects, valuable takeaways about VLMs in medicine, and interesting results which defied my expectations that are worth writing about.

### TL;DR
1. Probes and cheap experiments save a lot of time and energy (both in terms of human effort and electricity)
2. Fine-tuning still requires nontrivial amounts of compute and its impact can be limited
3. VLMs and foundation models are useful but not a panacea - they're best used for problems traditional ML fails on, such as when a task requires language, generalization, or has very limited data available for training
4. VLMs are bottlenecked by information loss in the decoder, not by the vision encoder in ophthalmic imaging data
5. Tools may be a promising and easier way to improve VLM performance on domain problems (instead of fine-tuning)
  * The bitter lesson applies.  Larger, better base models (perhaps with tools) beats smaller, lovingly-finetuned models

### Some Exigence
The first problem I worked on at my job was a regression problem on longitudinal patient data.  Compared to standard medical imaging, longitudinal problems are even more data-limited because data can only be collected by a dedicated, multi-year effort with patients that are sufficiently incentivized to continue participating.  While we were eventually able to come up with a clever solution to our particular problem by making some strong assumptions about the data (that seem to be correct based on the results we've achieved), the many months we spent on it convinced me that investing some effort into a better foundation model that could then solve many similar problems could lead to a big payoff.  

The first hope was that VLMs would be able to solve a lot of problems we were interested in out of the box.  There were some promising results on a few very select problems, but performance was actually generally poor - on many evaluations, VLMs in the 4-8 billion parameter range would get performance not much better than random chance.  

This was slightly disappointing but not particularly discouraging.  A lot of my prior research in applying VLMs for content moderation actually started in a similar way, with base performance being dramatically increased through relatively simple prompt engineering or finetuning.

We decided R1-style GRPO training for reasoning using verifiable rewards would be a promising direction to try to improve VLMs, since we noticed that while RL had dramatically improved LLM performance on math and code over the past 2 years, there were relatively few results in the medical imaging space (and what did exist wasn't focused on ophthalmology).  At the same time, we knew that there was a relatively large quantity of ophthalmology imaging data (e.g. OCT B-Scans) with diverse labels spanning many biomarkers, diseases, etc. that were a good fit for generating verifiable questions.  This approach was also appealing because we thought incentivizing reasoning would lead to more interpretable results for clinicians in downstream applications.

### A Series of Mistaken Hypotheses

|Model|Accuracy|
|---|---|
|Gemma-4-E4B|0.409|
|Gemma-4-26B-A4B|0.755|
|Gemma-4-E4B-RL|0.625|





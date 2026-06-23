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

### Some Exigence
The first problem I worked on at my job was a regression problem on longitudinal patient data.  Compared to standard medical imaging, working with longitudinal problems is even more data-limited because it can only be collected by a dedicated, multi-year effort with patients that are sufficiently incentivized to continue participating.  We were eventually able to come up with a clever solution to the problem by making some strong assumptions about the data (that seem to be correct based on the results we've achieved.)  This did take multiple months of working on the problem though, including lots of time spent hand-engineering classical computer vision techniques and experimenting with transfer learning (neither of which panned out.)  
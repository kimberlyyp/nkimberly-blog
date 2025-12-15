---
layout: post
title: "On human intelligence"
date: 2025-11-30 00:56:22 +0000
categories: ["Dangling Thoughts"]
---

Ilya points out that AI models generalize way worse than humans. [Discussing a point made by Yann LeCun](https://www.youtube.com/watch?v=aR20FWCCjAs&t=1513s), he notes that a teenager becomes licensed with roughly 10 hours of supervised driving (plus 40-50 hours of practice in most US states), while autonomous vehicle companies have collected tens of millions of miles of real-world driving data—roughly equivalent to thousands of human lifetimes—and still struggle with edge cases. He speculates that human emotions act as a kind of hardcoded "value function" that lets us learn efficiently from near-misses rather than actual crashes. He has ideas about what's missing but won't share them publicly.

The 10-hour driving example is compelling. But I think it understates how much preparation goes into those 10 hours. We're not blank slates at 16. We arrive at the steering wheel with 16 years of multimodal embodied pretraining: judging distances, tracking moving objects, predicting trajectories, understanding that cars are heavy and fast and dangerous. We also carry evolutionary priors shaped by millions of years of natural selection, which govern everything from our depth perception to our instinctive fear responses.

My intuition is that getting to human-level robustness (let alone superintelligence) requires a few things current approaches underemphasize:

**Unified multimodal embedding across all senses.** Not just vision and language bolted together, but something closer to a single embedding space spanning vision, audition, chemosensation (smell/taste), somatosensation (touch/pain/temperature), proprioception, and vestibular sense. I should note that whether the brain implements anything like a literal unified embedding is an open question in neuroscience; different modalities are processed in distinct cortical regions before integration. But whatever the mechanism, biological intelligence draws on far more sensory channels than current models, and those channels constrain each other in useful ways.

**Developmental curriculum that mimics biological maturation.** We pretrain modality-specific encoders (infancy), then do multimodal integration (childhood), then fine-tune on specific tasks (learning to drive). The 10 hours works because we've already done the hard part. Current ML training doesn't mirror this staged approach.

**Risk-sensitive objectives, not just reward maximization.** Teenagers learn to drive partly through fear of hitting things. Pure reward-seeking doesn't capture this. There's active research on distributional RL and risk-averse objective functions, but current approaches haven't matched biological risk-sensitivity, where hardcoded aversive priors (self-damage = bad) bootstrap learned threat representations.

My speculation is that much of human robustness comes from implicit cross-modal consistency. Physical reality enforces strong correlations across senses. If you train a high-dimensional unified embedding on embodied experience, those correlations get baked into the geometry of the space. When inputs violate physical coherence, they land in low-probability regions of the learned manifold and register as anomalous before you can articulate why.

A simple example: you're walking and the ground feels slightly different underfoot than it looks, maybe unexpectedly soft or slippery. You adjust your gait immediately, before conscious analysis. The mismatch between expected and actual somatosensory feedback triggers a response. The unified experience is anomalous even when vision alone says "clear path." This kind of cross-modal error detection happens constantly and mostly beneath awareness.

I should note that humans aren't infallible here. People walk into glass doors all the time, which is why many building codes and safety standards (including the International Building Code and various state regulations) require visual markers, decals, or contrasting elements on large glass panels at specific heights. Our cross-modal consistency checking fails when one modality (vision) strongly predicts "clear path" and others provide only weak or absent signals. But in domains where multiple senses provide rich, overlapping information, we're remarkably good at detecting when something is off.

This may explain why we're more robust to certain adversarial inputs than current systems. We're not doing explicit consistency checks between separate sensory models. We experience reality as an integrated whole (though how the brain achieves this integration remains one of the hard problems in neuroscience), and violations of physical coherence register as weird before we can articulate why.

For deployment, I'm imagining something like: first, training through developmental stages in sim (accelerated); then, freezing weights for production; now running continuous evals against core competency benchmarks; and finally ensuring periodic recertification where we retrain on edge cases from real-world deployment. This mirrors how we handle pilots or doctors. We don't experiment with untested approaches in safety-critical contexts, but we do periodic structured retraining as edge cases accumulate.

The baseline here is human-level sensory integration. Once that's working, then we can explore superhuman perception (more modalities, higher resolution, faster processing). But matching human robustness is step one, and my guess is we don't get there without embodiment + unified multimodal embedding + developmental curriculum + aversive learning.

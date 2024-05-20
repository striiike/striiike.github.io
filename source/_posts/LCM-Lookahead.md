---
title: LCM-Lookahead for Encoder-based Text-to-Image Personalization
date: 2024-05-19 15:46:10
tags:
---

Using a shortcut mechanism (fast sampling) to guide personalization, they focused on encoder approaches by further exploring the use of an attention-sharing mechanism and consistent data generation.

> Struggling to Maintain a Subject's Identity and Adapt to Novel Styles

- Leverage a pretrained face recognition model -> still driven by L2 noise-prediction.

- GAN can be incorporated with additional identity loss -> diffusion samples from intermediate time steps.

> An Intriguing Property of Generative Models: Fine-tuning Alignment

An intriguing property of generative models is that fine-tuning alignment results in a child's latent space being similar to its parent's latent space.

Use a fast sampling and latent consistency method (LCM).

## Method

### LCM-Lookahead Loss

`Equation (3)`
$$ \hat{z}_{r,0} = \frac{1}{\sqrt{\alpha_t}} \left( z_{r,t} - \sqrt{\beta_t} \cdot \epsilon_{\text{LCM}}(z_{r,t}, y, t, E(I_c)) \right) $$

`Equation (4)`
$$ \mathcal{L}_{\text{LH}} = \mathcal{D} \left( D_{\text{VAE}}(\hat{z}_{r,0}), I_c \right) $$

By adding $y$ and $E(I_c)$, which stand for the prompt and conditioning image respectively, as parameters into the diffusion network.

### Maintaining Alignment

> In order to keep alignment not only with the prior input but also with the baseline model

Randomly re-scale the LoRA component using a factor $\alpha_{LoRA}$ drawn from the range [0.1, 1.0].

Apply annealing to how early the time steps should be focused because early diffusion steps are more effective.

### Extended Self-Attention Features

Augment the identity fidelity by applying a KV-Encoder (derived from the U-Net in the diffusion reversing process).

Encode the conditioning image into the KV-Encoder to get its keys and values.

Cache the keys and values from the KV-Encoder and concatenate them to the keys and values from the diffusion process.

$$ K^l := K_{z_{r,t}}^l \circ K_{z_{c,t}}^l $$
$$ V^l := V_{z_{r,t}}^l \circ V_{z_{c,t}}^l $$

Here, $ \circ $ denotes concatenation.

Due to the excessive appearance transfer by the pretrained U-Net, which is the KV-Encoder, they tuned it with LoRA.

Because of the diversity of the training set, tuning it makes it easier to focus on the identity instead of appearance.

### Consistent Data Generation

To improve prompt alignment and address biases in training data, they generate a novel dataset with synthetic subjects across diverse prompts, ensuring consistency and including stylized images to prevent overemphasis on photo-realism. Among various methods, SDXL-Turbo provided the best balance between generation time, identity consistency, and style variability, producing 500k images for training our encoder.









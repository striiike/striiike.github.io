---
title: Layout to Image
date: 2024-04-18 19:17:35
tags:
mathjax: true
---

## Bounded Attention

## Layout Control with Cross-Attention Guidance

### Preliminaries

$A^{(\gamma)}_{ui}$ stands for the cross-attention map associating each spatial location $u \in \{1, ..., \frac{H}{r} \} \times \{1, ..., \frac{W}{r}\}$, and text token indexed by $i \in \{1, ..., N\}$. 

$u$ can be considered as the pixel compressed in the picture.


### Forward Guidance

$$
A^{(\gamma)}_{ui} \leftarrow (1 - \lambda)A^{(\gamma)}_{ui} + \lambda g^{(\gamma)}_u \sum_v A^{(\gamma)}_{vi}
$$

In this formula of the forward guidance, $A^{\gamma}$ stands for the ${\gamma}$-th layer of the U-Net, $g^{\gamma}_u$ plays a role of smooth window function, and ${\lambda}$ controls the scale of how much the window function works.

Only forward guidance is not enough, because the influence by spatial dependencies in the text tokens and the initial noise can not be eliminated.

### Backward Guidance

According to the author, this part is much more important than the former one.

In order to address the importance of $B$ as the box, an energy function is introduced to bias the attention layer $A^{(\gamma)}_{ui}$ mentioned.

$$
E(A^{(\gamma)}, B, i) = \left(1 - \frac{\sum_{u \in B} A^{(\gamma)}_{ui}}{\sum_u A^{(\gamma)}_{ui}}\right)^2
$$

This loss function is basically demanding the attention layer to decrease the part outside the box, and here is how the latent vector $z_t$ is updated.

$$
z_t \leftarrow z_t - \sigma^2_t \eta \nabla_{z_t} \sum_{\gamma \in \Gamma} E(A^{(\gamma)}, B, i),
$$





## BoxDiff: Box-Constrained Diffusion


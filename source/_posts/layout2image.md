---
title: Layout to Image
date: 2024-04-18 19:17:35
tags:
---

## Bounded Attention

## Layout Control with Cross-Attention Guidance

This article 


### Forward Guidance

$$
A^{(\gamma)}_{ui} \leftarrow (1 - \lambda)A^{(\gamma)}_{ui} + \lambda g^{(\gamma)}_u \sum_v A^{(\gamma)}_{vi}
$$

In this formula of the forward guidance, $A^{\gamma}$ stands for the ${\gamma}$-th layer of the U-Net, $g^{\gamma}_u$ plays a role of smooth window function, and ${\lambda}$ controls the scale of how much the window function works.

Only forward guidance is not enough, because the influence by spatial dependencies in the text tokens and the initial noise can not be eliminated.

### Backward Guidance



## BoxDiff: Box-Constrained Diffusion


---
title: "Nested Wave Function Collapse Enables Large-Scale Content Generation"
date: 2024-03-18
description: "An wave function collapse algorithm framework that supports deterministic, aperiodic, infinite content generation that is suitable for level design."
image: "/path/to/image.png"
type: "post"
showTableOfContents: true
tags: ["research", "algorithm", "pcg", "wfc"]
---
> This research was [first accepted by IEEE Conference on Games 2023](https://ieeexplore.ieee.org/abstract/document/10333214) and was extended to [journal paper for IEEE Transactions on Games 2024](https://ieeexplore.ieee.org/abstract/document/10473141). We won the **Best Paper Award**🏆 for IEEE Conference on Games! Congratulations to all the authors and their contributions to this paper! Thanks to NYU Tandon Game Innovation Lab for supporting the publication of this paper!

## 1. What is Wave Function Collapse (WFC)?

<img src="/images/research/n_wfc.assets/image-20240414125416245.png" alt="image-20240414125416245" style="zoom:80%;" />

**Wave Function Collapse (WFC)** algorithm has been introduced by [Maxim Gumin](https://github.com/mxgmn) for Texture Synthesis. However, it quickly gained a lot of attention in **Procedural Content Generation** (especially scene/level generation). It can be used not only for 2D scene generation, but also for more complex game scene generation in 3D.

If you're wondering how exactly this algorithm works, Gumin's [Github repository](https://github.com/mxgmn/WaveFunctionCollapse/tree/master?tab=readme-ov-file) has a lot of examples explaining it. In addition, you can also check this [Demo](https://oskarstalberg.com/game/wave/wave.html) to see their visualization of WFC.

## 2. What is this paper about?

This paper optimizes the **Time Complexity** of the WFC from exponential level $O(C^n)$ to **polynomial level** $O(n)$.  We propose an optimized algorithm: **Nested Wave Function Collapse (N-WFC)** to address the limitations of WFC's current ability to produce only small-scale content. We provide the **pseudo-code of the algorithm framework** and the **tileset norms (complete and sub-complete tileset)** required to realize the algorithm.

<img src="/images/research/n_wfc.assets/image-20240414115559547.png" alt="image-20240414115559547"  />

We also further prove that our optimized algorithm has good **characteristics** of:

- **Infinity**: theoretically it can generate infinite content
- **Aperiodicity**: under a large-scale generation, the generated content of the algorithm will not be repeated
- **Determinacy**: it will not backtrack due to conflicts

For more information on this paper, please read the [paper](https://ieeexplore.ieee.org/abstract/document/10473141)!

## 3. Awesome Experiments

We experimented the feasibility of our algorithm in two classic games, **Super Mario Bros** and **Carcassonne**. The results of the algorithm are amazing. 😮 

The advantage for WFC is that it **decouples the design and art of the tileset from the generative logic**. Neither need be restricted by the other. In other words, tiles of different artistic designs are equivalent for the algorithm!

### Super Mario Bros

![image-20240414120316058](/images/research/n_wfc.assets/image-20240414120316058.png)

### Carcassonne

![image-20240414124435782](/images/research/n_wfc.assets/image-20240414124435782.png)

## 4. Cite the Paper

Nie, Y., Zheng, S., Zhuang, Z., & Togelius, J. (2024). Nested Wave Function Collapse Enables Large-Scale Content Generation. *IEEE Transactions on Games*.
```bibtex
@article{nie2024nested,
  title={Nested Wave Function Collapse Enables Large-Scale Content Generation},
  author={Nie, Yuhe and Zheng, Shaoming and Zhuang, Zhan and Togelius, Julian},
  journal={IEEE Transactions on Games},
  year={2024},
  publisher={IEEE}
}
```


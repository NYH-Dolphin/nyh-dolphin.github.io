---
title: "Text2Map: Multi-modal Procedural Content Generation based On Synthetic Dataset"
date: 2024-04-21
description: "From unconditional to conditional, build a bridge between traditional PCG algorithm and PCGMML."
image: "/path/to/image.png"
type: "post"
showTableOfContents: true
tags: ["research", "deep learning", "pcg", "brogue"]
---
## 1. Why not traditional PCG?

Traditional PCG algorithms are based on specific heuristic algorithms. Although it can be modified and designed specifically for the requirements of the development team, it still relies on the heuristic experience of a large number of programmers and designers (fine-tune parameters, design fitness functions, human-algorithm interactions,etc).

Multi-modal PCG (MMPCG), leverage the similar ability as Generative AI to allow designer generates the map by simply typing prompt. It also allows developers to directly expose the input of PCG algorithm to players. Players can generate the desired game scene by a single prompt. This will greatly increase the diversity of player interaction with the game.

Our research ideas and framework will further prepare for distil User Generate Content (UGC), as UGC also provide us sufficient amount of map dataset.


## 2. Where is the map dataset?

The challenge for MMPCG is how to get sufficient amount of dataset (both map dataset and the description attached to each map). To resolve this problem, we put forward the method to generate [synthetic dataset](https://en.wikipedia.org/wiki/Synthetic_data).

### Brogue

> Traditional PCG Algorithm for Map Generation

![Brogue Gameplay](https://i.ytimg.com/vi/WzFGbHhXSv0/maxresdefault.jpg)	

## 3. How can I describe the map?


# How Architectural Decisions Shape SVG Structure in Neural Generative Models

**MSc Thesis — Technical University of Applied Sciences Würzburg-Schweinfurt (THWS)**  
**Author:** Fikrat Mutallimov  
**Supervisor:** Prof. Mária Gregorová  
**Program:** Applied Computer Science — Artificial Intelligence  
**Date:** 2026

---

## What is this?

A comparative analysis of 13 neural models for SVG generation, investigating how architectural choices (representation format, supervision signal, model family) influence the structural quality and editability of generated vector graphics.
This is a research thesis, not a software tool. The repo contains the manuscript, reference papers and supporting analysis.

SVGs are everywhere — logos, icons, UI elements, web graphics. The gap between "AI can generate an SVG" and "AI can generate an SVG a designer can actually use" is massive. This thesis maps that gap and identifies what architectural decisions would need to change to close it.
## The Core Question

Most AI-generated SVGs look decent as images but are structurally unusable. Why?

This thesis argues that the structural quality of generated SVGs is primarily shaped by the interaction between two architectural decisions: **how the SVG is represented** (Bézier paths, shape primitives, latent codes) and **what supervision signal drives the learning** (pixel reconstruction, text-guided diffusion, dataset-driven training).

## What's Inside

```
manuscript/     Thesis document (LaTeX source + compiled PDF)
papers/         Reference papers analyzed in the survey
code/           Supporting analysis and evaluation code
```

## Key Contributions

- **Comparative framework** covering 14 models across two families: optimization-based (LIVE, DiffSketcher, SVGDreamer, etc.) and dataset-driven (DeepSVG, Im2Vec, NeuralSVG, etc.)
- **Six-dimension evaluation framework** for assessing SVG structural quality: geometric accuracy, path structure, editability, scalability, semantic alignment, and style diversity
- **Five conditions for clean, editable SVG** (Table 8.2) — no existing model satisfies all five
- **Analysis of the DiffVG bottleneck** — why nearly every optimization-based model inherits flat, uneditable path structure
- **Identification of the representation-supervision interaction** as the primary driver of structural outcomes

## Models Analyzed

Optimization-based: Im2Vec, NiVeL, NeuralSVG, LIVE, SVGDreamer/++, VectorFusion, SAMVG

Dataset-driven: DeepSVG, DeepIcon, SVGFusion, T2V w/ NPR, LayerTracer

## Limitations

- The evaluation framework was never applied quantitatively to actual SVG outputs — it remains conceptual
- Table 7.3 ratings are subjective author assessments, not empirically validated
- No models were run locally; analysis is based on published results and paper descriptions
- Grammar and writing quality issues exist in the manuscript (acknowledged in the conclusion)

## Tech Stack

- LaTeX (thesis writing)
- Python (supporting analysis)
- Extensive literature review of 50+ papers in vector graphics generation


## Contact

- GitHub: [@mutallimof](https://github.com/mutallimof)
- Email: fikretmutallimov@gmail.com
- LinkedIn: [Fikrat Mutallimov](https://www.linkedin.com/in/fikratmutallimov-5b72152b4)

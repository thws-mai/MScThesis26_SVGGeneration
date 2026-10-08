# How Architectural Decisions Shape SVG Structure in Neural Generative Models

**MSc Thesis — Technical University of Applied Sciences Würzburg-Schweinfurt (THWS)**  
**Author:** Fikrat Mutallimov  
**Supervisor:** Prof. Magda Gregorová  
**Program:** Applied Computer Science — Artificial Intelligence  
**Date:** 2026

**Thesis text:** [./manuscript/main.pdf](./manuscript/main.pdf) 

---

## What is this?

A comparative analysis of 13 neural models for SVG generation, investigating how architectural choices (representation format, supervision signal, model family) influence the structural quality and editability of generated vector graphics.
This is a research thesis, not a software tool. The repo contains the manuscript, reference papers and supporting analysis.

SVGs are everywhere — logos, icons, UI elements, web graphics. The gap between "AI can generate an SVG" and "AI can generate an SVG a designer can actually use" is massive. This thesis maps that gap and identifies what architectural decisions would need to change to close it.
## The Core Question

Most AI-generated SVGs look decent as images but are structurally unusable. Why?

This thesis argues that the structural quality of generated SVGs is primarily shaped by the interaction between two architectural decisions: **how the SVG is represented** (Bézier paths, shape primitives, latent codes) and **what supervision signal drives the learning** (pixel reconstruction, text-guided diffusion, dataset-driven training).


## Key Contributions

- **Comparative framework** covering 13 models in three groups: optimization-based (LIVE, SAMVG, VectorFusion, SVGDreamer, etc.), dataset-driven (DeepSVG, DeepIcon, SVGFusion, LayerTracer) and hybrid (Im2Vec, T2V-NPR)
- **Six-dimension evaluation framework** for assessing SVG quality: semantic alignment, path economy, geometric quality, layer organization, editability, and efficiency
- **Five conditions for clean, editable SVG** (Table 7.2) — no existing model satisfies all five
- **Analysis of the DiffVG bottleneck** — why nearly every optimization-based model inherits flat, uneditable path structure
- **Identification of the representation-supervision interaction** as the primary driver of structural outcomes

## Models Analyzed

Optimization-based: LIVE, SAMVG, VectorFusion, SVGDreamer/++, NIVeL, NeuralSVG

Dataset-driven: DeepSVG, DeepIcon, SVGFusion, LayerTracer

Hybrid: Im2Vec (trained without vector supervision), T2V-NPR (trained path prior + per-prompt optimization)

## Limitations

- The evaluation framework was never applied quantitatively to actual SVG outputs — it remains conceptual
- Table 6.3 ratings are subjective author assessments, not empirically validated
- No models were run locally; analysis is based on published results and paper descriptions
- Grammar and writing quality issues exist in the manuscript (acknowledged in the conclusion)

## Tech Stack

- LaTeX (thesis writing)
- Python (supporting analysis)
- Literature review of the papers cited in the thesis (48 references)


## Contact

- GitHub: [@mutallimof](https://github.com/mutallimof)
- Email: fikretmutallimov@gmail.com
- LinkedIn: [Fikrat Mutallimov](https://www.linkedin.com/in/fikrat-mutallimov-5b72152b4/)

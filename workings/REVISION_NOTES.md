# Revision notes

Branch: `revision` (created from `master`). One commit per group. `main.pdf` is rebuilt in the last commit.
Build: `cd manuscript && latexmk -pdf main.tex` compiles with no errors, no undefined references and no `??`.
Page count: 82 → 80. Chapter 2 is gone (about 6 pages). The taxonomy figure and the new explanations add about 3 pages.

Chapter numbers in these notes use the **new** numbering: 1 Introduction, 2 Background, 3 Landscape, 4 Representation,
5 Supervision, 6 Evaluation, 7 Synthesis, 8 Conclusion, Appendix A–C. (Your prompt used the old numbering, where
§3.3 is now §2.3, Table 4.1 is now Table 3.1, and so on.)

## How facts were checked

The container cannot reach arxiv.org, DBLP, CVF, OpenReview or the ACM DL directly (blocked by the network policy).
I checked facts in two ways. First, web searches that quote the papers (arXiv HTML/PDF, CVF open access, NeurIPS,
proceedings.com, IEEE CSDL). Second, the official GitHub repositories, read directly (DiffVG source, LIVE,
NeuralSVG, SVGFusion, SVGDreamer, LayerTracer, Im2Vec and DeepSVG READMEs). I wrote nothing that I could not
confirm this way. Anything unconfirmed is listed below.

## Canonical model facts (single source of truth)

| Model | Family (Fig. 3.1) | Task / input | Training data | At inference | Supervision | Representation / init | DiffVG | Venue |
|---|---|---|---|---|---|---|---|---|
| LIVE | Optimization, reconstruction | Vectorization / image | none | iterative opt. | UDF pixel loss + Xing loss | closed cubic Bézier paths, 4 segments; each new path placed where the reconstruction error is largest; up to 256 paths on complex images | yes | CVPR 2022 |
| SAMVG | Optimization, reconstruction | Vectorization / image | none | iterative opt. | MSE + LPIPS | parametric paths, SAM mask init | yes ("differentiable rendering [DiffVG]") | ICASSP 2024 |
| Im2Vec | Hybrid | Vectorization / image | raster images only (fonts, emoji, icons, MNIST) | single forward pass | raster reconstruction through DiffVG + differentiable compositing; no vector supervision | closed Bézier paths (deformed circle) | yes | CVPR 2021 |
| VectorFusion | Optimization, semantic, parametric | Text-to-SVG | none | iterative opt. | SDS (Stable Diffusion); CLIP only reranks raster samples before LIVE tracing | 64 paths × 4 segments; random init or LIVE trace | yes | CVPR 2023 |
| SVGDreamer | Optimization, semantic, parametric | Text-to-SVG | none | iterative opt. | VPSD (builds on VSD, LoRA score estimator, reward model reweights particles) | SIVE attention-based init | yes | CVPR 2024 |
| SVGDreamer++ | Optimization, semantic, parametric | Text-to-SVG | none | iterative opt. | VPSD + HIVE | structured parametric | not explicitly confirmed | TPAMI 2025 |
| NIVeL | Optimization, semantic, neural implicit | Text-to-SVG | none | iterative opt. | SDS (DeepFloyd, pixel-space diffusion) | implicit fields on a grid, curves via marching squares | **no** (not in the optimization loop) | CVPR 2024 |
| NeuralSVG | Optimization, semantic, neural implicit | Text-to-SVG | none | iterative opt. | SDS + LoRA | MLP: shape index → control points + colour; dropout-based ordering of shapes | yes (README) | ICCV 2025 |
| DeepSVG | Dataset-driven, sequential | Unconditional | SVG-Icons8 | forward | cross-entropy on commands/arguments + KL | canonicalized: start at topmost-leftmost point, clockwise; paths sorted lexicographically by start (ordered variant) | no | NeurIPS 2020 |
| DeepIcon | Dataset-driven, sequential | Vectorization / image | SVG-Icons8 | forward | reconstruction against SVG commands; CLIP image encoder | sequential tokens | no ("bypassing the need for a differentiable rasterizer") | DICTA 2024 |
| SVGFusion | Dataset-driven, latent diffusion | Text-to-SVG | SVGX (~240k SVGs) | sampling with VS-DiT, 24–36 s, no optimization | VP-VAE recon. + latent denoising | latent; outputs include circle, rect, ellipse | n/a | arXiv only (code not released) |
| T2V-NPR | Hybrid | Text-to-SVG | FIGR-8-SVG (path VAE) | per-prompt VSD optimization of path latents + layer-wise vectorization | VSD | neural path latent | — | ACM TOG 43(4) / SIGGRAPH 2024 |
| LayerTracer | Dataset-driven, layered diffusion | Text-to-SVG (also image-conditioned) | ~20k designer-made layered SVGs, turned into construction sequences in a serpentine grid layout | forward diffusion + vectorization step (vtracer) | denoising | layered sequences | no | ICCV 2025 |

Other verified facts used in the text: DiffVG supports Circle, Ellipse, Path, Polygon and Rect (`pydiffvg/shape.py`).
VSD comes from ProlificDreamer (Wang et al., NeurIPS 2023, pp. 8406–8441). The SDS gradient is the weighted difference
between predicted and added noise. Oversaturation is linked to the very high CFG weight (≈100) and the mode-seeking
loss. Bézier Splatting (Liu et al., NeurIPS 2025) is an alternative differentiable rasterizer.

## What changed, per group

**A — Model count.** Every "fourteen/14/vierzehn" is now "thirteen/13/dreizehn" (abstracts, chapters, captions).

**B — SLR removed.** `ch_methodology.tex` is dropped from the build and deleted from the tree (it is still in the git
history). §1.3 now has an honest selection paragraph: not exhaustive, the criteria you gave, code recorded but not
required, and a new label `sec:objective`. "Open-source" is removed from both abstracts, research goal 2, RQ2, the
conclusion intro, the RQ2 answer and the limitations. The personal takeaways now say "most of the models and their code
are open". The Chapter 2 sentence is removed from §1.6. Kitchenham (×2), PRISMA and SALSA are removed. The hard-coded
"Section 4.1" in the appendix is now a `\ref`.

**C — Classification and facts.**
- T2V-NPR is now a hybrid everywhere: §3.2 text, Table 3.3 ("Hybrid", "Sup. + Self-Sup."), §5.4, §5.5, the conclusion
  RQ2 answer and the appendix tables.
- SVGFusion: §5.4 now says it samples with its latent diffusion transformer, with no optimization (24–36 s).
- Im2Vec: trained on raster images only (§3.1, §5.1, §5.4). The "None of these methods require a training dataset"
  claim and the "every optimization-based model refines at inference time" claim now name Im2Vec and T2V-NPR as the
  exceptions. I removed a duplicated paragraph before Table 3.1, which repeated the paragraph after it.
- Table 3.1: SAMVG is now "SVG param. / Pixel recon.". The appendix tables now say LIVE "Pixel recon. (UDF + Xing)"
  and SAMVG "Pixel recon. (MSE + LPIPS)".
- Table 5.1 (supervision signals): LIVE "L1 + perceptual (VGG) + Xing" → "UDF pixel loss + Xing loss", and SAMVG
  "L1 + IoU" → "MSE + LPIPS". Both were verified wrong in the papers. **This goes beyond your list; please confirm.**
- LIVE init is now "error-driven" in Fig. 5.3, its table header, §7.1 and the appendix. SVGDreamer init is now
  "Attention (SIVE)" in Table 3.1 and the appendix, consistent with Table 7.2 and Fig. C.2. **Beyond the list.**
- LIVE path count: "160 paths across five layers" and "up to ~150" could not be found in the paper. Both are now "up
  to 256 paths on complex images", which is verified: LIVE evaluates 32–256 paths on its complex image set. The garbled
  §7.2 sentence is rewritten.
- Table 4.1: the parametric range is 64–512, and the latent row is "Few (n.r.)".
- SVGFusion shape count: neither 10–20 nor 8–15 is reported in the paper, so both numbers are removed. §4.6 now says
  SVGFusion does *not* sit in between. In element count it is closer to DeepSVG, and what differs is its primitive
  vocabulary (circle/rect/ellipse, verified). Fig. 7.1 header: "few shapes, denoising".
- LayerTracer data: a descriptive name is used everywhere ("about 20,000 layered SVGs made by designers"; "20k layered
  SVGs" in tables). Fig. 5.2 explains the serpentine grid layout.
- DeepIcon: SVG-Icons8 and "icon vectorization" (§3.2).
- Fig. 4.1 caption plus one sentence in §4.6: the DeepSVG rocket is a dataset sample, not a DeepSVG output.
- Table 3.3: the label "Optimization" becomes "Pixel recon." and the definition is updated, so Im2Vec no longer
  carries "Self-Sup.".
- NeuralSVG §4.5: the dropout-based regularization replaces "borrows ordered assignment from DeepSVG" (verified).
- CLIP in VectorFusion (§2.4.2, §5.2): CLIP only reranks raster samples. I also removed "early SVGDreamer
  configurations", which I could not verify.
- Condition names in the conclusion now match Table 7.2. "Path Density" becomes "Path Economy" in Table 6.3.

**D — Technical fixes.** The SDS and VSD explanations in §2.4.4 and §5.3 are rewritten, and the ProlificDreamer
reference is added. The hard-coded "[1]" in the introduction is now `\cite{w3techsSVG2024}`. "All of them rely on DiffVG"
becomes "Almost all … NIVeL is the exception". The §3.1 SDS one-liner is corrected too.

**E — Table 6.3 and Chapter 6.**
- Asterisks are kept only where Table 6.1 shows a reported metric. The one exception is the T2V-NPR geometric quality
  rating, because the paper reports a smoothness score (the caption says so). No editability rating has an asterisk.
- Efficiency for LayerTracer and SVGFusion goes from + to ~. This is consistent with the A100 80GB and multi-A800
  hardware in Table 6.2 and with "Enterprise GPU required" in the appendix. As a result, no model is rated + in all six
  dimensions, so the sentence "No model scores well across all six dimensions" stays true.
- The layer-organization sentence now matches the table.
- The code paragraph now says code was recorded but not a selection criterion: four models have no code and NIVeL is
  pending, so five cannot be run from official code. "whom wants" is fixed.
- RQ6 now says LayerTracer (dataset side) and NeuralSVG (optimization side) come closest, both short only on efficiency.
  SVGFusion is similar but unverifiable. **This changes the wording of a finding; please confirm.**

**F — Supervisor points.**
- Taxonomy tree (`forest`, Fig. 3.1), referenced in the Chapter 3 intro.
- Reconstruction vs. semantic split at the start of §2.4.
- Primitive usage paragraph in §4.1, DeepSVG ordering in §4.2, and a worked mouth-curve example in §2.2.3
  (parametric → tokens → NeuralSVG MLP index → control points/colour; NIVeL coordinate → layer).
- "Generative layer" wording in §2.2.4 and §4.4.
- DiffVG paragraph in §2.3 (DeepSVG/DeepIcon/LayerTracer/NIVeL do not use it; Bézier Splatting as an alternative).
- SDS vs. VPSD comparison after Figs. 3.2/3.3.
- Column note in the Table 3.1 caption, plus a note on the hybrids in the Table 6.2 caption.

**G — References.**
- All 47 entries now have venues (conference/journal, year, pages where verified). arXiv-only papers are `@misc` with
  `eprint`: SVGFusion and SVGEditBench V2.
- Fixed: Eisenberg (2002), Kingma & Welling (ICLR 2014), SVGDreamer (CVPR 2024), VectorFusion (CVPR 2023), LIVE
  (CVPR 2022), Im2Vec (CVPR 2021), NIVeL (CVPR 2024), NeuralSVG (ICCV 2025), LayerTracer (ICCV 2025), DeepIcon
  (DICTA 2024), SAMVG (ICASSP 2024), T2V-NPR (TOG 2024), StarVector/LLM4SVG (CVPR 2025), OmniSVG (NeurIPS 2025),
  InternSVG (ICLR 2026), DreamFusion (ICLR 2023), Ha & Eck (ICLR 2018), and the others.
- The LLM review now has its authors (Malashenko, Jarsky, Efimova; Zap. Nauchn. Sem. POMI 546, 59–80, 2025).
- `\nocite{*}` is removed, so the bibliography now lists only cited works. The Claude entry is cited in the
  acknowledgment.

**H — Appendix.**
- Lettered chapters: A (LLM exclusion), B (model tables, sections B.1–B.4), C (pipeline diagrams). References now
  read "Appendix A/C".
- The tables are landscape pages with the heading on the same page. Nothing is cut off and there are no blank pages.
- "Inference Time Overview" is retitled "Task and Inference Overview". The values are aligned with the canonical
  table: LIVE/SAMVG supervision, T2V-NPR Hybrid, LayerTracer Text-to-SVG, dataset names, VectorFusion time, and
  `~` rendered as ∼.
- Page numbers are now continuous (1–72). I also added a short intro sentence to Appendix B.

**I — Typesetting.**
- All headings in Chapters 3–8 are numbered and appear in the TOC. The run-in paragraphs in Chapter 6 became
  subsections, plus one heading "Applying the Framework" for the closing paragraph. **Beyond the list.**
- Floats use `[htbp]`. Fig. 4.1 and Table 5.1 sit next to their first discussion. Figures are reduced to about
  0.72–0.75\textwidth.
- RQ4 is now its own paragraph.
- Added T1 fontenc, lmodern and microtype (no visual change in font). Long URLs in the bibliography can break.

**Grammar pass** (separate commit). Only real errors were fixed, for example "This chapter discuss", "worth to
consider", "primitves", "contrained", "that not limited", "as showed in", "teach them", "That differences", "a
designer" capitalization, and "dependent to".

## Unverified: needs author decision

1. **W3Techs "over 65% in 2024".** I could not retrieve a 2024 figure. Current W3Techs pages show about 63–67%
   (2025–2026). The sentence is unchanged.
2. **LIVE path count.** "160 paths / five layers" and "~150" are not in the sources I could reach. I replaced them with
   "up to 256 paths on complex images" (verified). Please check against the LIVE paper if you want a specific default.
3. **SVGFusion shape count** (10–20 / 8–15) is not reported. The numbers are removed and replaced by a qualitative
   statement.
4. **Not re-verified (not on the list, left unchanged):**
   - DeepSVG "15–30 paths"
   - NeuralSVG "exactly 16 shapes" and the CLIP scores 26.94/26.33/26.58
   - SVGDreamer "256–512 paths"
   - T2V-NPR smoothness 0.80/0.53
   - LIVE 2,600–10,000 s, SAMVG upper bound 2,000 s (139 s is verified)
   - VectorFusion 10–20 min
   - SVGDreamer ≥31 GB and 7–14 min
   - NIVeL ∼5 min/A100
5. **T2V-NPR ∼13 min and RTX 3090** (Table 6.2): not found.
6. **T2V-NPR trainables "NPR-VAE + LoRA".** A LoRA is part of VSD, but the paper's use of it is not confirmed.
7. **Im2Vec "L2 via differentiable rendering"** (Table 5.1): the exact loss form is not confirmed.
8. **DeepIcon "Cross-entropy + CLIP embedding"** (Table 5.1). One secondary source says DeepIcon uses MSE on the
   arguments.
9. **NIVeL priors** (Appendix Table B.2): resolved in the final fixes. DeepFloyd is verified. CLIP is not confirmed,
   so it was removed.
10. **SVGDreamer++ uses DiffVG**: likely (same codebase), but not explicitly confirmed. The text says "almost all".
11. **LayerTracer dataset name.** I did not find "LayerSVG" or a dataset called "Serpentine". The paper describes about
    20k designer-made layered SVGs arranged in a serpentine grid. I used a descriptive name.
12. **"stylized graphics (T2V-NPR)"** in §7.3: the training set is FIGR-8-SVG (black-and-white icons). You may want to
    reword.
13. **SVGFusion version.** arXiv v3 is retitled "SVGFusion: A VAE-Diffusion Transformer for Vector Graphic Generation"
    and has more authors (Xing, Hu, Xue, Zhang, Li, Wang, Xu, Yu). The thesis cites v2, marked "(v2)". Decide whether
    to switch.
14. **arXiv:2604.08809.** Resolved in the self-review round: cited in §6.1 with one sentence (see below).
15. **Partly verified page numbers.** DeepIcon pp. 70–77 comes from a single listing. Pages are omitted for OmniSVG,
    NeRF and LayerTracer.

## Decisions I did not make / for you to check

- **Table 6.3 re-rating and the new RQ6 wording** (see E). Confirm you agree with LayerTracer/SVGFusion efficiency = ~.
- **RQ2 and research goal 2 wording.** "Open-source" is removed, and goal 2 now says "a representative set of
  existing methods".
- **`readme.md`** still says 14 models and lists DiffSketcher and a different family split. I did not touch it.
- **Not addressed from the review (not on your list):**
  - the supervisor's question about what the 16 NeuralSVG rocket shapes are
  - moving the appendix pipeline diagrams into the main text
- **Remaining overfull boxes.** They are small or pre-existing: the TOC page break, the declaration URL, and three
  of 1–8 pt in body paragraphs. None hides text.

## Self-review round (after your confirmation)

Must-fix items applied:
1. Efficiency definition (§6.4.6) now says training hardware counts for dataset-trained models (option a).
2. Table 6.2: SVGFusion and LayerTracer hardware marked "(train)"; the caption explains the label.
3. §6.4.7: the efficiency and path-economy sentences now match Table 6.3. "Poorly on efficiency" for the
   optimization methods became "mostly only partial", and geometric quality became "mixed".
4. LIVE/SAMVG "the loss function is the same" → "the same kind, pixel reconstruction" (§3.1 and Fig. C.1 caption).
5. §7.3 domain list: T2V-NPR removed (it is a hybrid trained on FIGR-8-SVG icons).
6. Appendix B.4: "LoRA pretraining" → "a LoRA is trained during each run" (SVGDreamer, SVGDreamer++).
7. Fig. 5.3 is now cited in §5.5. Table 4.1 is now cited in §4.6.
8. Style rewrites:
   - SDS vs. VPSD comparison: two shorter paragraphs, closing summary line dropped
   - SDS/VSD paragraph in §2.4.4: split in two and plainer
   - Im2Vec trainables paragraph after Table 3.1: tightened

Optional items applied:
- "A concrete example…" opener removed; "NIVeL works the other way around".
- §4.2: the question opener became a plain sentence, and the DeepSVG preprocessing sentence was split. The question at
  §4.1 is kept.
- LayerTracer sentence in §5.4 split.
- RQ6 now reads "my assessment, they were not measured".
- §7.2: LIVE is no longer presented as a diffusion-supervised method. It "shows the same problem without any
  diffusion model".
- "Self-Sup." unified in Appendix B.1.
- readme.md:
  - 13 models with the taxonomy split
  - correct six dimensions
  - table numbers updated (7.2, 6.3)
  - "50+ papers" replaced by the actual reference count (48)
  - DiffSketcher removed

New content:
- One sentence in §6.1 after the SVGEditBench/SVGenius discussion, citing Zhu, Deganutti, Hirsch and Mehta,
  "Structural Evaluation Metrics for SVG Generation via Leave-One-Out Analysis" (arXiv:2604.08809, Apr 2026).
- The bib entry is restored as `@misc` with the eprint. Title and authors were verified via search results that quote
  the arXiv page.
- I placed the sentence after "However, both benchmarks…" so that this sentence still refers to the two benchmarks.

Unchanged: Table 3.3 placement.
The build has no errors and no undefined references. It still has 80 pages, and the remaining overfull boxes are the
same pre-existing ones listed above.

## Final fixes

1. **NIVeL diffusion model.** The paper uses the pretrained DeepFloyd model for its SDS gradients. The authors chose
   it because it denoises in pixel space, so no image encoder has to be backpropagated through. This was verified via
   search results quoting the arXiv paper.
   - Table 5.1 changed from "SDS from Stable Diffusion" to "SDS from DeepFloyd".
   - §5.3 now names the backbone of each model: Stable Diffusion for VectorFusion, DeepFloyd for NIVeL.
   - Appendix Table B.2 priors changed from "DeepFloyd + CLIP" to "DeepFloyd", because CLIP is not confirmed.
   - The appendix Fig. C.3 caption already said DeepFloyd.
2. **Spelling.** "NIVeL" (as in the paper) is now used everywhere: thesis, readme and these notes.
3. **Taxonomy caption.** It now reads "Taxonomy of the thirteen analyzed models by generation strategy."

## Narrative review round

All A items (A1–A34) were applied. From A35 only the grammar fixes were applied: abstract "the same loss",
"although … but" → "while …, but", and the intro sentence "which we will see more over time".

Newly verified facts (via search results quoting the papers):
- DeepSVG decodes all commands non-autoregressively, in a single forward pass.
- SVGFusion's modules are the Vector-Pixel Fusion VAE (VP-VAE), which encodes each SVG together with its rendering,
  and the Vector Space Diffusion Transformer (VS-DiT).
- SVGDreamer assigns a set (non-adaptive) number of primitives to each object through SIVE. SVGDreamer++ adjusts
  the number of primitives during optimization. Both were added to "Primitive count control" in Table 7.2 (B8).

Decisions applied:
- B1: "same loss, different representation" softened in the Ch. 5 intro and §7.1. The representation is the biggest of
  several differences (LoRA, saliency init, dropout ordering, preset shape count).
- B2: "optimization adds paths" → "these methods are run with large path budgets"; only LIVE and SVGDreamer++ add paths.
- B3: error accumulation is now attributed to models that predict one command after another (SketchRNN). DeepSVG's
  icon-level output is explained by its data (§4.2, §4.6).
- B4: RQ6 → "reported and assessed quality" (§1.5, §8.1).
- B5: "dataset-driven" everywhere, including the taxonomy node, the appendix and the readme.
- B6: the "Self-Sup." label is now "Semantic" (Table 3.3, its definition, Appendix B.1).
- B7: novelty claims in §8.2 softened.
- B9: the evaluation chapter now points to the motivation in §1.1 instead of an "industry requirement".
- B10: §2.2.4 and §4.4 are retitled "Latent Space as a Generative Layer". The Table 4.1 and Fig. 4.1 captions say so.
- B11–B13 (Table 6.3):
  - LIVE and SAMVG semantic alignment is now "n/a".
  - Im2Vec and DeepSVG efficiency is "+†" with the note "single forward pass; time not reported".
  - NIVeL semantic alignment is now "+*".
- B14: future work cites SVGDreamer++'s HIVE instead of VPSD.
- B15: the sentence was cut.

Supervisor point: §4.6 now describes the visible parts of the NeuralSVG rocket (dark body with a lighter stripe,
angled fins, pale flame, flat background) without a shape-by-shape mapping. The rocket is in Fig. 4.1 in the current
numbering.

The build has no errors and no undefined references. It has 79 pages, and the remaining overfull boxes are the same
11 pre-existing ones.

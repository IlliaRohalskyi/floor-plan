# LLM-Based Semantic Compilation of Floor Plans into GREB

This repository contains the artifacts and experimental materials for a research project investigating the use of large language models (LLMs) as **semantic compilers** for architectural floor plans. The goal of the project is to translate human-readable spatial descriptions into a formal, graph-based representation using the Generic Role–Entity-Based (GREB) thought language.

Rather than extracting spatial structure directly from raster images, the approach shifts the problem to controlled language specification, where topology, access, and adjacency are described explicitly and then compiled into GREB by an LLM under strict syntactic constraints.

---

## Project Overview

The core research question addressed in this project is:

> Can a large language model reliably translate structured natural-language descriptions of floor plans into a formal GREB representation suitable for symbolic reasoning and navigation?

To answer this, the project evaluates two interaction paradigms:
1. **Image-based prompting**, where the LLM is asked to infer a description from a floor plan image and then generate GREB.
2. **Text-based prompting**, where a manually authored textual description is provided directly and translated into GREB.

The results demonstrate a clear distinction between these paradigms: while image-based inference is unreliable due to hallucination and perceptual ambiguity, text-based semantic compilation is stable and reproducible.

---

## Repository Structure
---

## GREB Representation

GREB (Generic Role–Entity-Based) is a symbolic representation language that models spatial structure using entities (e.g., rooms, walls, doors) and explicit role-based associations (e.g., `room & wall`, `wall & door`, `connected & access`). The representation encodes **topological relationships only** and does not claim geometric or metric accuracy.

The file `greb/example_floor3.greb` is generated exclusively from the textual description in `descriptions/hw_3_og_text.txt` and is considered correct **with respect to that description**.

---

## LLM Configuration

The LLM is configured via a system prompt to act as a **GREB compiler**, not as a conversational agent. The prompt enforces:

- Strict adherence to the GREB EBNF grammar
- Exact identifier usage
- Suppression of explanatory or conversational output
- Generation of GREB code only

The prompt used during experimentation is provided verbatim in `prompts/greb_compiler_system_prompt.txt`, along with documentation explaining prompt provenance and formatting decisions.

---

## Experiments

The experimental methodology and results are documented in the `experiments/` directory.

- `experiment_design.md` describes the evaluation setup, including both image-based and text-based prompting strategies.
- `llm_results.md` summarizes the observed behavior of the LLM under each setup.

The experiments show that:
- Image → Text → GREB pipelines are unreliable due to hallucinated spatial structure.
- Text → GREB translation is stable and reproducible when descriptions are explicit and accurate.

---

## Scope and Limitations

This project focuses on symbolic topology rather than geometric reconstruction. It does not perform pixel-level segmentation, door detection, or metric validation against architectural drawings. The evaluation is limited to a single building case study and a single LLM configuration, and cross-model generalization is left for future work.

---

## Usage

This repository is intended for:
- Academic review and reproducibility
- Methodological reference for controlled LLM-based semantic compilation
- Exploration of GREB as a formal spatial representation

No executable code is required to interpret the contents of this repository.

---




# LLM-Based Semantic Compilation of Floor Plans into GREBNET using GREBTL

This repository contains the artifacts and experimental materials for a research project investigating the use of large language models (LLMs) as **semantic compilers** for architectural floor plans. The goal of the project is to translate human-readable spatial descriptions into a formal, graph-based representation (**GREBNET**) expressed using the **GREB Thought Language (GREBTL)**.

Rather than extracting spatial structure directly from raster images, the approach shifts the problem to controlled language specification, where topology, access, and adjacency are described explicitly and then compiled into a GREBNET representation by an LLM under strict syntactic constraints defined by GREBTL.

---

## Project Overview

The core research question addressed in this project is:

> Can a large language model reliably translate structured natural-language descriptions of floor plans into a formal GREBNET representation, expressed in GREBTL, suitable for symbolic reasoning and navigation?

To answer this, the project evaluates two interaction paradigms:

1. **Image-based prompting**, where the LLM is asked to infer a textual description from a floor plan image and then generate GREBTL statements.
2. **Text-based prompting**, where a manually authored textual description is provided directly and compiled into a GREBNET representation expressed in GREBTL.

The results demonstrate a clear distinction between these paradigms: while image-based inference is unreliable due to hallucination and perceptual ambiguity, text-based semantic compilation is stable and reproducible when spatial knowledge is provided explicitly.

---

## Repository Structure

The repository is organized into the following main components:

- `descriptions/`  
  Human-authored textual descriptions of floor plans, used as input for GREBTL generation.

- `grebtl/`  
  GREBTL grammars, templates, and generated GREBTL programs expressing GREBNET representations.

- `experiments/`  
  Experimental design documentation and qualitative results of LLM-based compilation.

- `prompts/`  
  System prompts used to configure the LLM as a GREBTL compiler, including prompt provenance notes.

---

## GREBNET Representation and GREBTL

**GREB** (Generic Role–Entity-Based) describes the underlying modeling principle.  
**GREBTL** (GREB Thought Language) is the formal language used to express associations.  
**GREBNET** is the resulting network representation of real-world entities.

In this project:

- **GREBNET** represents rooms, walls, doors, and their relationships as a symbolic network.
- **GREBTL** defines how this network is formally expressed using role–entity associations such as  
  `room & wall`, `wall & door`, and `connected & access`.

The representation encodes **topological relationships only** and does not claim geometric or metric accuracy.

The file `grebtl/example_floor3.greb` contains GREBTL code that defines a GREBNET representation generated exclusively from the textual description in `descriptions/hw_3_og_text.txt`. The output is considered correct **with respect to that description**, not with respect to the original architectural drawing.

---

## LLM Configuration

The LLM is configured via a system prompt to act as a **GREBTL compiler**, not as a conversational agent. The prompt enforces:

- Strict adherence to the GREBTL EBNF grammar
- Exact identifier usage
- Suppression of explanatory or conversational output
- Generation of GREBTL code only

The prompt used during experimentation is provided verbatim in  
`prompts/grebtl_compiler_system_prompt.txt`, along with documentation describing prompt provenance, versioning, and formatting decisions.

---

## Experiments

The experimental methodology and results are documented in the `experiments/` directory.

- `experiment_design.md` describes the evaluation setup, including both image-based and text-based prompting strategies.
- `llm_results.md` summarizes the observed behavior of the LLM under each setup.

The experiments show that:

- **Image → Text → GREBTL** pipelines are unreliable due to hallucinated spatial structure.
- **Text → GREBTL** compilation is stable and reproducible when descriptions are explicit, accurate, and complete.
- The resulting GREBNET representations preserve topological intent as specified in the input text.

---

## Scope and Limitations

This project focuses on symbolic topology rather than geometric reconstruction. It does not perform pixel-level segmentation, metric validation, or consistency checking against architectural drawings. Errors or omissions in the textual descriptions are faithfully propagated into the resulting GREBNET representation.

The evaluation is limited to a single building case study and a single LLM configuration. Cross-model comparison and large-scale generalization are outside the scope of this work.

---

## Usage

This repository is intended for:

- Academic review and reproducibility
- Methodological reference for controlled LLM-based semantic compilation
- Exploration of GREBTL and GREBNET as formal spatial modeling tools

No executable code is required to interpret the contents of this repository.




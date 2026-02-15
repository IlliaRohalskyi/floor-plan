# Experiment Design

This document describes the experimental design used to evaluate a large language model (LLM) as a semantic compiler for floor plan representations. The experiments focus on translating floor plan information into a formal GREBNET representation, expressed using the Generic Role–Entity-Based Thought Language (GREBTL), under different input conditions.

## Objective

The primary objective of the experiments is to assess whether an LLM can reliably generate a GREBNET representation of a floor plan when spatial information is provided:

1. Implicitly, through a raster floor plan image.
2. Explicitly, through a structured natural-language description.

The experiments are designed to isolate the role of perception versus language understanding in the generation of topologically correct GREBTL code.

## Scope and Constraints

- The experiments are limited to residential and institutional floor plans from Cairo, used as a controlled case study.
- Only symbolic topology (rooms, walls, doors, access, adjacency) is evaluated.
- No geometric measurements, scaling, or metric validation are performed.
- No external vision models, CAD tools, or geometric solvers are used.

## Model Configuration

All experiments were conducted using OpenAI’s ChatGPT large language model in a text-only interaction setting. The model was configured via a system prompt to act as a **GREBTL compiler**, constrained to output only syntactically valid GREBTL statements following a predefined EBNF grammar.

The grammar specification and output template used by the model are provided in the `greb/` directory. The resulting outputs define a GREBNET that represents the symbolic topology of the floor plan.

## Experimental Setups

### Experiment 1: Image-Based Prompting (Image → Text → GREBTL)

In this setup, the LLM was provided directly with a floor plan image and instructed to first describe the layout and then generate GREBTL code based on that description.

This experiment evaluates the model’s ability to infer spatial structure from visual input and formalize it into a GREBNET without explicit textual guidance.

### Experiment 2: Text-Based Prompting (Text → GREBTL)

In this setup, a detailed, manually authored textual description of the floor plan was provided as input. The description explicitly specifies rooms, walls, door locations, adjacencies, and circulation structure.

The LLM was then instructed to translate this description directly into GREBTL code, producing a GREBNET representation.

This experiment evaluates the model’s performance as a semantic compiler under controlled, ambiguity-free input conditions.

## Evaluation Criteria

The generated GREBNET representations are evaluated qualitatively using the following criteria:

- **Syntactic correctness**: Compliance with the GREBTL EBNF grammar.
- **Identifier consistency**: Stable and consistent naming of rooms, walls, and doors.
- **Topological fidelity**: Preservation of stated adjacencies and access paths.
- **Reproducibility**: Similar outputs across repeated runs with identical inputs.

Geometric accuracy is intentionally excluded, as GREBNET encodes symbolic topology rather than metric geometry.

## Model Scope and Limitations

The experiments in this work were conducted using a single large language model and do not constitute a comparative evaluation across different LLM architectures. While the GREBTL compiler prompt is, in principle, model-agnostic, this study does not assess how differences in instruction-following behavior or output formatting across models might influence the stability or structure of the generated GREBNET representations.

The effectiveness of the approach depends primarily on strict adherence to controlled language and syntactic constraints rather than on model-specific reasoning or visual understanding capabilities. As a result, reproducibility is demonstrated only within the selected prompt–model configuration, and cross-model reproducibility remains an open question for future work.

Importantly, the primary limitation observed in the experiments stems from the absence of grounded visual–spatial understanding when processing floor plan images, rather than from the choice of language model itself. When explicit textual descriptions are provided, the LLM reliably functions as a semantic compiler; when images are used as input, perceptual ambiguity leads to hallucinated or inconsistent spatial interpretations that propagate into the resulting GREBNET.



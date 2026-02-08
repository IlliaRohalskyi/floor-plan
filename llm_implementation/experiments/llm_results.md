# LLM Experiment Results

This document summarizes the observed results from the LLM-based experiments described in `experiment_design.md`, focusing on the quality and reliability of the generated GREB representations.

## Results: Image-Based Prompting

When provided directly with floor plan images, the LLM consistently failed to produce reliable textual descriptions of the layout. Across multiple runs, the generated descriptions exhibited:

- Hallucinated rooms or missing spaces.
- Incorrect or inconsistent room adjacencies.
- Misplaced or omitted doors.
- Unstable interpretation of circulation paths.

Although the resulting GREB code was often syntactically valid, it was semantically incorrect due to errors introduced during visual interpretation. These failures were consistent across repeated runs, indicating a structural limitation rather than prompt sensitivity.

**Conclusion:**  
The LLM cannot be relied upon to accurately infer spatial structure from floor plan images alone.

## Results: Text-Based Prompting

When provided with manually authored, structured textual descriptions, the LLM generated GREB representations that were:

- Fully compliant with the GREB grammar.
- Topologically consistent with the input description.
- Stable across repeated executions.
- Free of hallucinated rooms or connections.

All rooms were represented with exactly four walls, doors were correctly declared on both adjacent sides, and shared boundaries between neighboring spaces were explicitly encoded.

**Conclusion:**  
Under controlled input conditions, the LLM performs reliably as a semantic translator from natural language to GREB.

## Comparative Analysis

The experiments reveal a clear distinction between perception-driven and language-driven tasks:

- Failures in the image-based setup stem from the model’s lack of grounded visual-spatial understanding.
- Success in the text-based setup demonstrates strong capability in symbolic reasoning and structured code generation.

This indicates that the LLM’s primary limitation lies in spatial perception, not in formal language translation.

## Implications

These results support the use of LLMs as semantic compilation layers within larger pipelines, where spatial information is explicitly provided or pre-processed. The approach is not intended as a standalone solution for floor plan interpretation from images, but as a robust bridge between human-authored descriptions and formal graph-based representations.

The GREB files generated in the successful experiments are provided in the `greb/` directory, along with their corresponding textual descriptions in the `descriptions/` directory.

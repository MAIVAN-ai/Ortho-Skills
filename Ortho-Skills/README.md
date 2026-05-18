# Ortho-Skills

Open orthopaedic agent skills for surgeon-supervised diagnosis, AO/OTA fracture classification, treatment planning, implant-selection reasoning, outcome measurement, aftercare, documentation, evidence retrieval, and education.

Repository target: `https://github.com/MAIVAN-ai/Ortho-Skills`

## Workflow

```text
case intake
  -> image quality and diagnostic adequacy
  -> fracture classification
  -> treatment options and AO Surgery Reference lookup
  -> implant-selection factors
  -> outcome measurement
  -> aftercare and follow-up
  -> structured case capsule
  -> education / publication / registry / PMCF handoff
```

## Clinical safety boundary

These skills are not medical devices and do not provide autonomous diagnosis or treatment. They are draft workflow and reasoning protocols for qualified clinicians.

Core rule:

> The AI proposes. The surgeon confirms. The confirmed decision becomes auditable clinical metadata.

## Source standards and references

1. AO/OTA Fracture and Dislocation Classification Compendium — 2018. Used as the classification framework for the AO/OTA Fracture Classifier skill. This repository does not reproduce the compendium or its figures.
2. AO Surgery Reference: <https://surgeryreference.aofoundation.org/>. Used as a web-source lookup target for diagnosis, indications, treatment, and aftercare.

## Validation levels

Initial release: all skills are V0 unless stated otherwise.

- V0: Conceptual / unvalidated
- V1: Expert-reviewed prompt or workflow
- V2: Retrospective case-tested
- V3: Multi-site retrospective validation
- V4: Prospective silent-mode validation
- V5: Clinical workflow validation
- V6: Regulatory-grade deployment context

## Licensing

Original skill text is released under Apache License 2.0. External standards and web sources remain the property of their respective rights holders.

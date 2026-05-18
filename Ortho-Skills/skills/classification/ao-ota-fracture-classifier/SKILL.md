# Skill: AO/OTA Fracture Classifier

## Skill ID

`ao-ota-fracture-classifier`

## Category

`classification`

## Version

`0.1.0`

## Status

Draft — research, education, and workflow prototyping only.

## Validation Level

`V0 — Conceptual / unvalidated`

## Purpose

This skill assists an AI agent in classifying bone fractures in uploaded images or text descriptions according to the **AO/OTA Fracture and Dislocation Classification Compendium — 2018**.

It guides the agent through a surgeon-supervised workflow:

1. identify image type and diagnostic adequacy
2. identify anatomical region and bone
3. determine fracture localisation
4. determine AO/OTA segment
5. determine morphology: type, group, subgroup
6. add qualifications and universal modifiers where appropriate
7. state uncertainty explicitly
8. request missing views or clinical information when needed
9. produce a structured draft AO/OTA classification
10. require confirmation by a qualified orthopaedic surgeon before the classification is locked

The skill does **not** replace clinical judgement. It creates a structured draft classification proposal.

## Runtime Dependency

Image-based use requires a vision-language model.

```yaml
required_for_image_classification:
  - image_input
  - vision_language_model
  - structured_json_output
  - medical_image_reasoning

fallback_for_text_only_classification:
  - text_language_model
  - radiology_report_parsing
  - surgeon_description_mapping
```

If no VLM is configured, the agent must refuse image-based classification and offer text-only classification from a radiology report, surgeon description, or operative note.

## Intended Use

Typical use cases:

- classify an uploaded X-ray according to AO/OTA
- structure a fracture-classification draft for surgeon review
- prepare a case for peer review
- create registry-ready fracture metadata
- support case reports, teaching, outcome tracking, and PMCF workflows

## Not Intended For

This skill must not be used for autonomous diagnosis, autonomous treatment selection, replacing radiology or orthopaedic review, coding fractures when required views or clinical information are missing, medicolegal certification, or regulatory-grade use unless separately validated and governed.

## Source Standard

Primary classification standard:

- AO/OTA Fracture and Dislocation Classification Compendium — 2018

This repository does not reproduce the full compendium. It provides an agent workflow that uses the official standard where available.

## Core Safety Principle

The agent must never present its output as a final clinical diagnosis.

Use this default safety phrase:

> This is a draft AO/OTA classification proposal based on the available image(s) or case information. It requires confirmation by a qualified orthopaedic surgeon. If image quality or required views are insufficient, the final classification should be deferred.

## Human-in-the-Loop Requirement

Every classification output must be labelled as one of:

```text
draft_unconfirmed
needs_additional_imaging
needs_surgeon_confirmation
confirmed_by_surgeon
not_classifiable
```

Default state: `draft_unconfirmed`.

## Required Inputs

```yaml
input:
  images:
    - uploaded_xray_or_dicom_or_photo
  body_region:
    required: false
  laterality:
    required: false
  patient_age_group:
    required: false
    allowed_values: [adult, paediatric, unknown]
  mechanism_of_injury:
    required: false
  clinical_context:
    required: false
  available_views:
    required: false
    examples: [AP, lateral, mortise, oblique, axial, inlet, outlet, Judet, CT, MRI, fluoroscopy, intraoperative_image]
```

## Image-Handling Rules

### Identify modality

```text
plain_radiograph
CT_slice_or_reconstruction
MRI
fluoroscopy
clinical_photo
screenshot
smartphone_photo_of_xray
DICOM
unknown
```

### Assess diagnostic adequacy

Before proposing an AO/OTA code, check:

- correct anatomical region visible
- complete bone or joint segment visible
- AP and lateral views where required
- rotation, exposure, resolution, and cropping
- overlapping structures
- implants
- whether the joint surface is visible
- whether fracture centre can be estimated
- whether CT is needed to assess articular involvement
- whether paediatric growth plates are relevant

If the image is insufficient, do not force a code. Return `needs_additional_imaging`.

## Classification Logic Overview

```text
1. Is this adult or paediatric?
2. What bone is involved?
3. What anatomical segment is involved?
4. Is the fracture diaphyseal or end-segment?
5. If diaphyseal: simple, wedge, or multifragmentary?
6. If end-segment: extraarticular, partial articular, or complete articular?
7. Determine group and subgroup.
8. Add qualifications.
9. Add universal modifiers.
10. State uncertainty and required confirmation.
```

## AO/OTA Code Structure

Build the code in this order:

```text
[Bone][Segment][Type][Group].[Subgroup](qualification)[universal_modifier]
```

Examples:

```text
11A1.1
31A2
43C3
2R2A2(a)
13C2.1(f)[2]
```

Conventions:

- upper-case letters for type: `A`, `B`, `C`
- lower-case letters for qualifications: `(a)`, `(b)`, `(x)`
- square brackets for universal modifiers: `[1]`, `[2]`, `[5a]`
- multiple qualifications or modifiers separated by commas

## Common Adult AO/OTA Location Examples

```yaml
humerus:
  proximal_end_segment: "11"
  diaphyseal_segment: "12"
  distal_end_segment: "13"
radius:
  proximal_end_segment: "2R1"
  diaphyseal_segment: "2R2"
  distal_end_segment: "2R3"
ulna:
  proximal_end_segment: "2U1"
  diaphyseal_segment: "2U2"
  distal_end_segment: "2U3"
femur:
  proximal_end_segment: "31"
  diaphyseal_segment: "32"
  distal_end_segment: "33"
patella:
  location: "34"
tibia:
  proximal_end_segment: "41"
  diaphyseal_segment: "42"
  distal_end_segment: "43"
malleolar_segment:
  location: "44"
fibula:
  proximal_end_segment: "4F1"
  diaphyseal_segment: "4F2"
  distal_end_segment: "4F3"
pelvic_ring:
  location: "61"
acetabulum:
  location: "62"
clavicle:
  location: "15"
scapula:
  location: "14"
```

## Diaphyseal Fracture Workflow

```yaml
type:
  A: {label: simple, definition: single circumferential disruption of the diaphysis}
  B: {label: wedge, definition: wedge fragment present; main fragments have contact after reduction}
  C: {label: multifragmentary, definition: many fracture lines/fragments with no contact between main fragments after reduction if the fractured area were removed}
groups:
  A1: spiral
  A2: oblique
  A3: transverse
  B2: intact_wedge
  B3: fragmentary_wedge
  C2: intact_segmental
  C3: fragmentary_segmental
common_qualifications:
  a: proximal_third
  b: middle_third
  c: distal_third
```

## End-Segment Fracture Workflow

```yaml
type:
  A: {label: extraarticular, definition: fracture spares articular surface}
  B: {label: partial_articular, definition: part of articular surface involved while part remains attached to metaphysis/diaphysis}
  C: {label: complete_articular, definition: articular surface separated from diaphysis}
general_groups:
  A1: avulsion
  A2: simple
  A3: wedge_or_multifragmentary
  B1: simple
  B2: split_and_or_depression
  B3: fragmentary
  C1: simple_articular_simple_metaphyseal
  C2: simple_articular_multifragmentary_metaphyseal
  C3: multifragmentary_articular_multifragmentary_metaphyseal
```

## Universal Modifiers

Use square brackets.

```yaml
1: nondisplaced
2: displaced
3: impaction
3a: articular_impaction
3b: metaphyseal_impaction
4: no_impaction
5: dislocation
5a: anterior_dislocation
5b: posterior_dislocation
5c: medial_dislocation
5d: lateral_dislocation
5e: inferior_dislocation
5f: multidirectional_dislocation
6: subluxation_or_ligamentous_instability
6a: anterior_instability
6b: posterior_instability
6c: medial_instability
6d: lateral_instability
6e: inferior_instability
6f: multidirectional_instability
7: diaphyseal_extension
8: articular_cartilage_injury
8a: ICRS_grade_0
8b: ICRS_grade_1
8c: ICRS_grade_2
8d: ICRS_grade_3
8e: ICRS_grade_4
9: poor_bone_quality
10: replantation
11: amputation_associated_with_fracture
12: associated_with_nonarthroplasty_implant
13: spiral_type_fracture
14: bending_type_fracture
```

Do not add a universal modifier unless supported by the image or clinical information.

## Special Rules

### Paediatric

If the patient is skeletally immature or growth plates are relevant, consider whether paediatric fracture classification is more appropriate. Do not force an adult AO/OTA code.

### Periprosthetic

If a fracture occurs around arthroplasty or implant, consider whether a periprosthetic fracture classification is more appropriate.

### Open fracture

Do not classify open-fracture severity from X-ray alone. Open-fracture classification requires clinical soft-tissue assessment.

### Multiple fractures

If more than one distinct fracture exists in the same bone but different segments, classify each separately. If both radius and ulna are fractured, classify each bone separately.

## Required Output

```yaml
ao_ota_classification:
  classification_status: draft_unconfirmed
  patient_age_group: adult_or_paediatric_or_unknown
  image_modality: plain_radiograph_or_CT_or_other
  image_quality:
    adequate_for_classification: true_or_false
    limitations:
      - string
    required_additional_views:
      - string
  anatomy:
    body_region: string
    laterality: left_or_right_or_unknown
    bone: string
    ao_ota_location: string
    segment: proximal_end_or_diaphyseal_or_distal_end_or_other
  fracture_assessment:
    fracture_present: yes_no_uncertain
    fracture_centre: string
    articular_involvement: yes_no_uncertain
    displacement: yes_no_uncertain
    impaction: yes_no_uncertain
    comminution: none_wedge_multifragmentary_uncertain
    dislocation_or_subluxation: yes_no_uncertain
    implant_present: yes_no_uncertain
  ao_ota_code:
    draft_code: string
    type: string
    group: string
    subgroup: string
    qualifications:
      - string
    universal_modifiers:
      - string
  confidence:
    level: high_moderate_low
    rationale: string
  uncertainty:
    differential_codes:
      - code: string
        reason: string
    unresolved_questions:
      - string
  recommendation:
    classification_action: confirm_or_request_more_imaging_or_defer
    suggested_next_information:
      - string
  audit:
    skill_version: "0.1.0"
    model_used: string
    image_ids:
      - string
    timestamp: string
    surgeon_confirmation_required: true
    confirmed_by: null
```

## Forbidden Behaviour

The agent must not claim diagnostic certainty from inadequate images, invent AO/OTA codes, ignore missing views, ignore paediatric or periprosthetic context, infer open-fracture severity from X-ray alone, or present draft output as surgeon-confirmed.

## Integration Notes for OrthoClass MCP

```yaml
tools:
  classify_ao_ota_fracture:
    description: "Return draft AO/OTA classification from image and case metadata."
  assess_xray_quality:
    description: "Assess whether image quality and views are sufficient for classification."
  suggest_required_views:
    description: "Suggest additional radiographic views or CT/MRI needed."
  lock_surgeon_confirmed_classification:
    description: "Lock surgeon-confirmed AO/OTA code into the case capsule."
  export_registry_ready_classification:
    description: "Export confirmed classification for registry, PMCF, or research workflows."
```

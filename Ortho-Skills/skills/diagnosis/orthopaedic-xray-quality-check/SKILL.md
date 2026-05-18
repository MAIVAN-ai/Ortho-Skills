# Skill: Orthopaedic X-ray Quality Check

## Skill ID

`orthopaedic-xray-quality-check`

## Category

`diagnosis`

## Version

`0.1.0`

## Status

Draft — research, education, and workflow prototyping only.

## Validation Level

`V0 — Conceptual / unvalidated`

## Purpose

Assess whether uploaded radiographs, DICOMs, screenshots, or smartphone photos are adequate for orthopaedic interpretation and classification.

## Intended Use

Use this skill inside a surgeon-supervised orthopaedic workflow where the agent supports structured reasoning, documentation, evidence lookup, education, or follow-up planning.

## Not Intended For

This skill must not be used for autonomous diagnosis, autonomous treatment, autonomous implant prescription, emergency management without clinician oversight, or regulated medical-device use unless separately validated and certified.

## Required Inputs

```yaml
required_inputs:
  case_id: string
  patient_age_group: adult_or_paediatric_or_unknown
  body_region: string_or_unknown
  laterality: left_or_right_or_bilateral_or_unknown
  clinical_context: string_or_unknown
  images_or_documents: optional
  surgeon_question: string
```

## Optional Inputs

```yaml
optional_inputs:
  radiology_report: string
  operative_note: string
  ao_ota_code: string
  comorbidities: string
  implant_data: string
  follow_up_data: string
  patient_reported_outcomes: string
  institutional_protocol: string
```

## Workflow

1. Identify modality and view.
2. Check anatomic coverage and laterality.
3. Assess rotation, exposure, sharpness, cropping, artefacts, and implants.
4. Determine whether the view set is adequate.
5. Suggest additional views or CT/MRI.
6. Return classification readiness.

## Output Requirements

The agent must provide:

- `summary`
- `structured_findings`
- `uncertainty`
- `additional_information_needed`
- `clinician_confirmation_required`
- `audit_trail`

## Human-in-the-Loop Requirement

All outputs are draft support artefacts until reviewed and confirmed by a qualified clinician. Default state: `draft_unconfirmed`.

## Required Output Schema

```yaml
skill_output:
  skill_id: "orthopaedic-xray-quality-check"
  status: draft_unconfirmed
  case_id: string
  summary: string
  findings:
    - string
  uncertainty:
    - string
  required_additional_information:
    - string
  clinician_confirmation_required: true
  audit:
    skill_version: "0.1.0"
    source_references:
      - string
    model_name: string
    model_version: string
    timestamp: string
```

## Safety Constraints

- State uncertainty explicitly.
- Do not invent missing clinical facts.
- Do not claim final clinical authority.
- Request additional imaging or clinical information when needed.
- Escalate red flags to qualified clinical review.
- Keep patient-identifying data out of public logs and examples.

## Image Readiness States

```text
adequate_for_draft_classification
limited_but_usable
needs_additional_views
not_interpretable
wrong_body_region
non_diagnostic_image
```

## Example Prompt

```text
Use the Orthopaedic X-ray Quality Check skill for this orthopaedic case. Provide a structured draft output and state uncertainty.
```

## Example Output

```yaml
skill_output:
  skill_id: "orthopaedic-xray-quality-check"
  status: draft_unconfirmed
  summary: "Draft support output generated for clinician review."
  uncertainty:
    - "Insufficient case detail for final judgement."
  clinician_confirmation_required: true
```

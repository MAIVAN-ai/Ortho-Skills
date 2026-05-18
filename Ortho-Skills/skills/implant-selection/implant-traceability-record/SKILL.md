# Skill: Implant Traceability Record

## Skill ID

`implant-traceability-record`

## Category

`implant-selection`

## Version

`0.1.0`

## Status

Draft — research, education, and workflow prototyping only.

## Validation Level

`V0 — Conceptual / unvalidated`

## Purpose

Capture implant-related metadata for documentation, registry follow-up, UDI/Digital Product Passport integration, and PMCF evidence workflows.

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

1. Capture implant type and function.
2. Capture manufacturer, product name, UDI, lot, serial, size, side, and quantity if provided.
3. Link implant to case, procedure, surgeon confirmation, and outcome timepoints.
4. Flag missing traceability data.
5. Prepare PMCF/registry export fields.

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
  skill_id: "implant-traceability-record"
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

## Traceability Fields

```yaml
implant_traceability:
  manufacturer: string
  product_name: string
  product_family: string
  UDI: string
  lot_number: string
  serial_number: string
  size: string
  side: string
  implantation_date: string
  associated_fracture_classification: string
```

## Example Prompt

```text
Use the Implant Traceability Record skill for this orthopaedic case. Provide a structured draft output and state uncertainty.
```

## Example Output

```yaml
skill_output:
  skill_id: "implant-traceability-record"
  status: draft_unconfirmed
  summary: "Draft support output generated for clinician review."
  uncertainty:
    - "Insufficient case detail for final judgement."
  clinician_confirmation_required: true
```

# Skill: Red-Flag Escalation

## Skill ID

`red-flag-escalation`

## Category

`treatment`

## Version

`0.1.0`

## Status

Draft — research, education, and workflow prototyping only.

## Validation Level

`V0 — Conceptual / unvalidated`

## Purpose

Identify orthopaedic trauma red flags that require urgent senior clinical review or emergency escalation.

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

1. Scan case data for emergency or high-risk features.
2. Classify red flags as immediate, urgent, or routine-review.
3. Avoid detailed emergency instructions beyond escalation.
4. Recommend urgent qualified clinical review.
5. Document trigger and source input.

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
  skill_id: "red-flag-escalation"
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



## Example Prompt

```text
Use the Red-Flag Escalation skill for this orthopaedic case. Provide a structured draft output and state uncertainty.
```

## Example Output

```yaml
skill_output:
  skill_id: "red-flag-escalation"
  status: draft_unconfirmed
  summary: "Draft support output generated for clinician review."
  uncertainty:
    - "Insufficient case detail for final judgement."
  clinician_confirmation_required: true
```

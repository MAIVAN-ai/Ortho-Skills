# Skill: AO Surgery Reference Query

## Skill ID

`ao-surgery-reference-query`

## Category

`evidence`

## Version

`0.1.0`

## Status

Draft — research, education, and workflow prototyping only.

## Validation Level

`V0 — Web-source workflow / unvalidated`

## Purpose

This skill instructs an agent to use the **AO Surgery Reference** as a web-source lookup target for fracture management information, including diagnosis, indications, treatment options, surgical approaches, fixation principles, postoperative care, and aftercare.

The agent must link to the official page rather than copying protected content.

## Official Web Source

```yaml
web_source:
  name: "AO Surgery Reference"
  url: "https://surgeryreference.aofoundation.org/"
  use: "lookup_and_summary_only"
```

## Intended Use

Use this skill after a draft or confirmed anatomical region and fracture pattern are available.

## Not Intended For

This skill must not be used to create final treatment instructions, copy AO Surgery Reference pages, copy diagrams, bypass clinician judgement, or replace local hospital protocols.

## Required Inputs

```yaml
required_inputs:
  case_id: string
  body_region: string
  diagnosis_or_fracture_pattern: string
  ao_ota_code: string_or_unknown
  clinical_question: diagnosis_or_indications_or_treatment_or_aftercare
```

## Workflow

1. Receive confirmed or draft anatomical region, fracture pattern, AO/OTA code, and clinical question.
2. Formulate a precise lookup query targeting `surgeryreference.aofoundation.org`.
3. Retrieve the most relevant AO Surgery Reference page for the region and treatment question.
4. Summarise only the management principles needed for clinician review.
5. Separate diagnosis, indications, treatment, implant/fixation considerations, and aftercare.
6. State when no exact page was found or when the source does not cover the requested scenario.
7. Return official links and avoid copying diagrams or substantial text.

## Query Pattern

```text
site:surgeryreference.aofoundation.org [anatomical region] [AO/OTA code or fracture description] [treatment/approach/aftercare]
```

## Required Output Schema

```yaml
ao_surgery_reference_query:
  status: draft_unconfirmed
  official_source_url: string
  query_used: string
  diagnostic_context: string
  indications_summary:
    - string
  treatment_options_summary:
    - string
  implant_or_fixation_considerations:
    - string
  aftercare_summary:
    - string
  uncertainty:
    - string
  clinician_confirmation_required: true
  copyright_note: "Summarised from official source; do not copy protected AO content."
```

## Safety Constraints

- Summaries are for clinician review only.
- Link to official pages.
- Do not copy AO figures, diagrams, tables, or large text blocks.
- Do not present treatment options as a prescription.
- Local protocols and surgeon judgement override draft output.

## Example Prompt

```text
Use AO Surgery Reference to find treatment and aftercare principles for a confirmed AO/OTA 31A2 proximal femur fracture.
```

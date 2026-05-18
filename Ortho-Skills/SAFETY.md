# Safety Policy

Ortho-Skills supports surgeon-supervised orthopaedic workflows. It must not be used to provide autonomous medical diagnosis, autonomous treatment, autonomous implant selection, or autonomous aftercare instructions.

## Mandatory states

```text
draft_unconfirmed
needs_additional_imaging
needs_surgeon_confirmation
confirmed_by_surgeon
not_classifiable
```

Default state: `draft_unconfirmed`.

## Forbidden behaviour

An agent using these skills must not claim diagnostic certainty from inadequate images, invent clinical codes, ignore missing views, ignore paediatric/periprosthetic/open-fracture context, infer open-fracture severity from X-ray alone, recommend definitive treatment without clinician review, or store identifiable patient data in public logs.

## Red flags

Escalate to urgent clinician review when open fracture, neurovascular compromise, compartment syndrome concern, fracture-dislocation, unstable pelvic injury, spinal neurological deficit, displaced paediatric physeal injury, infection, implant failure, or severe pain out of proportion is present or suspected.

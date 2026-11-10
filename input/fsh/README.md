# FSH sources for smart-anc

This IG was converted from the XML ImplementationGuide (`input/anc-cds.xml`) to SUSHI.
`sushi-config.yaml` now holds what `anc-cds.xml` held (identity, pages, menu,
parameters, dependencies).

The FSH here was produced with GoFSH 2.6.1 from the original resources. A resource
was moved to FSH only when SUSHI 3 builds it with no errors AND the JSON it writes
is identical to the original resource (every element and value, after parsing).
Everything else stays in `input/` as the original JSON or XML, unchanged:

| Kind | In FSH | Kept as original |
| --- | --- | --- |
| ValueSet | 623 | 0 |
| Library | 56 | 8 |
| PlanDefinition | 37 | 3 |
| ActivityDefinition | 17 | 2 |
| Questionnaire | 12 | 0 |
| Measure | 10 | 3 |
| ConceptMap | 6 | 0 |
| Device, OperationDefinition, Patient | 3 | 0 |
| StructureDefinition (profiles, extensions) | 0 | 354 |
| CodeSystem | 0 | 5 |
| Examples (Observation, Procedure, ...) | 0 | 294 |

Why some stay as JSON:

- Profiles and extensions: SUSHI writes a minimal differential. The originals repeat
  elements the parent already constrains (for example `code 1..1 MS` and the empty
  root element), so the FSH output would mean the same but differ as JSON.
- Most examples do not satisfy their own profiles (missing `Observation.category`,
  `Procedure.performed[x]`, a `status` different from the profile's fixed code, ...).
  SUSHI reports each as an error.
- 20 ValueSets share a `name` with another ValueSet. FSH needs unique names, so the
  FSH entity is named `<name>_<id>` and `* ^name` sets the original name back.

To check the round trip again: run `sushi build .` and compare
`fsh-generated/resources/*.json` with the originals in git history (commit before
the conversion).

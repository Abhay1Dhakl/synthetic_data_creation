# Nepali Healthcare Parallel Data Blueprint

## Objective

Build `300-400` English-Nepali parallel examples for each healthcare subdomain.
Use the dataset for machine translation research, not for clinical decision support.

## Recommended Record Unit

One record should contain one medically coherent unit:

- a factual statement
- a patient instruction
- a warning sign
- a short question-answer pair
- a 2-sentence counseling snippet

Recommended length:

- English: `8-40` words
- Nepali: `8-45` words

## Target Mix Per Subdomain

Use the same mix for cancer, heart disease, diabetes, eye, and later domains.

| Content bucket | Target pairs |
| --- | ---: |
| Definitions and anatomy | 30 |
| Symptoms and warning signs | 35 |
| Risk factors and causes | 35 |
| Diagnosis and tests | 30 |
| Treatment overview | 40 |
| Procedure preparation | 35 |
| Aftercare and side effects | 35 |
| Medicines and devices | 25 |
| Lifestyle and prevention | 40 |
| Follow-up and emergency guidance | 35 |
| **Total** | **340** |

## Source Selection Rules

1. Prefer public patient-education material from official health institutions.
2. Use review articles only for terminology support, not as the main source for patient-language pairs.
3. Remove local hospital details that do not transfer well to MT research:
   - phone numbers
   - room numbers
   - addresses
   - donation text
   - survey text
   - institution-specific logistics
4. Keep exact numbers, doses, frequencies, and survival figures only when they are explicitly present in the source.
5. Send records with staging, dosing, contraindications, or survival statistics to manual review.

## Variation Dimensions

Generate diversity without changing the underlying fact.

- Audience: patient, caregiver, trainee
- Register: plain, formal, counseling
- Form: statement, question, answer, instruction, warning
- Complexity: simple sentence, compound sentence, 2-sentence mini paragraph
- Terminology: technical term plus plain-language alternative where useful

## Anti-Repetition Rules

Do not create a large dataset by lightly rephrasing the same sentence again and again.
That weakens the value of the MT corpus.

Use these rules:

1. One fact cluster should usually produce only `3-5` records.
2. Do not repeat the same sentence form for the same fact.
3. Do not keep two records whose meaning is almost identical.
4. Do not allow one content bucket such as definitions or treatment to dominate the topic.
5. Prefer adding a new fact type over adding a new paraphrase.

Bad pattern:

- "Glaucoma is an eye disease."
- "Glaucoma is a disease of the eye."
- "Glaucoma is an eye condition."

Better pattern:

- what glaucoma is
- early symptoms are often absent
- a dilated eye exam helps detect it
- eye drops may help lower pressure
- early treatment can protect vision

## Sentence-Type Mix

For each subdomain, spread records across different communicative purposes.

| Sentence type | Target share |
| --- | ---: |
| Definition / explanation | 15% |
| Symptom description | 15% |
| Risk / cause statement | 10% |
| Test / diagnosis description | 10% |
| Treatment overview | 10% |
| Procedure or preparation instruction | 10% |
| Aftercare instruction | 10% |
| Warning / emergency advice | 10% |
| Lifestyle / prevention advice | 5% |
| Short FAQ | 5% |

## Minimal Schema

```json
{
  "id": "cancer_brachy_001",
  "domain": "cancer",
  "topic": "brachytherapy",
  "content_type": "definition",
  "audience": "patient",
  "register": "plain",
  "source_url": "https://example.org/page",
  "source_fact_en": "Brachytherapy places radiation close to the tumor.",
  "synthetic_en": "Brachytherapy is an internal radiation treatment placed in or near the tumor.",
  "target_ne": "ब्राकिथेरापी भनेको ट्युमरभित्र वा ट्युमरनजिक राखिने आन्तरिक विकिरण उपचार हो।",
  "qa_flags": []
}
```

## Scalable Workflow

1. Collect `20-30` source documents for one subdomain.
2. Split them into atomic facts.
3. Group similar facts into clusters such as symptoms, diagnosis, treatment, aftercare.
4. Create `3-5` diverse records per fact cluster.
5. Translate each variant into natural Nepali.
6. Run terminology and consistency checks.
7. Deduplicate near-identical pairs.

Practical rule:

- `70-100` fact clusters x `3-4` records each = `280-400` pairs

This is better than forcing hundreds of paraphrases from a small number of facts.

## Quality Checks

Before accepting a record, verify:

1. English and Nepali keep the same clinical meaning.
2. Numbers, units, and time expressions are preserved.
3. No unsupported drug name, dose, stage, or survival figure was invented.
4. Institution-specific phone numbers and addresses were removed.
5. The Nepali sentence is natural and not just word-for-word transliteration.
6. The sentence can stand alone without hidden context.
7. The record is not a near-duplicate of an earlier record.
8. The record adds a new fact type or a new sentence function.

## Notes For Your Current Brachytherapy Material

The current `readme` mixes three useful source types:

- technical overview
- patient preparation and aftercare
- hospital-specific administrative details

For MT data creation, keep the first two and drop most of the third.
For example, generic advice like fasting before a procedure is useful, but specific extensions, room numbers, and hospital contact blocks should not be part of the synthetic corpus.

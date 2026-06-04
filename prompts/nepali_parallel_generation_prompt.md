# Nepali Healthcare Parallel Data Generation Prompt

Use this prompt with an LLM after you provide a trusted source passage.

## Prompt

You are generating English-Nepali healthcare parallel data for machine translation research.

Task:
- Read the source passage.
- Extract only supported medical facts from the passage.
- Generate `{target_count}` synthetic parallel records about the same topic.

Output format:
- Return JSONL only.
- One JSON object per line.
- Use this schema:

```json
{
  "id": "domain_topic_001",
  "domain": "cancer",
  "topic": "brachytherapy",
  "content_type": "definition|symptom|risk_factor|diagnosis|treatment|prep|aftercare|warning|lifestyle|faq",
  "audience": "patient|caregiver|trainee",
  "register": "plain|formal|counseling",
  "synthetic_en": "English sentence or short 2-sentence snippet.",
  "target_ne": "Natural Nepali translation.",
  "source_support": "Short note describing which fact from the source this came from."
}
```

Rules:
1. Do not invent facts, doses, timelines, contraindications, statistics, or outcomes.
2. Preserve all medical meaning across English and Nepali.
3. Keep numbers, units, and time expressions aligned.
4. Prefer short, self-contained records.
5. Mix sentence styles:
   - plain statement
   - patient instruction
   - warning sign
   - short FAQ
   - counseling sentence
6. Use natural Nepali. Do not overuse untranslated English terms when a normal Nepali expression exists.
7. Keep technical terms when necessary, but make the Nepali sentence readable.
8. Remove institution-specific details such as phone numbers, room numbers, hospital names, and survey instructions unless the source fact itself is about access to care.
9. If the source uses a very local instruction, generalize it safely. Example:
   - "Go to room C1609" -> do not include
   - "Do not eat solid food after midnight before the procedure" -> keep
10. If a source passage contains unclear or contradictory information, skip that fact.
11. Do not generate shallow paraphrases of the same sentence.
12. Each new record must add at least one new dimension:
   - new fact
   - new sentence type
   - new audience
   - new clinical focus
13. Avoid repeating the same English openings such as:
   - "X is..."
   - "X can cause..."
   - "Treatment may include..."
   Vary sentence structure across the set.
14. Limit each fact cluster to `3-5` records maximum.
15. If two candidate records have nearly the same meaning, keep the clearer one and drop the other.

Distribution target:
- 40% plain patient language
- 40% mid-level educational language
- 20% technical but readable language

Sentence-type target:
- 15% definitions
- 15% symptoms
- 10% risks/causes
- 10% tests/diagnosis
- 10% treatment overview
- 10% prep/procedure steps
- 10% aftercare
- 10% warnings/emergency advice
- 5% prevention/lifestyle
- 5% FAQ

Quality bar:
- The Nepali side must be publication-ready for MT research.
- Avoid duplicate wording.
- Avoid pronouns with missing context like "it", "this", "they" unless the noun is repeated.
- Prefer topic breadth over heavy paraphrasing of a few facts.

## Example Input

Domain: cancer
Topic: brachytherapy
Target count: 20
Source text:
[paste cleaned source passage here]

## Example Output

```json
{"id":"cancer_brachy_001","domain":"cancer","topic":"brachytherapy","content_type":"definition","audience":"patient","register":"plain","synthetic_en":"Brachytherapy is an internal radiation treatment placed in or near the tumor.","target_ne":"ब्राकिथेरापी भनेको ट्युमरभित्र वा ट्युमरनजिक राखिने आन्तरिक विकिरण उपचार हो।","source_support":"Definition of brachytherapy and placement near the tumor."}
{"id":"cancer_brachy_002","domain":"cancer","topic":"brachytherapy","content_type":"prep","audience":"patient","register":"counseling","synthetic_en":"Your care team may use imaging tests to plan the exact position of the applicator before treatment.","target_ne":"उपचार सुरु गर्नु अघि उपचार टोलीले एप्लिकेटरको ठ्याक्कै स्थान तय गर्न इमेजिङ परीक्षण प्रयोग गर्न सक्छ।","source_support":"Planning with imaging before treatment."}
```

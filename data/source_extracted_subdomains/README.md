# Source-Extracted Healthcare Subdomains

This folder contains stricter raw-style healthcare extracts collected from official online sources in the same overall purpose as the root `readme` brachytherapy example.

Rules for files in this folder:

- near-verbatim source text only
- no generated sentence expansion
- one subdomain per file
- every block must include the source title and source URL
- only minimal cleanup is allowed
- remove navigation and layout noise only
- do not add new medical claims

Quantity note:

The root `readme` brachytherapy reference has about 7,270 words. The current subdomain files are much smaller, so they should be treated as seed extracts only, not as final extraction volume for synthetic data generation. See `quantity_report.csv` for the measured gap.

The file `long_form_source_candidates.csv` lists long-form NCBI Bookshelf / StatPearls candidate pages that are closer to the root brachytherapy source style and should be used for the next extraction pass.

Recommended expansion target:

- collect about 6,000 to 8,000 source-extracted words per subdomain
- prefer long official clinical pages or multiple official pages per subdomain
- preserve source title and source URL for each block
- avoid generated or paraphrased expansion

Current files:

- `coronary_heart_disease.md`
- `heart_failure.md`
- `high_blood_pressure.md`
- `diabetes.md`
- `chronic_kidney_disease.md`
- `asthma.md`
- `stroke.md`
- `glaucoma.md`
- `cataracts.md`
- `chemotherapy.md`

The root `readme` remains the brachytherapy reference example.

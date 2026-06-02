# drug-adverse-events-sql-analysis
SQL analysis of a clinical dataset to identify drug safety risks and adverse events

**Background & Objective**
In healthcare data science and pharmacovigilance, monitoring drug safety is paramount—particularly in polypharmacy scenarios where patients concurrently take multiple medications. This project analyzes a clinical dataset of patient records to identify which medications carry the highest risk of adverse events (AE) and to surface potential high-risk drug-drug interactions for public health and regulatory insights.

**Technical Challenges & Data Cleaning**
To ensure data integrity and reliable statistical reporting, several advanced data engineering and cleaning steps were performed in SQL:

**Structural Transformation (Wide-to-Long):** The original dataset was structured "horizontally," with concomitant medications split across four distinct columns (`drug_1` through `drug_4`). To prevent statistical bias and fragmentation, a **`UNION ALL`** operator within a **CTE (Common Table Expression)** was utilized to consolidate all medications into a single, unified column (`drug_name`).

**The Empty String Trap (`NULL` vs. `''`):** During initial aggregation, an anomaly appeared where an "empty" row ranked in the top 10. This occurred because patients taking fewer than four drugs left remaining columns blank, which the system stored as empty strings (`''`) rather than true `NULL` values. The query was optimized to filter out both conditions (`WHERE drug_name IS NOT NULL AND drug_name != ''`), ensuring 100% data integrity.

**Clinical Insights & Findings**
The statistical analysis revealed clear clinical patterns rather than random distribution, strongly reflecting known pharmacological profiles and critical drug-drug interactions:

A. Phenelzine (Top Risk - 26.81%): As a Monoamine Oxidase Inhibitor (MAOI), Phenelzine is a first-generation antidepressant known for its complex safety profile, severe side effects, and extensive dietary and drug interactions. Its position at the top of the risk index aligns perfectly with clinical reality.
B. Aspirin (26.63%) & Warfarin (25.96%): Warfarin is an anticoagulant with a narrow therapeutic window, while Aspirin is an antiplatelet agent. Seeing both highly prevalent in the adverse event risk index of a polypharmacy dataset strongly signals a high clinical risk of severe bleeding due to their combined mechanism.
C Fluoxetine (26.17%):Fluoxetine is a Selective Serotonin Reuptake Inhibitor (SSRI). Concomitant use of an SSRI (Fluoxetine) and an MAOI (Phenelzine) is a major clinical contraindication due to the high risk of inducing **Serotonin Syndrome**, a potentially life-threatening condition.

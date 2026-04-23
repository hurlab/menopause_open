# v4.5.1 Explainer — Slides 1-37

**Deck**: `docs/menopause_sns_gtex_enhanced_2026-04-22_v4.5.1.pptx` (37 slides, 13.33 × 7.50 in widescreen)
**Version lineage**:
- In an earlier build, slides 1-46 contained v4.3.0 baseline content (2026-03-04) rescaled to the 13.33 × 7.50 in canvas; those slides have been removed in this revision, so the deck now opens directly with what were previously slides 47 onward.
- Slides 1-14 (added in v4.4.0, 2026-04-12): Kenichi follow-up targeted analyses, glycolysis gene-node modeling, CIBERSORTx pipeline status.
- Slides 15-22 (added in v4.4.1, 2026-04-12): CIBERSORTx HiRes differential expression results (both depots, 14 cell types).
- Slides 23-37 (added in v4.5.0, 2026-04-21): thorough response to Kenichi's 2026-04-21 email (OVX catecholamine resistance context, Q1–Q4 answers, updated cross-species concordance).

**Purpose of this explainer**: every slide is documented in detail — what it shows, what the data mean, where the numbers come from, and known caveats. Use this as the reference companion when reviewing the deck with Kenichi or Christoph, or when writing the manuscript.

---

## Section 1 — Kenichi Follow-Up & Glycolysis Gene-Node Modeling (Slides 1-14, v4.4.0)

### Slide 1 — Section Header: Update (2026-04-12)

Transition slide marking the start of the v4.4.0 content. The subtitle establishes the three work streams covered in slides 2-14: the targeted follow-up to Kenichi's 2026-03-27 email, glycolysis pathway gene-node modeling, and the then-in-progress CIBERSORTx pipeline. No analytical content on this slide — its role is purely structural. In the previous (v4.5.1 original) build this slide separated the v4.3.0 baseline content from the 2026-04-12 additions; in the current revision the baseline content has been removed, so this slide now opens the deck.

### Slide 2 — Kenichi Follow-Up: Overview

A four-box summary of the targeted follow-up addressing Kenichi's 2026-03-27 email. Each box labels one of his four points and the specific analysis we designed in response:

- **Point 1 (inflammation depot specificity)**: Kenichi noted inflammation appears UP in subcutaneous but NOT in visceral — opposite to his OVX mouse model. We tested this with a formal depot × menopause interaction test (females only, pre vs post, adjusted for SMCENTER).
- **Point 2 (baseline transcriptome limitation)**: We agreed with Kenichi that bulk baseline transcriptomic data cannot prove SNS activity. The response pointed to the then-in-progress CIBERSORTx HiRes analysis for cell-type resolution.
- **Point 3 (aging vs menopause confound)**: Kenichi observed that male and female trends appear parallel in some gene sets (Slide 43 of the older deck). We fit a sex × age_group interaction model to isolate female-specific effects beyond generic aging.
- **Point 4 (ADRB2 correlations)**: Kenichi noted ADRB2 is negatively correlated with inflammation/fibrosis, consistent with catecholamine resistance. We performed a deep correlation analysis with CIBERSORT fractions and produced a human-mouse concordance table.

The bottom panel summarizes five key findings from the follow-up, including the depot-specific inflammation pattern, senescence UP in both depots, the aging-vs-menopause interaction results, and an early version of the concordance table (8/14 concordant). Note: this concordance "8/14" figure is from the v4.4.0-era table and was later superseded in v4.5.0 after the concordance regex bug was fixed on 2026-04-22 (see Slide 35).

**Source files**: `R/next_steps_2026-04-12_kenichi_followup_targeted.R`.

### Slide 3 — Inflammation by Depot and Menopause Status (Point 1)

Left panel: a faceted boxplot showing cytokine inflammation, HALLMARK inflammatory response, and senescence gene-set scores by menopause proxy group (pre, peri, post) within each depot (subcutaneous vs visceral). Right panel: bulleted interpretation.

**Key numbers** (from `kenichi_inflammation_depot_comparison.tsv`):
- Subcutaneous inflammation cytokines: estimate = +0.179, p = 0.138 (UP in post, trend)
- Subcutaneous HALLMARK inflammatory: estimate = +0.124, p = 0.044 (significant UP in post)
- Subcutaneous senescence: estimate = +0.355, p = 0.00077 (strong UP in post)
- Visceral cytokine inflammation: estimate = −0.182, p = 0.131 (DOWN trend in post, opposite direction to subq)
- Visceral HALLMARK inflammatory: estimate = +0.007, p = 0.920 (essentially flat)
- Visceral senescence: estimate = +0.330, p = 0.005 (significant UP)
- Depot × menopause interaction for cytokines: estimate = −0.511, p = 0.003 (formally significant depot-specific divergence)

**Interpretation**: the cross-species discordance is real at the level of inflammation in visceral adipose — humans show a non-significant DOWN trend while OVX mice show UP. Possible explanations include: (1) human GTEx "visceral omentum" is not anatomically analogous to mouse gonadal fat; (2) species-specific immune infiltration patterns; (3) steady-state post-menopausal tissue differs from acute OVX.

**Source files**: `GTEx_v10_AT_analysis_out/tables/next_steps_2026-04-12_kenichi_followup/kenichi_inflammation_depot_comparison.tsv`, `figs/next_steps_2026-04-12_kenichi_followup/kenichi_inflammation_by_depot_menopause.png`.

### Slide 4 — Aging vs Menopause: Sex-Interaction Analysis (Point 3)

Left panel: multi-faceted trajectory plot showing gene-set score means by GTEx age bin, colored by sex, separate panels for subcutaneous vs visceral. Right panel: text summary of the sex × age interaction model results.

The statistical model was `score ~ SMCENTER + sex + age_group + sex:age_group` over two age contrasts: 40-49 vs 60-69 (broad menopause-adjacent window) and 40-49 vs 50-59 (narrower peri-menopausal transition).

**Key findings for 40-49 vs 60-69 subcutaneous** (from `kenichi_aging_vs_menopause_interaction.tsv`):
- Inflammation cytokines: male p = 2.4e-4 (highly sig), female p = 0.13 (NS) → males show stronger aging effect
- HALLMARK inflammatory: male p = 0.019, female p = 0.11 → males stronger
- Senescence: male p = 2.4e-4, female p = 0.046 → both sexes show effect
- Lipolysis core: male p = 4.0e-4 (DOWN), female p = 0.17 → aging effect in both sexes
- TGFβ/ALK7 axis: male p = 0.09, female p = 0.66 (NS)

**Peri-menopausal window (40-49 vs 50-59) visceral** shows different picture with significant female-specific interactions:
- TGFβ/ALK7 axis: interaction p = 0.002 (females UP +0.357, males −0.101)
- Catecholamine resistance: interaction p = 0.003 (females +0.187, males −0.104)
- Fibrosis: interaction p = 0.012 (females +0.238, males −0.051)
- HALLMARK inflammatory: interaction p = 0.020 (females +0.151, males −0.065)

**Bottom line**: broad aging trends (40-49 vs 60-69) are largely parallel between sexes in subcutaneous (no formal female-specific excess). Female-specific effects emerge most clearly in visceral adipose during the narrower peri-menopausal transition. Manuscript framing should lead with the visceral peri-menopausal findings rather than overclaiming the broader age window.

**Source files**: `kenichi_aging_vs_menopause_interaction.tsv`, `kenichi_aging_vs_menopause_interaction_40to60_window.tsv`, `figs/.../kenichi_aging_vs_menopause_trajectories.png`.

### Slide 5 — ADRB2 Correlation Deep Dive (Point 4)

Two-image slide: left panel is the ADRB2 ↔ gene-set-score correlation panel (scatter plots for top 4 gene sets, both depots); right panel is ADRB2 vs CIBERSORT cell fractions. These figures anchor the conclusion that ADRB2 acts as an inverse marker of inflammatory/fibrotic burden in subcutaneous tissue and is positively associated with adipocyte fraction.

**Headline correlations** (from `kenichi_adrb2_correlations_detail.tsv`):
- Subcutaneous, female: ADRB2 × fibrosis r = −0.42, p = 3.3e-11 (strongest negative)
- Subcutaneous, male: ADRB2 × fibrosis r = −0.46, p = 4.5e-26 (strongest male)
- Subcutaneous, both sexes: ADRB2 × cytokine inflammation r ≈ −0.24 (highly sig)
- Visceral, male: ADRB2 × cytokine inflammation r = +0.11, p = 0.023 (POSITIVE — opposite direction to subq)
- Visceral, female: ADRB2 × cytokine inflammation r = +0.07, p = 0.35 (NS, but directionally positive)
- Subcutaneous, female: ADRB2 × adipocyte fraction r = +0.47, p = 3.8e-14 (strong positive)
- Subcutaneous, male: ADRB2 × adipocyte fraction r = +0.32, p = 5.3e-13

The depot-specific reversal of the ADRB2 × inflammation relationship in visceral is the most noteworthy new finding here. It suggests that the catecholamine-resistance interpretation of ADRB2 is depot-dependent and should not be generalized without caveat.

**Source files**: `kenichi_adrb2_correlations_detail.tsv`, `figs/.../kenichi_adrb2_correlation_panel.png`, `figs/.../kenichi_adrb2_vs_cibersort_fractions.png`.

### Slide 6 — Human-Mouse Concordance: GTEx vs OVX Model (early version)

A 14-row concordance table comparing expected OVX mouse direction against human GTEx direction for each phenotype × depot combination. Each row includes the significance of the human result and a concordance label.

**Status note (IMPORTANT)**: This v4.4.0-era concordance table was built before the OVX 042126 email arrived. It uses "8/14 concordant" as the summary, treats visceral inflammation/fibrosis as DISCORDANT (same as v4.5.0), and lists ADRB2 direction as DISCORDANT based on the pre-vs-post comparison (which showed a slight UP trend in humans, opposite to the mouse DOWN expectation).

The **current** authoritative concordance table is on Slide 35 (v4.5.0) and reflects a bug fix applied 2026-04-22 that correctly marks iWAT inflammation/fibrosis/senescence as INDETERMINATE rather than CONCORDANT, because Kenichi's OVX 042126 pptx only shows pWAT measurements for those phenotypes. When presenting this deck, it is best to treat Slide 6 as historical context and direct attention to Slide 35 for the final interpretation.

**Source file**: `kenichi_human_mouse_concordance.tsv` (2026-04-12 version — superseded by `concordance_with_ovx_042126.tsv` for v4.5.0).

### Slide 7 — Section Header: Glycolysis Gene-Node Analysis

Transition slide marking the start of the glycolysis sub-section (slides 8-12). The work here modeled 221 individual glycolysis pathway genes (HALLMARK_GLYCOLYSIS members plus 32 named candidate genes) both at baseline and after CIBERSORT-based composition adjustment, in both depots.

### Slide 8 — Glycolysis Gene-Node Trajectories: Subcutaneous

A wide (12.7 in) faceted trajectory plot showing VST expression of the top glycolysis genes by GTEx age bin, stratified by sex, for subcutaneous adipose. Each facet is a single gene; the x-axis is age bin; the y-axis is VST; male and female trajectories are overlaid.

**Purpose**: to visually document which glycolysis genes show age-dependent trajectories and whether those trajectories differ by sex. Genes such as PFKFB3, HK1, GAPDH, and PGK1 are typical highlights. Many show modest downward trends in post-menopause females, consistent with reduced aerobic-glycolytic capacity.

**Source files**: `R/next_steps_2026-04-12_glycolysis_gene_nodes.R`, `figs/next_steps_2026-04-12_glycolysis_gene_nodes/glycolysis_trajectories_subcutaneous.png`.

### Slide 9 — Glycolysis Gene-Node Trajectories: Visceral

Same content structure as Slide 8 but for visceral omentum. Visceral trajectories are generally flatter or show opposite direction compared to subcutaneous, reinforcing the depot-specific pattern seen in the inflammation analysis on Slide 3.

**Source file**: `figs/.../glycolysis_trajectories_visceral.png`.

### Slide 10 — Glycolysis Gene-Node Effects: Post vs Pre (Forest Plots)

Side-by-side forest plots: left = subcutaneous, right = visceral. Each forest plot shows the post-vs-pre menopause coefficient (and 95% CI) for each glycolysis gene, estimated by the base LM (`~ SMCENTER + menopause_group`). The forest format makes it easy to see which genes move UP vs DOWN in post-menopause and which have CIs that exclude zero.

**Interpretation**: In subcutaneous, a meaningful minority of glycolysis genes show negative post-vs-pre coefficients (DOWN in post) at nominal significance. In visceral, coefficients cluster more tightly around zero.

**Source files**: `glycolysis_gene_node_lm_stats_subq.tsv`, `glycolysis_gene_node_lm_stats_visc.tsv`, two forest PNGs.

### Slide 11 — Glycolysis: Effect Attenuation After Composition Adjustment

Two scatter plots (left subq, right visc): X-axis = base-model post-vs-pre coefficient; Y-axis = CIBERSORT-adjusted coefficient. Points on the diagonal indicate effects robust to composition adjustment (cell-intrinsic); points pulled toward the X-axis (Y ≈ 0) indicate effects driven by cell-composition shifts.

**Interpretation**: Most glycolysis genes show modest attenuation along the Y-axis after CIBERSORT adjustment, meaning both cell-intrinsic and composition-driven contributions exist. A small number of genes collapse strongly toward the X-axis (pure composition effects) and a small number retain almost identical coefficients (pure intrinsic effects).

**Source files**: `glycolysis_effect_attenuation_subq.png`, `glycolysis_effect_attenuation_visc.png`.

### Slide 12 — Glycolysis Genes vs CIBERSORT Fractions: Correlation Heatmaps

Two correlation heatmaps (left subq, right visc) showing the Pearson correlation between each glycolysis gene's VST expression and each CIBERSORT cell-type fraction. This tells us which cell types "carry" the expression of each gene — for example, genes with strong positive correlation to adipocyte fraction are likely adipocyte-expressed; genes that correlate with macrophage fraction are likely immune-cell-expressed.

**Interpretation**: The heatmap reveals that subset of glycolysis genes (e.g., ALDOA, GAPDH) have fairly uniform expression across cell types, while others show clear cell-type bias (e.g., preferential ASPC or macrophage expression).

**Source file**: `glycolysis_fraction_correlations_subq.png` + `_visc.png`.

### Slide 13 — CIBERSORTx HiRes Pipeline: Status and Next Steps (as of v4.4.0)

A three-box layout describing the state of the CIBERSORTx pipeline at v4.4.0 generation time (approximately mid-afternoon 2026-04-12). At that point, Docker images were installed, reference was identified (GSE176171), inputs were prepared (refsample 2.3 GB, mixtures 412 MB subq / 338 MB visc), but Docker execution was still pending due to a token authentication issue.

**Status note**: By v4.4.1 (same day, later), the Docker execution succeeded after token regeneration, HiRes ran for both depots (subq 47 min, visc 41 min), and the DE analysis completed. So this slide is **historical** and the reader should proceed to Slide 15 onward for actual HiRes results.

**Expected outputs** described on this slide:
1. CIBERSORTx Fractions with S-mode batch correction + 100 permutations
2. CIBERSORTx HiRes per-sample per-cell-type expression matrices
3. Downstream DE on the imputed expression (pre/peri/post menopause + male comparison)

**Source files**: `R/next_steps_2026-04-12_prepare_cibersortx_inputs.R`, `scripts/run_cibersortx_pipeline_2026-04-12.sh`, `R/next_steps_2026-04-12_cibersortx_hires_de_analysis.R`.

### Slide 14 — 2026-04-12 Update: New Outputs Summary

Four-box directory of all outputs generated in the 2026-04-12 Update session:
- Box 1: Kenichi follow-up — 1 R script, 7 tables (inflammation depot comparison, 2 aging-vs-menopause tables, 2 ADRB2 correlation tables, sex-difference table, concordance), 4 figures.
- Box 2: Glycolysis gene-nodes — 1 R script, 6 tables, 8 figures.
- Box 3: CIBERSORTx — 3 scripts (prepare, run, DE) plus prepared inputs (3.1 GB). At v4.4.0 time this was still pending.
- Box 4: Email draft — `docs/reply_to_kenichi_2026-04-12.md` (the April 12 response email that was never sent; its content was later consolidated into `reply_to_kenichi_2026-04-21_quick.md` after the April 21 session).

Serves as a quick directory for anyone navigating the 2026-04-12 artifacts.

---

## Section 2 — CIBERSORTx HiRes Cell-Type-Resolved DE (Slides 15-22, v4.4.1)

### Slide 15 — Section Header: CIBERSORTx HiRes

Section opener for the CIBERSORTx HiRes DE results. The subtitle emphasizes three key design choices: both depots analyzed, 14 cell types resolved, and the female menopause proxy is paired with a male comparison cohort to control for non-menopause aging effects.

### Slide 16 — CIBERSORTx Pipeline: What Was Done

A six-box layout describing the end-to-end CIBERSORTx workflow:

- **Reference**: GSE176171 single-cell adipose atlas (human WAT), 14 cell types annotated.
- **Fractions**: Signature matrix built from the sc reference inside CIBERSORTx Docker, with S-mode batch correction and 100 permutations for confidence estimation.
- **HiRes Imputation**: Sample-level cell-type-specific expression inferred from bulk RNA, window=20, 10 threads per job. This is the key step — it converts bulk RNA into approximate per-cell-type expression for each GTEx sample.
- **Differential Expression**: Per-cell-type limma models, testing post vs pre menopause in females, with a male cohort for comparison.
- **Subcutaneous cohort**: 714 samples, 18,359 genes, 14 cell types.
- **Visceral cohort**: 587 samples, 18,359 genes, 14 cell types.

**Key technical clarifications for the reader**:
- `window=20` was raised from the default 10 to exceed the threshold `window > (cell_types + 1)` required for 14 cell types.
- For the HiRes step, we used the pre-adjusted mixture/signature from the Fractions step and disabled S-mode (which is only needed at Fractions step); this resolved an initial HiRes error.

### Slide 17 — CIBERSORTx HiRes DE: Subcutaneous (Post vs Pre, Females)

A 14-row table showing per-cell-type differential expression counts for post-vs-pre menopause in subcutaneous females, sorted by total significant gene count. Columns: cell type, genes tested, UP count, DOWN count, total significant.

**Top findings**:
- **Adipocyte**: 9,697 genes tested; **221 significant** (79 UP, **142 DOWN**). The asymmetric DOWN pattern is the single most striking finding — it suggests cell-intrinsic downregulation of the adipocyte transcriptome in post-menopausal subcutaneous tissue.
- **ASPC (Adipose Stem/Progenitor Cells)**: 152 significant (112 UP, 40 DOWN) — strong UP bias, consistent with progenitor-cell activation post-menopause.
- **Macrophage**: 128 significant (53 UP, 75 DOWN) — balanced.
- **Smooth muscle cell (SMC)**: 138 (64 UP, 74 DOWN).
- **Endothelial**: 124 (83 UP, 41 DOWN).
- **Pericyte**: 0 significant DE genes — likely reflects poor imputation quality for pericyte in subcutaneous depot (noted as an open technical risk).

**Interpretation note in the slide caption**: "Adipocyte shows asymmetric DOWN (142 vs 79 UP), suggesting cell-intrinsic downregulation after menopause. ASPC and T cell show strong UP bias."

**Filter applied**: |logFC| > 0.5 AND FDR < 0.05 (applied in the summary; the raw DE tables contain all genes passing FDR < 0.05 regardless of logFC, including near-zero-logFC imputation artifacts).

**Source files**: `R/next_steps_2026-04-12_cibersortx_hires_de_analysis.R`, DE tables under `tables/next_steps_2026-04-12_cibersortx_hires/` (28 tables per depot).

### Slide 18 — CIBERSORTx HiRes DE: Visceral (Post vs Pre, Females)

Same table structure as Slide 17 but for visceral omentum. Key contrasts with subcutaneous:

- **Endothelial** becomes the #1 DE cell type in visceral (141 significant: 84 UP, 57 DOWN).
- **Macrophage** is consistent across depots (130 in visc vs 128 in subq).
- **Adipocyte** is demoted to #3 in visceral (115 significant: 76 UP, 39 DOWN) — and the direction flips! In subq adipocyte is DOWN-biased (142 vs 79), while in visc it is UP-biased (76 UP, 39 DOWN). This is another depot-specific sign flip.
- **Mast cell** in visceral shows a striking DOWN-only pattern (3 UP, 26 DOWN) — unusual enough to flag as either biologically interesting or an imputation artifact warranting further investigation.

**Interpretation note in the slide caption**: "Endothelial becomes the top DE cell type in visceral. Macrophage consistent across depots. Mast cell shows striking DOWN-only pattern (3 up, 26 down)."

### Slide 19 — Adipocyte: Post vs Pre Menopause (HiRes-Resolved)

Two volcano plots side by side: left = subcutaneous adipocyte, right = visceral adipocyte. Each volcano plots −log10(FDR) vs logFC for every gene in the adipocyte-imputed expression matrix. Genes above the horizontal FDR=0.05 threshold and outside |logFC|>0.5 are colored as significant.

**Visual interpretation**:
- Subcutaneous adipocyte volcano shows the asymmetric DOWN pattern: a thicker tail of significant genes to the left (negative logFC) than to the right (positive logFC).
- Visceral adipocyte volcano is more symmetric, with slight bias toward UP.

The comparison between the two volcanos visually reinforces the depot-specific direction difference noted on Slides 17-18.

**Source file**: `figs/next_steps_2026-04-12_cibersortx_hires/volcano_adipocyte_subcutaneous.png`, `..._visceral.png`.

### Slide 20 — Macrophage: Post vs Pre Menopause (HiRes-Resolved)

Same layout as Slide 19 but for macrophage. Macrophage DE counts are similar between depots (128 subq, 130 visc), and the volcano shapes reflect this consistency. Together with Slide 19 this establishes that adipocyte and macrophage are both substantial contributors to the bulk signal.

### Slide 21 — ASPC & Endothelial: Post vs Pre (Subcutaneous, HiRes)

Two volcano plots for subcutaneous ASPC (left) and endothelial (right). ASPC's UP-biased volcano (112 UP, 40 DOWN) is clearly visible. Endothelial in subcutaneous shows a moderate UP bias (83 UP, 41 DOWN). These are the second- and third-ranked cell types by total DE count in subcutaneous after adipocyte.

### Slide 22 — Cell-Type-Specific Expression of Key Genes (Subcutaneous)

A 2×3 grid of box/violin plots showing per-sample cell-type-resolved expression of six key genes across menopause proxy groups (pre/peri/post) in subcutaneous females. The specific genes typically include ADRB2, ADIPOQ, and a selection of inflammation/fibrosis/senescence markers, each shown in its dominant cell type (e.g., ADRB2 in adipocyte, IL6 in macrophage, COL1A1 in ASPC).

**Purpose**: to visualize concrete per-gene patterns in the HiRes output — which genes show clean menopause-associated changes, which are noisy, and whether the direction of change matches expectations from the bulk analysis.

**Source file**: `figs/.../boxplot_{gene}_subcutaneous.png` × 6.

---

## Section 3 — Response to Kenichi 2026-04-21 (Slides 23-37, v4.5.0)

This section was added in v4.5.0 in response to Kenichi's 2026-04-21 email (`references/042126_Email-from-Kenichi.txt`) and the attached mouse-data PPTX (`references/Catecholamine_resistance_in_OVX_042126.pptx`, 9 slides). Kenichi shared new OVX mouse data showing Adrb3 DOWN in both pWAT and iWAT (Adrb1 also DOWN in iWAT, more pronounced in iWAT), and inflammation/fibrosis/senescence UP in OVX pWAT. He posed four explicit questions, answered across Slides 24-36 below.

### Slide 23 — Section Header: Response to Kenichi (2026-04-21)

Section opener. The subtitle previews the four work items to be addressed: age-stratified ADRB2 analyses, sex-specific decline tests, Saltiel Fig 8-style scatter panels, and cross-species concordance update.

### Slide 24 — Recap: your OVX 042126 findings

Two-box layout. Left box ("What you showed and why it matters for our human GTEx analysis") summarizes Kenichi's key OVX observations:
- Slide 1 of his PPTX: Adrb3 DOWN in both pWAT (visceral) and iWAT (subcutaneous); Adrb1 also DOWN in iWAT.
- The reduction is MORE PRONOUNCED in iWAT.
- Slide 2 of his PPTX: inflammation, fibrosis, senescence markers UP in OVX pWAT (similar trend in iWAT but NOT shown in his slides).
- Hypothesis: estrogen deficiency → chronic SNS activity → catecholamine resistance (reduced β-AR signaling).
- Context: Prog Lipid Res 2009 states ADRB1 AND ADRB2 (not ADRB3) are the primary lipolytic β-AR subtypes in human adipose.

Right box ("Your four questions") enumerates Kenichi's explicit asks:
- Q1: Is ADRB2 – inflammation/fibrosis/senescence negative correlation STRONGER in older women vs younger women?
- Q2: Is ADRB2 reduced in subcutaneous older women, and is the reduction more pronounced in women than men?
- Q3: Reproduce Saltiel Fig 8-style scatter for ADRB2 vs age, CCL2, fibrosis, senescence in women.
- Q4: Suggest best visualization approach.

This slide sets up the answers in Slides 25-33 (Q1-Q3) and Slide 36 (Q4).

### Slide 25 — Q1: ADRB2 correlations in females — 20-49 vs 50-79 (forest plot)

A forest plot showing Pearson r (with 95% CI from Fisher-z) between ADRB2 and each gene-set target (inflammation cytokines, HALLMARK inflammation, fibrosis, senescence, TGFβ/ALK7, CA-resistance, lipolysis core, etc.), stratified by age: 20-49 (younger females) vs 50-79 (older females), for both depots.

**The forest plot answer to Q1**: the point estimates for older females are NOT systematically more negative than for younger females. For some targets (e.g., subcutaneous fibrosis) older is slightly more negative; for others (e.g., subcutaneous CCL2 and HALLMARK inflammation) older is slightly LESS negative. The 95% CIs overlap broadly.

**Key statistic on the slide caption**: "Fisher-z tests for young vs older female correlation magnitudes are all non-significant at α=0.05. Min p = 0.062 for visceral TGFβ/ALK7 axis, where older females show a STRONGER POSITIVE correlation — opposite direction to catecholamine-resistance hypothesis."

This is an intellectually honest negative result: Kenichi's hypothesis (older women show stronger negative ADRB2-inflammation correlation) is NOT supported by the GTEx data.

**Source files**: `R/next_steps_2026-04-21_adrb2_age_stratified_saltiel_style.R`, `figs/next_steps_2026-04-21_adrb2_age_stratified/adrb2_age_stratified_forest_female.png`, `tables/.../adrb2_age_correlation_young_vs_older_fisherz.tsv`.

### Slide 26 — Q1: Fisher-z comparison (older vs younger females) — top gene sets

An 18-row table showing for each (depot, target) pair: n_young, r_young, n_older, r_older, r_delta (older minus younger), Fisher-z p, and whether "older more negative" is TRUE.

**Summary row-by-row**:
- Subcutaneous fibrosis: r_young=-0.379, r_older=-0.433, r_delta=-0.054, p=0.647 → YES older more negative, but NS.
- Subcutaneous senescence: r_young=-0.315, r_older=-0.255, r_delta=+0.060, p=0.644 → NO (older LESS negative).
- Subcutaneous CCL2: r_young=-0.410, r_older=-0.245, r_delta=+0.165, p=0.189 → NO (older LESS negative; largest directional counter-example).
- Subcutaneous TGFB1: r_young=-0.237, r_older=-0.381, r_delta=-0.144, p=0.258 → YES older more negative, NS.
- Visceral TGFβ/ALK7 axis (context from fisherz TSV, off-table): r_young=+0.036, r_older=+0.314, p=0.062 → older shows STRONGER POSITIVE correlation.

**Slide caption summary**: "Net result: no formal evidence that the ADRB2–inflammation/fibrosis negative correlation is STRONGER in older women vs younger women. In subcutaneous, fibrosis shows slight strengthening with age (Delta r = −0.05, NS); CCL2 shows the opposite (Delta r = +0.16, NS)."

### Slide 27 — Q2: Beta-AR expression by age and sex (both depots)

A 3×2 grid of age-trend plots: three rows (ADRB1, ADRB2, ADRB3) × two columns (subcutaneous, visceral). Each cell shows mean VST (±95% CI) per GTEx age bin, colored by sex.

**Visual answer to the first part of Q2 ("is ADRB2 reduced in older women?")**: YES, ADRB2 declines with age in subcutaneous in both sexes. The female trajectory is slightly less steep than the male trajectory but both show decline.

**Slide caption**: "All three receptors decline with age in subcutaneous; ADRB3 declines in both depots."

**Source file**: `figs/next_steps_2026-04-21_adrb2_age_stratified/adrb2_age_trend_by_sex.png`.

### Slide 28 — Q2: Sex × age interaction — is female decline larger than male?

Split-panel: left image is the sex × age interaction coefficient plot (forest-style) across all four receptors × two depots; right side is a compact 16-row table of separate-fit within-sex slopes with p-values.

**Key statistics** (from `adrb2_age_effects_by_sex.tsv` and `adrb2_sex_age_interaction.tsv`):
- ADRB2 subcutaneous: male slope = −0.0103/yr (p = 0.002), female slope = −0.0084/yr (p = 0.064, borderline within-female model). In the pooled sex × age interaction model, interaction coefficient = +0.0004 VST/yr, p = 0.94 → NO significant sex difference.
- ADRB3 visceral: male slope = −0.0109/yr (p = 0.008), female slope = −0.0208/yr (p = 0.003). Interaction coefficient = −0.0098, p = 0.194 → NS but directionally matches Kenichi's OVX pWAT observation (female decline larger).
- ADRB3 subcutaneous: both sexes decline significantly (male p = 7.5e-9, female p = 0.002); interaction p = 0.46 NS.
- ADRB1 subcutaneous: male declines significantly (p = 6.2e-4), female does not (p = 0.228); interaction p = 0.32 NS.

**Slide caption**: "ADRB2 subcutaneous: male slope = −0.0103 (p=0.002), female slope = −0.0084 (p=0.064, borderline within-female); pooled sex*age interaction p = 0.94 — NO significant sex difference in ADRB2 decline. ADRB3 visceral: female −0.0208 vs male −0.0109, interaction p = 0.19 (directionally matches OVX pWAT but NS)."

**Intellectual honesty note**: the answer to Kenichi's Q2 Part B ("is the reduction more pronounced in women than in men?") is NO for ADRB2 (p = 0.94) and only directionally-matched-but-NS for ADRB3 visceral (p = 0.19). We do not overclaim.

### Slide 29 — Q3: Saltiel Fig 8-style panel — FEMALE, subcutaneous (primary)

A 2×2 scatter panel reproducing Saltiel/Valentine JCI 2022 Fig 8 format but with ADRB2 on the X-axis and four menopause-relevant endpoints on the Y-axis: age, CCL2, fibrosis score, senescence score. Points are GTEx female subcutaneous samples (n=233); overlaid is the OLS regression line with 95% CI band and in-panel annotations of R², p-value, and n.

**Key regression stats** (from `saltiel_style_regression_stats.tsv`):
- ADRB2 vs age: slope = −2.11, R² = 0.021, p = 0.029 (significant but weak).
- ADRB2 vs CCL2: slope = −0.599, R² = 0.090, p = 3.0e-6 (strong negative).
- ADRB2 vs fibrosis: slope = −0.235, R² = 0.174, p = 3.3e-11 (strongest panel).
- ADRB2 vs senescence: slope = −0.207, R² = 0.076, p = 2.0e-5 (strong negative).

**Caption**: "ADRB2 vs fibrosis: R² = 0.17, p = 3.3e-11 (strongest panel). ADRB2 vs CCL2: R² = 0.09, p = 3.0e-6. ADRB2 vs senescence: R² = 0.076, p = 2.0e-5. ADRB2 vs age: R² = 0.021, p = 0.029 (weak but significant)."

This is the primary visual answer to Q3 and is recommended as the primary manuscript figure.

**Source**: `saltiel_adrb2_scatters_female_subcutaneous.png`.

### Slide 30 — Q3: Saltiel-style panel — FEMALE, visceral (supplementary)

Same 2×2 layout as Slide 29 but for female visceral (n=180). Relationships are markedly weaker:

- ADRB2 vs age: slope = +1.02, R² = 0.004, p = 0.40 — NOT significant. Slope is directionally OPPOSITE to hypothesis (positive = ADRB2 goes UP with age).
- ADRB2 vs CCL2: slope = +0.11, R² = 0.002, p = 0.57 — NOT significant. Slope is positive (opposite direction to subq).
- ADRB2 vs fibrosis: slope = −0.108, R² = 0.039, p = 0.008 — only significant panel, weak effect.
- ADRB2 vs senescence: slope = +0.019, R² = 0.001, p = 0.73 — NOT significant.

**Caption**: "Female visceral: weaker ADRB2 relationships overall. Only fibrosis reaches significance (R² = 0.04, p = 7.6e-3). Depot-specific attenuation consistent with 2026-04-12 analysis."

This slide serves as the supplementary depot-specific caveat.

### Slide 31 — Q3: Saltiel-style panel — MALE, subcutaneous (sex-comparison)

Same layout for male subcutaneous (n=481). Demonstrates that the ADRB2 axis is NOT female-specific — the same negative relationships hold in males with higher statistical power:

- ADRB2 vs age: slope = −1.92, R² = 0.019, p = 0.002.
- ADRB2 vs CCL2: slope = −0.47, R² = 0.084, p = 8.6e-11.
- ADRB2 vs fibrosis: slope = −0.224, R² = 0.208, p = 4.5e-26 (strongest panel of the entire deck).
- ADRB2 vs senescence: slope = −0.244, R² = 0.107, p = 1.7e-13.

**Caption**: "Male subcutaneous (n=481) shows the same directional relationships as females, with higher power. ADRB2 vs fibrosis: R² = 0.21, p = 4.5e-26. Demonstrates that the ADRB2 axis is NOT female-specific — robust in both sexes."

### Slide 32 — Q3: Saltiel-style panel — MALE, visceral (sex-comparison)

Fourth Saltiel-style panel, male visceral (n=407). Fibrosis relationship survives; other panels are weak or directionally opposite:

- ADRB2 vs age: slope = −0.71, R² = 0.002, p = 0.32 — NS.
- ADRB2 vs CCL2: slope = **+0.26**, R² = 0.014, p = 0.017 — significant but POSITIVE (opposite direction to subcutaneous). This is a bona fide depot-specific reversal in male visceral tissue.
- ADRB2 vs fibrosis: slope = −0.095, R² = 0.040, p = 5.0e-5.
- ADRB2 vs senescence: slope ≈ 0, R² ≈ 0, p = 0.87.

**Caption**: "Male visceral: fibrosis relationship holds (R² = 0.04, p = 5.0e-5). CCL2 shows a weak POSITIVE relationship (R² = 0.014, p = 0.017) — opposite direction to subcutaneous, consistent with the depot-specific reversal noted on 2026-04-12. Age and senescence are not significant."

### Slide 33 — Q3: Saltiel-style regression stats — all panels

A 16-row summary table consolidating all four Saltiel panels (Slides 29-32) into a single view: columns for sex, depot, phenotype (y), n, slope vs ADRB2, R², p-value. Rows are ordered by sex then depot then target.

**Caption**: "Strongest effects: ADRB2 vs fibrosis in MALE subcutaneous (R² = 0.21) and FEMALE subcutaneous (R² = 0.17). Visceral relationships consistently weaker in both sexes."

Use this slide as a quick reference when discussing the Saltiel panels — it lets a reviewer see which panels have which effect sizes without flipping between Slides 29-32.

### Slide 34 — Cross-species concordance: OVX 042126 vs human GTEx (heatmap)

Single-image slide: the 12-row concordance heatmap color-coded by the 5-category classification (introduced 2026-04-22 after the triple-review bug fix):
- Dark green = CONCORDANT direction with human p<0.05 (e.g., ADRB3 both depots).
- Light green = CONCORDANT direction but human p≥0.05 (e.g., ADRB1 iWAT, p=0.228).
- Dark red = DISCORDANT direction with human p<0.05.
- Light red = DISCORDANT direction with human p≥0.05 (both visceral inflammation and fibrosis fall here).
- Grey = INDETERMINATE (mouse data not shown for that phenotype × depot).

**Caption summary**: "Concordant (human p<0.05): ADRB3 DOWN in both pWAT/iWAT; Senescence UP in pWAT/visceral. Concordant (human NS): ADRB1 iWAT (direction match, p=0.228). Discordant (human NS only): inflammation/fibrosis DOWN-trend in human visceral (p=0.13, p=0.69) vs UP in OVX pWAT. Indeterminate (OVX iWAT not shown): inflammation/fibrosis/senescence iWAT/subq; ADRB1 pWAT; ADRB2 both depots."

Critically, the "discordant" visceral inflammation/fibrosis rows both have human p>0.13, meaning the cross-species "divergence" is based on non-significant human trends rather than robust human findings in the opposite direction.

**Source**: `figs/next_steps_2026-04-21_adrb2_age_stratified/concordance_heatmap_with_ovx_042126.png`.

### Slide 35 — Concordance table — OVX 042126 observations vs human GTEx (detail)

The underlying 12-row concordance table with all columns visible: phenotype, mouse depot, human depot, OVX 042126 direction, human GTEx direction, human stat, concordance label (with "(NS)" suffix where applicable).

**Key rows to highlight**:
- ADRB3 pWAT/visceral: OVX DOWN vs human DOWN (p=0.003) → CONCORDANT.
- ADRB3 iWAT/subcutaneous: OVX DOWN (more pronounced) vs human DOWN (p=0.002) → CONCORDANT.
- ADRB1 iWAT/subcutaneous: OVX DOWN vs human DOWN (p=0.228) → CONCORDANT (NS) — direction match but non-significant human signal.
- ADRB1 pWAT/visceral: OVX NOT SHOWN vs human DOWN (p=0.246) → INDETERMINATE.
- ADRB2 both depots: OVX NOT EXPLICITLY SHOWN → INDETERMINATE on both rows.
- Inflammation pWAT: OVX UP vs human DOWN (p=0.131) → DISCORDANT (NS).
- Inflammation iWAT: OVX "UP (similar, not shown)" vs human UP (p=0.138) → INDETERMINATE (because mouse data wasn't directly shown).
- Fibrosis pWAT: OVX UP vs human DOWN (p=0.687) → DISCORDANT (NS).
- Fibrosis iWAT: OVX "UP (similar, not shown)" → INDETERMINATE.
- Senescence pWAT: OVX UP vs human UP (p=0.005) → CONCORDANT.
- Senescence iWAT: OVX "UP (similar, not shown)" → INDETERMINATE.

**Source**: `tables/next_steps_2026-04-21_adrb2_age_stratified/concordance_with_ovx_042126.tsv`.

**Revision history** (important for reviewer understanding):
- v4.5.0 initial build on 2026-04-21 had a regex bug that classified the three iWAT "UP (similar, not shown)" rows as CONCORDANT instead of INDETERMINATE, and did not flag ADRB1 iWAT's NS classification. Both were fixed on 2026-04-22 after a triple-reviewer audit; the v4.5.1 deck uses the corrected concordance output.

### Slide 36 — Interpretation and manuscript framing

Two-box layout. Left box ("What we can conclude") contains seven bullets summarizing the scientific conclusions:

1. ADRB3 declines with age in both depots in women AND men (p<0.05 in both sexes; CONCORDANT with OVX). Not female-specific.
2. OVX "iWAT>pWAT" asymmetry reproduces in HUMAN MALES (ADRB3 subq −0.017 vs visc −0.011) but REVERSES in FEMALES (subq −0.013 vs visc −0.021).
3. ADRB2 declines with age in subcutaneous in both sexes; pooled sex*age interaction NS (p=0.94). Visceral ADRB2–age slope is directionally POSITIVE in females (NS), opposite to catecholamine-resistance prediction.
4. ADRB2 × fibrosis correlation robust in subcutaneous (female R² = 0.17; male R² = 0.21). Weak in visceral.
5. Prog Lipid Res 2009: ADRB1 and ADRB2 (not ADRB3) are the primary lipolytic β-ARs in human adipose — the translational analog of OVX Adrb3 in humans is ADRB1 + ADRB2 combined.
6. Visceral inflammation/fibrosis "discordance" is direction-only (both human p>0.13); should NOT be framed as genuine cross-species divergence.
7. iWAT inflammation/fibrosis/senescence are INDETERMINATE (not concordant) because Kenichi's Slide 2 only shows pWAT mouse data; iWAT was noted as "similar" but not measured.

Right box ("Visualization recommendations (Q4)") answers Kenichi's fourth question with five concrete figure proposals:

1. **PRIMARY figure** for manuscript: female subcutaneous Saltiel-style panel (R² up to 0.17; Slide 29).
2. **COMPANION**: male subcutaneous Saltiel-style panel to show sex-agnostic robustness (Slide 31).
3. **RECEPTOR-DECLINE figure**: β-AR age trend by sex (Slide 27 figure).
4. **CONCORDANCE figure**: heatmap for discussion section (Slide 34 figure).
5. **Depot-comparison figure** (from 2026-04-12): retain the inflammation-by-depot plot (Slide 3) to document the visceral/subcutaneous divergence.

### Slide 37 — Caveats and proposed next steps

Two-box closer. Left box ("Caveats") lists five honest limitations:

1. GTEx age is binned (10-yr increments); using bin midpoints as a continuous predictor attenuates absolute slope estimates (p-values remain valid).
2. Female sample sizes in narrow age windows are modest (e.g., subcutaneous 40-49: n=43; 50-59: n=76; visceral peri window: n=36 / n=54).
3. Baseline GTEx does not capture acute hormonal withdrawal — steady-state post-menopausal tissue may differ from acute OVX.
4. CIBERSORTx HiRes (14 cell types) from 2026-04-12 has NOT yet been applied to ADRB2 correlations — a promising next step for adipocyte-specific vs macrophage-specific interpretation.
5. OVX 042126 does not directly show ADRB2 — adding mouse ADRB2 would complete the translational bridge.

Right box ("Proposed next steps") lists four concrete follow-up actions:

1. Finalize manuscript figures with Female SubQ Saltiel-style as primary.
2. Cell-type-resolved ADRB2 analysis using CIBERSORTx HiRes (adipocyte-specific expression trajectories).
3. Joint figure combining OVX mouse panels with human concordance for manuscript introduction/discussion.
4. Add mouse ADRB2 measurement if available in Kenichi's lab to resolve the INDETERMINATE rows in the concordance table.

---

## Appendix A — File and directory reference

### 2026-04-12 Update (Slides 1-14 source)

- R scripts (all under `R/`):
  - `next_steps_2026-04-12_kenichi_followup_targeted.R`
  - `next_steps_2026-04-12_glycolysis_gene_nodes.R`
  - `next_steps_2026-04-12_prepare_cibersortx_inputs.R`
  - `next_steps_2026-04-12_cibersortx_hires_de_analysis.R`
- Bash: `scripts/run_cibersortx_pipeline_2026-04-12.sh`
- PPTX builder: `scripts/make_pptx_2026-04-12_v4.4.0.py`
- Tables: `GTEx_v10_AT_analysis_out/tables/next_steps_2026-04-12_{kenichi_followup,glycolysis_gene_nodes}/`
- Figures: `GTEx_v10_AT_analysis_out/figs/next_steps_2026-04-12_{kenichi_followup,glycolysis_gene_nodes}/`
- Email draft (unsent): `docs/reply_to_kenichi_2026-04-12.md`

### 2026-04-12 HiRes (Slides 15-22 source)

- PPTX builder: `scripts/make_pptx_2026-04-12_v4.4.1_hires_update.py`
- CIBERSORTx outputs: `GTEx_v10_AT_analysis_out/cibersortx_output/` (raw), `tables/next_steps_2026-04-12_cibersortx_hires/` (DE tables), `figs/next_steps_2026-04-12_cibersortx_hires/` (volcanoes, box plots)

### 2026-04-21 Response (Slides 23-37 source)

- R script: `R/next_steps_2026-04-21_adrb2_age_stratified_saltiel_style.R`
- Python builders: `scripts/make_docx_2026-04-21_kenichi_response.py`, `scripts/make_pptx_2026-04-21_kenichi_response_v4.5.0.py`
- Tables (7): `GTEx_v10_AT_analysis_out/tables/next_steps_2026-04-21_adrb2_age_stratified/*.tsv`
- Figures (8): `GTEx_v10_AT_analysis_out/figs/next_steps_2026-04-21_adrb2_age_stratified/*.png`
- Quick email: `docs/reply_to_kenichi_2026-04-21_quick.md`
- Detailed Word response: `docs/response_to_kenichi_2026-04-21_detailed.docx`
- Kenichi input files: `references/042126_Email-from-Kenichi.txt`, `references/Catecholamine_resistance_in_OVX_042126.pptx`

### 2026-04-22 Rescale (this deck)

- PPTX rescaler: `scripts/make_pptx_2026-04-22_v4.5.1_rescale.py`
- This explainer: `docs/menopause_sns_gtex_enhanced_2026-04-22_v4.5.1_explainer.md`
- Deck: `docs/menopause_sns_gtex_enhanced_2026-04-22_v4.5.1.pptx`

---

## Appendix B — Reading order recommendations

For Kenichi review (first-time): Slide 24 (recap of his 042126 findings), then Slides 25-36 (Q1-Q4 answers), finally Slide 37 (caveats and next steps).

For Christoph review (manuscript discussion): Slide 3 (depot-specific inflammation), Slide 4 (aging vs menopause), Slides 17-18 (HiRes DE by depot), Slides 29-33 (Saltiel panels), Slides 34-36 (concordance + interpretation).

For a technical reviewer / statistician: Slides 2, 16, 28 (methods), Slides 33, 35 (complete stat tables), Slide 37 (caveats). The underlying TSVs (Appendix A) are recommended for reproducibility checks.

---

**Deck version**: v4.5.1 (2026-04-22), revised after removal of original slides 1-46
**Rescale rationale (historical)**: the original v4.5.1 deck contained 83 slides, where slides 1-46 were v4.3.0 baseline content (designed for 10×5.62 canvas) uniformly rescaled by 1.333× to fill the 13.33×7.50 canvas. In this revision those 46 baseline slides have been removed, leaving the 37 slides (previously 47-83) that cover the v4.4.0/v4.4.1/v4.5.0 additions.
**Verification**: 37 slides total; shape bounds check PASSED (on original v4.5.0 content); slide content is unchanged from v4.5.0 — only the deck-level slide indices shift by −46.

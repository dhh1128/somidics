# Empirical Plausibility and Measurement Agenda

## A. Empirical plausibility (non-exhaustive grounding)

Somidions are defined as visually observable, physically embodied features of a human subject, including both **positive features** (e.g., tattoos, scars, piercings, congenital anomalies) and **negative features** (anti-somidions), defined as the *absence* of features within a specified anatomical scope. The utility of somidions for discrimination does not depend on any single feature being rare; rather, it arises from the **composition of multiple weakly discriminating features**, combined with contextual constraints (e.g., visibility, stability over time, and resistance to spoofing).

While this paper does not attempt an exhaustive census of somidion prevalence, existing epidemiological, forensic, and population-survey literatures strongly suggest that:

1. **Many somidion classes are common but not universal**, making both presence- and absence-based predicates informative. Examples include tattoos, non-earlobe piercings, scars, and melanocytic nevi under standard clinical definitions.

2. **Anatomical localization materially increases discriminating power.** Even when a mark type is common at the whole-body level, its occurrence within a constrained region (e.g., facial sites, hands, oral cavity) is typically far less frequent.

3. **Anti-somidions are often as informative as positive marks.** For example, the absence of any tattoos, any facial piercings, or any scars within a visible scope can exclude large fractions of a population under realistic priors.

4. **Prevalence varies substantially by cohort, culture, and age**, but this variability does not undermine the concept; instead, it reinforces the need for stratified models and conservative assumptions.

Taken together, these observations support the claim that somidions occupy a plausible middle ground between high-entropy biometrics (e.g., fingerprints) and low-entropy demographic attributes, offering useful discrimination when combined, without requiring precision measurement at introduction.

---

## B. Selected bibliography for somidion quantification (illustrative, not exhaustive)

The following categories of sources already contain much of the raw material needed to quantify somidion prevalence rigorously. This paper treats them as *evidence on-ramps*, not as definitive measurements.

### 1. Population surveys of body modification

National or regionally representative surveys have reported prevalence of tattoos and piercings, sometimes with anatomical or visibility breakdowns. These sources are particularly valuable for whole-body priors and anti-somidions (e.g., “no tattoos anywhere”).

*Use for somidics:* whole-body presence/absence rates; cohort stratification; visibility proxies.

### 2. Clinical and dermatological epidemiology

Dermatology studies frequently report prevalence and distribution of melanocytic nevi, scars, and other skin features using clinician examination and standardized protocols. Definitions are typically explicit (e.g., size thresholds, lesion type).

*Use for somidics:* biologically grounded mark definitions; anatomical site distributions; age-dependent effects.

### 3. Forensic identification literature

Forensic anthropology and medicolegal studies routinely catalog tattoos, scars, and anomalies for identification purposes, often with fine-grained anatomical coding and photographic corroboration.

*Use for somidics:* region-specific prevalence; mark typologies; co-occurrence patterns; real-world identification relevance.

### 4. Congenital anomaly registries and administrative health data

Population-based registries and administrative datasets quantify rates of limb differences, ear anomalies, amputations, and related features.

*Use for somidics:* low-prevalence, high-stability somidions; clearly bounded definitions; long-term persistence.

### 5. Occupational, military, and intake screening datasets

Certain institutional datasets record visible marks due to policy relevance (e.g., tattoos in visible locations).

*Use for somidics:* visibility-conditioned prevalence; bias analysis; policy-driven under- or over-reporting effects.

These literatures are heterogeneous in method and scope, but collectively they demonstrate that somidions are already measured—albeit for different purposes—and can be repurposed under a unified framework.

---

## C. Sensitivity analysis: discriminating power without precise prevalence

To show that somidions can be useful without committing to exact prevalence values, we model discrimination as a function of prevalence alone.

Let a somidion predicate \( S \) have population prevalence \( p \). The self-information conveyed by observing \( S \) is:

\[
I(S) = -\log_2(p)
\]

For anti-somidions (absence predicates):

\[
I(\neg S) = -\log_2(1 - p)
\]

### Example ranges

| Prevalence \(p\) | Information \(I(S)\) |
|------------------|----------------------|
| 0.5              | 1.0 bit              |
| 0.1              | 3.3 bits             |
| 0.01             | 6.6 bits             |
| 0.001            | 10.0 bits            |

Even features that are “common” in colloquial terms (e.g., 10%) yield several bits of information. Crucially, **somidions are composable**. Under an independence assumption, the information from multiple predicates adds:

\[
I(S_1 \land S_2 \land \dots \land S_n) \approx \sum_{i=1}^{n} I(S_i)
\]

Because many somidions are correlated (e.g., multiple tattoos on one person), conservative models can apply correlation penalties or use bounds rather than point estimates. Even with such penalties, composing several modest predicates (e.g., 2–4 bits each) rapidly yields discrimination comparable to many deployed low-friction identity checks.

This analysis shows that the *existence* of plausible prevalence ranges—not their exact values—is sufficient to justify the somidion concept.

---

## D. Measurement agenda and future work

A disciplined somidion measurement program would include:

1. **Standardized definitions**  
   Clear operational definitions for each somidion class (e.g., tattoo vs temporary marking; melanocytic nevus vs lentigo), with explicit inclusion/exclusion criteria.

2. **Anatomical zoning standard**  
   A stable, hierarchical zone system enabling aggregation and refinement (whole body → region → subregion).

3. **Stratified sampling**  
   Measurement by age, sex, geography, and cultural context to avoid false universality.

4. **Presence and absence capture**  
   Explicit recording of anti-somidions (“none observed”) rather than treating absence as missing data.

5. **Co-occurrence modeling**  
   Empirical estimation of correlation between somidions to replace independence assumptions.

6. **Open, auditable datasets**  
   Where ethically permissible, publication of anonymized prevalence tables and protocols to enable replication.

The present work does not attempt this measurement itself; instead, it defines the conceptual and mathematical scaffolding needed to make such work coherent and comparable.

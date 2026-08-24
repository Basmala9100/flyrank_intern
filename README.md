# FlyRank ML Internship — Content Optimization Prioritization

**Applied Search Intelligence: Content Prioritization for Human Review**

This repository contains my work for the FlyRank ML Internship.

The project focuses on a practical content-optimization problem:

> **Which content pages should a content or SEO team review first?**

I built a decision-support workflow that starts with observable content and performance signals, establishes a transparent baseline, compares it with machine-learning models, validates the modeling approach, audits for leakage, and turns the validated output into a human-reviewed action playbook.

The goal is **prioritization**, not automatic content editing or a claim about Google's ranking algorithm.

---

## Project Overview

Content teams may have many pages that could potentially be improved, but limited time to review them.

Instead of treating every page equally, this project produces a ranked queue of pages that are worth investigating first.

The workflow is:

```text
FlyRank Content Data
        |
        v
Feature Preparation
        |
        v
Signal Audit
        |
        v
Transparent Baseline
        |
        v
Machine-Learning Model
        |
        v
Validation + Leakage Audit
        |
        v
Action Playbook
        |
        v
Ranked Review Queue
        |
        v
Human Review


The Problem

The project started from a simple operational question:

How can we prioritize content pages for review using observable signals without overstating what the data can prove?

The system therefore focuses on decision support.

It does not claim:

that the model predicts Google's algorithm,
that every recommended page needs an edit,
that an edit will cause a ranking recovery,
or that observational data proves causality.

The recommendations are candidates for human review.

Data

The project uses an anonymized FlyRank content-performance dataset.

The main data sources include:

dim_content
fact_content_daily_performance

The content data contains signals such as:

content creation and update dates,
content type,
search volume,
competition,
backlinks,
content depth,
word count,
optimization dates,
publication status.

Performance data provides observable search and engagement signals used to evaluate content behavior.

The public project intentionally excludes identifying client information.

Data Safety

This project follows the public-safe rules of the internship.

Client identities are not published.
Raw client domains and URLs are not published.
Private queries and identifying content fields are not published.
Sensitive client data is not added to the repository.
Outputs are framed using terms such as observed, measured, directional, and decision-support.
The project does not claim to reveal or predict Google's ranking algorithm.

The repository's data-use and leak-guard rules should be followed before committing new files.

Method

The project was developed progressively rather than starting with a complex model.

1. Signal Audit

Before modeling, I checked whether important signals were actually present and useful in the data.

Two signals were examined using bucket-level summaries and counts.

At least one of the audited signals was connected to a real FlyRank flag from the training material.

The purpose was to avoid building a rule around a signal without first checking whether the signal behaved as expected.

2. Baseline

I created a transparent baseline before introducing machine learning.

The baseline uses observable signals to produce:

a score,
one reason code,
and an action label.

The baseline creates a ranked queue of pages for review.

This provided a simple reference point that the Week-5 models had to beat.

3. Machine-Learning Model

The Week-5 modeling stage compared several approaches from the internship toolkit, including:

Logistic Regression
Decision Tree
Random Forest

The model was evaluated against the Week-4 baseline using the same ranking-oriented evaluation framework.

The purpose was not to reward complexity.

A more complex model was useful only if it provided better evidence for the actual prioritization task.

Model Evaluation

The starter modeling evaluation produced the following results:

Method	ROC AUC	Average Precision	Precision@50
Baseline rules	0.627	0.468	0.240
Logistic Regression	0.700	0.522	0.400
Decision Tree	0.742	0.575	0.540
Random Forest	0.750	0.618	0.740

The Random Forest produced the strongest Precision@50 in this evaluation.

However, these numbers are interpreted as evaluation results on the available anonymized starter slice, not as a production benchmark.

The modeling work also included validation and methodology checks in later weeks.

Validation

A major part of the project was checking whether the apparent model performance was trustworthy.

The validation work included:

an honest client-grouped validation design,
comparison of validation approaches,
feature leakage checks,
inspection of real failure examples,
and rewriting claims that went beyond the available evidence.

Client-grouped validation is important because rows from the same client can otherwise appear in both training and test data and make performance look stronger than it really is.

The project therefore treats validation design as part of the modeling work rather than as a final reporting step.

Leakage Audit

Features were reviewed for possible future information.

The main question was:

Could this feature contain information that would only be known after the decision point?

Features derived from future outcomes or post-decision behavior should not be used to justify a recommendation.

The final analysis therefore distinguishes between:

information available when making the decision,
information observed later,
and information that should not be used as an input.

This is important because a model can have excellent metrics while still being unusable if it has access to future information.

Action Playbook

The model output is converted into an action-oriented review queue.

Each recommendation contains:

a priority score,
a reason code,
an action label,
and enough context for a human reviewer to investigate the page.

The playbook also defines:

intended use,
human-review rules,
no-go cases,
monitoring triggers,
retraining considerations,
and practical cost/value thinking.

The model does not directly publish or modify content.

Intended Use

The intended use is:

Prioritize content pages for human review.

A content or SEO reviewer can use the ranked queue to decide where to spend investigation time first.

The output can support questions such as:

Which pages should I inspect first?
Why was this page prioritized?
Is the recommendation consistent with the page's current state?
Does the page actually need an update?
Is there another explanation for the observed behavior?
Human Review

Human review remains part of the workflow.

A reviewer should check:

Whether the recommendation makes sense for the actual page.
Whether the reason code matches the page's current state.
Whether important context is missing from the model.
Whether the proposed action is appropriate.
Whether there is a reason to reject the recommendation.

The model is therefore a prioritization aid rather than an autonomous content-management system.

What Should NOT Be Automated

The following should not be automatically executed based only on the model output:

publishing content changes,
deleting pages,
rewriting pages,
changing titles or metadata automatically,
declaring that a page will recover,
claiming that an edit caused a ranking improvement,
making client-facing claims without human review.

The model can recommend where to look.

A human decides what should actually happen.

Limitations

The project has several important limitations.

1. Observational data

The analysis uses observational data.

Therefore, a relationship between a signal and a later outcome does not prove that the signal caused the outcome.

For example:

A page being refreshed and later performing better does not by itself prove that the refresh caused the improvement.

2. Dataset scope

The modeling evaluation uses an anonymized starter slice rather than the complete production warehouse.

The starter dataset is approximately 30,000 rows, while the full warehouse is much larger.

Therefore, the reported metrics should not be presented as a benchmark for the entire FlyRank warehouse.

3. Model generalization

Model performance can change across:

clients,
time periods,
content types,
and different data distributions.

A strong result on one evaluation slice does not guarantee the same performance everywhere.

4. Target definition

The usefulness of the model depends on how the target is defined.

Changing the target definition can change what the model learns and how the output should be interpreted.

5. Human context

The model does not have every piece of context available to an experienced content reviewer.

Business priorities, editorial requirements, search intent, brand considerations, and recent changes may not be fully represented in the available features.

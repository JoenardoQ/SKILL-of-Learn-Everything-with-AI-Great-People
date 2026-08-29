# Development evaluations

This directory contains development evidence and release gates. It is deliberately
outside `ai-learning-coach/`, because evaluation plans are not part of the runtime
Skill bundle.

`ai-learning-coach.v4.json` is the machine-valid source of truth for routing and
behavior cases. The specification defines what must be run; validation does not
mean that the model runs occurred or passed.

The campaign is classified as `high-risk` on the evaluation dimension because
the Skill consumes untrusted-content surfaces such as serialized handoff fields
and instruction-like text embedded in study material. A user-selected paper is
authorized and trusted as the object of study, but its claims are not
pre-validated facts and its embedded text has no instruction authority. This
classification does not imply that the Skill itself is authorized to perform
external or destructive actions.

Validate the inventory from the repository root:

```bash
python3 /path/to/agent-skill-author/scripts/validate_eval_spec.py \
  evals/ai-learning-coach.v4.json
```

Before a behavioral release, run the same cases without the Skill and with the
candidate Skill in fresh contexts, five repetitions per variant, then manually
review every result. Record host, model, version, candidate revision, raw output,
case ID, repetition, observations, verdict, and limitations in a separate result
artifact. Structural and inventory validation alone do not establish discovery,
loading, behavioral improvement, or cross-host compatibility.

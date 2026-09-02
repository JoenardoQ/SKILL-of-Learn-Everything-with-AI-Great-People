# AI Learning Coach

[简体中文](README.zh-CN.md)

AI Learning Coach is a Codex Agent Skill for people who want to understand a subject, not merely receive an answer. It teaches from first principles, lets you attempt meaningful work, diagnoses the earliest gap, and checks whether you can transfer the idea independently.

Use it for concepts, engineering skills, mathematical reasoning, academic papers, social phenomena, practice, and review.

## What you get

- A learning path matched to your goal and current level.
- Necessary foundations before you are asked to reason with unfamiliar material.
- A few substantial questions instead of a long quiz of tiny prompts.
- Hints that preserve useful effort, followed by a fresh independent check.
- Clear separation between guided practice, independent transfer, and closed-book recall.
- Optional belief calibration when evidence can genuinely test a prior prediction or confidence judgment.
- Paper reading that distinguishes authors' claims, inspected evidence, assumptions, and unresolved questions.
- Social analysis that separates facts, causal explanations, predictions, interpretations, and value judgments.
- A compact handoff record when you want to pause or continue with another Agent.

The coach does not force a lesson when you only want a direct answer. Ask for the answer directly at any time.

## Install in Codex

Ask Codex to install the Skill from this repository:

```text
Use $skill-installer to install
https://github.com/JoenardoQ/SKILL-of-Learn-Everything-with-AI-Great-People/tree/main/ai-learning-coach
```

Start a new Codex task after installation so the Skill can be discovered in a fresh context. If you install manually, copy the complete `ai-learning-coach/` directory to `$CODEX_HOME/skills/ai-learning-coach/` without flattening its contents.

## Quick start

Invoke the Skill by name and describe what you want to be able to do:

```text
$ai-learning-coach Help me understand Bayesian updating well enough to
interpret a medical-test result. Assume I know basic probability.
```

The coach will normally:

1. identify your learning goal and starting level;
2. select a teaching mode and define one manageable phase;
3. provide the foundations needed for the next inference;
4. ask you to explain, predict, derive, solve, or apply;
5. diagnose gaps and give the smallest useful hint;
6. check the idea with a different case and later with closed-book recall.

You remain in control. You can ask it to slow down, skip material you already know, change the goal, show a worked solution, or answer directly.

## Example requests

### Learn a concept

```text
$ai-learning-coach Teach me how database indexes work from first principles.
I can write SQL but do not understand query plans.
```

### Learn by building

```text
$ai-learning-coach Help me learn React state by building a small filterable list.
Do not give me the complete implementation before I try each meaningful step.
```

### Reconstruct a proof

```text
$ai-learning-coach Help me reconstruct why the square root of 2 is irrational.
Teach any prerequisite I am missing, then test me with a related proof.
```

### Study a paper

Attach or link the paper when possible, then state your objective:

```text
$ai-learning-coach Help me critique this paper. Separate what the authors report
from what we can verify in the text, and teach me to evaluate the central claim.
```

The coach can optimize a paper session for understanding, critique, reproduction, comparison, or application. It will state whether it has metadata, an abstract, selected excerpts, or the full main text instead of implying access it does not have.

### Analyze a social phenomenon

```text
$ai-learning-coach Teach me how to analyze falling birth rates without jumping
from correlation to causation. Compare mechanisms and the evidence each predicts.
```

### Review a topic

```text
$ai-learning-coach Test what I still remember about TCP congestion control.
Start with closed-book retrieval and adapt the review to my mistakes.
```

## Learning modes

The Skill chooses the mode for the current objective and can change it as the task evolves:

| Mode | Best for | Typical learner work |
| --- | --- | --- |
| Hands-on engineering | Tools, systems, debugging, implementation | Build, inspect, predict, and diagnose |
| Abstract concept | Models, mechanisms, theories, definitions | Explain, compare, classify, and apply |
| Mathematical proof | Formal arguments and derivations | State assumptions, justify steps, and test boundaries |
| Hybrid | Work that combines several of the above | Alternate between formal understanding and practical use |

The exact sequence is adaptive. It is not an eight-question script, and a zero-baseline learner receives instruction and a worked example before being asked for a near-transfer attempt.

## Progress, mastery, and review

Receiving a hint does not count as independent mastery. The coach only claims mastery after both:

- a correct closed-book response; and
- a meaningfully different transfer task completed without a material cue.

It may suggest a later retrieval schedule based on performance. Suggestions are not calendar reminders unless you explicitly request one and the host provides a supported reminder tool.

When you pause or change Agents, ask for a handoff:

```text
$ai-learning-coach Create a LEARNING_STATE handoff so I can continue later.
```

The handoff stores bounded learning progress, not source passages, credentials, permissions, or hidden context.

## When not to use it

Skip the Skill when you only need a finished answer, report, translation, edit, or other deliverable. It is also not a substitute for a qualified professional in safety-critical, medical, legal, or financial decisions.

AI Learning Coach can structure practice and inspect reasoning, but it cannot prove long-term retention or real-world competence from a single conversation. It does not invent inaccessible paper content, citations, tool results, or completed reminders.

## Project contents

The runtime Skill is the `ai-learning-coach/` directory:

```text
ai-learning-coach/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── learning-modes.md
    ├── learning-state.md
    ├── paper-learning.md
    └── social-phenomena-learning.md
```

The project contains procedural Markdown rather than an executable application. `SKILL.md` holds the shared coaching contract; references are loaded only for the selected learning mode or specialization. The current protocol version is `2.1`, and version-2 handoffs remain readable when produced by protocol `2.0`.

To validate a checkout with the Agent Skill Author validator, run:

```bash
python3 /path/to/agent-skill-author/scripts/validate_skill.py ai-learning-coach
git diff --check
```

Validation checks package structure and document integrity. It does not by itself establish teaching effectiveness, host discovery behavior, or compatibility with every Agent host.

## License

[MIT](LICENSE)

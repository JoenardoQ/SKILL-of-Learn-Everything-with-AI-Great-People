# Learning mode playbooks

Use this reference when designing a substantive lesson, changing modes, or diagnosing whether the teaching method matches the learner's objective. The mode applies to the current objective, not permanently to the whole subject.

## Hands-on engineering

Optimize for the learner's ability to build, operate, debug, or modify something observable.

- Establish the concrete outcome, environment, prerequisites, and safety or reversibility constraints.
- Give the smallest runnable demonstration, then have the learner perform the next action.
- Ask for a prediction before execution; inspect actual output afterward and reconcile differences.
- Prefer commands, artifacts, tests, measurements, and troubleshooting evidence over extended theory.
- Explain theory at the point where it changes an implementation decision.
- Finish with an independent variation that produces a verifiable result.

Use the phase checkpoint structure `prediction → execution → evidence → diagnosis → verified fix or variation`. Do not turn a request for a tutorial into a sequence of abstract quizzes.

## Abstract concept

Optimize for a coherent mental model and the ability to explain relationships.

- Begin with the problem or observation that made the concept necessary.
- Develop the concept from primitive ideas through its causal or logical sequence.
- Contrast it with nearby concepts and identify scope, boundary cases, and common misconceptions.
- Use examples and analogies only after defining the mechanism; state where each analogy breaks.
- Ask the learner to reconstruct the concept in their own words before introducing dense notation.

Use the phase checkpoint structure `development → mechanism → contrast → novel case → boundary`. Do not accept restating a label as an explanation of the mechanism.

## Mathematical proof

Optimize for validity, derivational transparency, and the ability to reproduce the argument.

- State definitions, assumptions, domains, and the exact proposition before manipulating symbols.
- Present equations alongside a plain-language account of what each expression represents.
- Justify every non-obvious transformation by naming the definition, identity, theorem, or inference rule used.
- Separate implication from equivalence, necessary from sufficient conditions, and examples from proof.
- Check exceptional cases and conditions under which a division, inverse, limit, approximation, or theorem is valid.
- Have the learner prove one link or lemma at a time, then reconstruct the full proof without notes.

Use the phase checkpoint structure `assumptions → justified derivation → conceptual meaning → exceptional case`. A worked numerical example may motivate a proposition but cannot establish it generally.

## Hybrid

Split mixed topics into labeled phases and apply the relevant playbook within each phase. State transitions explicitly. Use the usual order **conceptual model → mathematical justification → hands-on implementation**, unless the learner's goal supports a different order.

Size the first phase around one dependency boundary, not the entire hybrid map. For a zero-baseline learner, stop after the earliest bridge that supports a meaningful inference and a worked example. Name later phases as deferred rather than teaching them immediately. In a mixed technical topic, first secure one complete bridge such as `primitive objects → repeatable relation → justified consequence`; defer advanced extraction or implementation machinery until the learner can explain that bridge.

Bridge every transition:

- concept to mathematics: map each symbol and relation to the conceptual object it represents;
- mathematics to implementation: map the formal operation to data, state, code, or an observable action;
- implementation to concept: explain what the measured behavior does and does not establish about the model.

Do not jump from intuition to notation or code without this mapping. Completion requires the learner to explain how the representations correspond, not merely succeed in each phase separately.

## Adaptation when the learner cannot start

“I don't know” is evidence that the current generation demand exceeds the established foundation; it is not a reason to repeat the same question in smaller wording.

1. Identify the earliest missing prerequisite without testing more unstated facts.
2. Teach one coherent foundation with a fully worked example.
3. Explain why each step follows and where the method stops applying.
4. Label and give one near-transfer practice task that changes a meaningful detail without expanding into later prerequisites.
5. If the learner is still blocked, reduce one dimension of difficulty and demonstrate the missing link.

Return to the mode-specific phase checkpoint after the learner can use the repaired prerequisite.

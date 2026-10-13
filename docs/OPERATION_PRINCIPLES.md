# Operating principles (not a wired platform component)

Principles for long-running operational automation are stated separately from the verified GitOps experiment:

1. Define explicit inputs and permitted changes before execution.
2. Separate action ownership, implementation review and the decision to accept evidence.
3. Record bounded checkpoints that make interrupted work understandable.
4. Resume from verified checkpoints without assuming success.
5. Compare claims against measured state; stop on missing or contradictory evidence.
6. Preserve a concise, redacted evidence trail for subsequent review.

These principles explain engineering judgment; they do **not** imply a policy engine, checkpoint runner or approval gate was connected to the Canary environment.

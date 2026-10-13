# Operating principles (design statement)

Task-control principles, documented separately from the GitOps experiment (design statement):

1. Define explicit inputs and permitted changes before execution.
2. Separate action ownership, implementation review and the decision to accept evidence.
3. Record bounded checkpoints that make interrupted work understandable.
4. Resume from verified checkpoints without assuming success.
5. Compare claims against measured state; stop on missing or contradictory evidence.
6. Preserve a concise, redacted evidence trail for subsequent review.

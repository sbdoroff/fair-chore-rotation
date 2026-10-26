# Chore-planning agent skill

> Sanitized portfolio example. This file describes the workflow; it is not a standalone executable skill.

## Objective

Create a weekly household chore chart that follows the saved rotation, respects eligibility and active constraints, checks workload by task count and estimated effort, and records any necessary rebalance.

## Inputs

- **Household context:** Retrieved through an MCP server when running in the original agent environment. Use it to resolve the current eligible participants; do not copy private profile data into public examples.
- **Rotation state:** The durable source for task rotation positions, recent history, active constraints, and documented exceptions.
- **Chart template:** Defines the printable output layout.

Household context and rotation state are separate inputs. Do not substitute one for the other.

## Workflow

1. Read the current rotation state and active constraints.
2. Retrieve the household context needed to determine task eligibility.
3. Advance each task according to its saved rotation order.
4. Check task-specific constraints and any recent-assignment limits.
5. Evaluate distribution by task count and estimated effort.
6. If a check fails, move an eligible task from an overloaded participant to an underloaded participant. Preserve the normal rotation position unless the state explicitly says otherwise.
7. Recheck all constraints after rebalancing. Record why each override was made.
8. Generate the print-ready HTML chart.
9. Update the proposed rotation state and include both changes in a reviewable pull request.

## Balance checks

- Compare task counts across participants; a low task count does not by itself prove a light workload.
- Estimate effort using the task estimates in the active state or planning data.
- Compare each participant's total estimated effort with the others.
- Treat estimates as approximations, not observed durations.
- Do not rebalance someone into a task they are not eligible to perform.
- Document the rebalance and verify the resulting allocation again.

## Output expectations

- A browser-friendly, print-ready HTML chart.
- An updated rotation state with the next rotation positions and concise assignment notes.
- A pull request containing the proposed chart and state changes for human review.

## Safety and privacy

- Use household information only for the requested planning task.
- Keep private profile details out of chart metadata and public examples.
- Do not infer a new eligibility exception; require it to be explicitly recorded.
- When an input is ambiguous, follow the latest documented precedent and surface the assumption for review.

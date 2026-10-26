# Architecture

## Components and responsibilities

| Component | Responsibility | Does not own |
|---|---|---|
| AI agent | Executes the planning workflow, evaluates context against the skill, and produces proposed changes | Durable rotation history |
| Chore-planning skill | Defines workflow order, eligibility handling, checks, rebalancing, and output expectations | Household profile data |
| Household MCP server | Supplies household context to the agent through MCP tools | Chore rotation state and chart rendering |
| Rotation-state file | Stores rotation positions, recent assignments, and active rules | Household profile management |
| HTML chart | Presents the current assignments in a printable format | Future rotation state |
| Pull request | Makes generated chart and state changes reviewable | Automatic proof that assignments are fair |

## Run flow

1. A hosted agent run is initiated on the recurring schedule.
2. The agent loads the chore-planning skill and the current rotation state.
3. It requests the household context needed to evaluate eligibility.
4. It advances task rotations and applies active constraints.
5. It checks the proposed assignment counts and estimated effort.
6. If a check fails, it rebalances eligible assignments and checks again.
7. It generates the printable chart, updates the proposed rotation state, and opens a pull request.
8. A person reviews the proposed changes.

The production workflow was first exercised locally and refined over multiple iterations before recurring hosted runs were introduced.

## State and assignment policy

Rotation positions are stored separately from household profile data. That lets a run continue from a known state without treating the previous chart as the source of truth. The state file records the current assignment, next rotation order, applicable eligibility, and any documented exception or rebalance.

The skill calls for balance checks by both task count and estimated labor. The estimates are planning inputs, not measured time. A rebalance must respect eligibility and active task constraints, and should be documented in both the proposed chart and state update.

## Boundaries

- The sample in this repository is synthetic and does not call an MCP server.
- No household profile, names, location, ages, credentials, or real assignment history are included.
- The recurring hosted-agent configuration is not included in this showcase.
- A pull request enables review; it does not independently validate subjective fairness.

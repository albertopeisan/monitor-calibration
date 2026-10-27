## Sub-agents

Never spawn, invoke, or use sub-agents or delegate work to other agents under any circumstances. Complete all work in the current agent session.

## Usage-limit checkpoints and resumption

- At task start, look for a matching unfinished checkpoint under `~/.local/state/agent-checkpoints/<repository-key>/`, where `<repository-key>` identifies the canonical repository path. Keep each task/session in its own `<task-id>.md` temporary state file outside version control; never overwrite another task's checkpoint.
- Create a checkpoint early for multi-step work and refresh it after meaningful milestones. When usage information is available, save before any applicable usage or context budget falls to 2% remaining, or immediately on a limit warning, leaving enough budget to finish the save. If remaining usage is unavailable, checkpoint regularly rather than relying on a final warning.
- Record the actual state: timestamp, repository path, branch and HEAD, objective and scope, user instructions and approvals, completed work, changed files and pre-existing edits to preserve, checks run and their results, failures/blockers, pending operations, and precise next steps and commands. Record a reset time only when reported by the service; never guess. Exclude secrets, credentials and unnecessary personal data.
- Write updates atomically (write a sibling temporary file, then rename it) and verify the checkpoint is readable. Saving state never authorizes staging, committing, pushing, stashing, reverting, switching branches, or changing unrelated work. If execution must stop, report the checkpoint path and what remains; never claim the task is complete.
- After limits reset and execution is resumed, read the matching checkpoint, re-read applicable instructions, inspect the current files, Git state and pending operations, and continue from the first unfinished step without repeating completed actions. Revalidate stale facts and task-scoped approvals. Mark the checkpoint complete only after the objective is fulfilled. Instructions alone do not schedule a restart or guarantee a save after an abrupt cutoff.

# Parallel-write worktrees

Read only when concurrent project writes justify isolation. Sequential work and read-only review need no extra worktree.

Luna prepares one worktree/branch per writer (default codex/ prefix) from a common verified starting state. Preserve relevant dirty/untracked inputs; do not silently substitute HEAD. If safe setup is unavailable, serialize.

Briefs name absolute checkout, baseline, ownership, interfaces, and report path accessible to the lead. Writers stay in assigned checkouts; never switch another writer's branch. One shared cycle state remains lead-owned. Worktrees are not sandboxes: coordinate shared databases, services, ports, and external generated outputs.

After writers stop, Luna integrates into the designated result checkout, preserving unrelated changes and resolving conflicts, then validates the combined task. Scoped patches may avoid commits; existing authorization rules still govern commits/merges. Fresh review covers all integrated task changes, not just isolated branches. Repairs target the integrated state.

Luna reports fingerprints and result location without code/diffs. Retain unintegrated or uncommitted work; remove worktrees only with authorization and verified preservation.

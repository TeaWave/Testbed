# Testbed

Sandbox repo for testing Lumo's GitHub connection. ✅ Working — README added by Lumo via API.

## Collab script sketch: `pair_relay.py`

A rough draw of a "relay race" co-coding loop — human and Lumo alternate
turns editing a file, with a shared session log keeping the context:

```text
        ┌─────────────┐         ┌─────────────┐
        │   HUMAN     │         │    LUMO     │
        │  (turn n)   │ ──────▶ │  (turn n+1) │
        └──────┬──────┘         └──────┬──────┘
               │ edits file             │ reviews + edits
               ▼                        ▼
        ┌──────────────────────────────────────┐
        │           shared_session.log          │
        │   (who changed what, and why)         │
        └──────────────────────────────────────┘
```

```python
# pair_relay.py — rough sketch, not runnable
SESSION_LOG = "shared_session.log"

def human_turn(task):
    change = prompt_human_edit(task)      # human edits code
    log(SESSION_LOG, "human", change.note)
    return change

def lumo_turn(change):
    review = lumo_review(change)          # Lumo critiques + patches
    apply_patch(review.patch)
    log(SESSION_LOG, "lumo", review.reason)
    return next_task(review)

while not task.done:
    change = human_turn(task)
    task = lumo_turn(change)              # handoff — relay baton
```

Ideas it plays with:
- Turn-taking instead of chat free-for-all — each side sees the other's last change.
- The session log doubles as context for the AI on every handoff.
- Any file in the repo can be the "baton".

*Sketch by Lumo, Nov 2026.*

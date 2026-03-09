# AI Dev Env Workflow

```
  ┌──────────────────┐
  │      prime       │
  └─────────┬────────┘
            │
            ▼
  ┌──────────────────┐
  │   plan-feature   ├·· auto ·····> ┌─────────────┐   ┌────────────────────────────┐
  └─────────┬────────┘               │ explore-api │   │ acceptance-criteria-define │
            │                        └─────────────┘   └────────────────────────────┘
            ▼
  ┌──────────────────┐
  │      commit      │
  └─────────┬────────┘
            │
            ▼
  ┌──────────────────┐
  │ create-worktree  │
  └─────────┬────────┘
            │  "new session in worktree"
            ▼
  ┌──────────────────┐
  │      prime       │
  └─────────┬────────┘
            │
            ▼
  ┌──────────────────┐
  │     execute      ├·· auto · parallel ·····> ┌─────────────┐   ┌──────────────────┐   ┌──────────────────────────────┐
  └─────────┬────────┘                          │ code-review │   │ execution-report │   │ acceptance-criteria-validate │
            │                                   └──────┬──────┘   └──────────────────┘   └──────────────────────────────┘
            ▼                                  "if issues found"
  ┌──────────────────┐                                 │
  │  system-review   │                                 ▼
  └─────────┬────────┘                        ┌─────────────────┐
            │                                 │ code-review-fix │
            ▼                                 └────────┬────────┘
  ┌──────────────────┐                               auto
  │ cleanup-progress │                                 │
  └─────────┬────────┘                                 ▼
            │                                    ┌──────────┐
            ▼                                    │ validate │
  ┌──────────────────┐                           └──────────┘
  │      commit      │
  └──────────────────┘
```

**Legend**
- Blue `[ ]` = manually triggered
- Purple `< >` = manual or auto-called
- `····>` = auto-invoked
- `──>` = triggered conditionally

# Codex configuration repository

- Keep edits in this repository. The maintainer approves deployment to user or server configurations, changes to other projects, and any GitHub push separately.
- Maintain global instructions in [global/AGENTS.md](global/AGENTS.md), role behavior in [agents/](agents/), and personal skills in [skills/](skills/). This root file applies to this repository.
- Update both READMEs when configuration paths or manual deployment instructions change. Preserve the configured models and machine-specific settings unless the request covers them.
- When checking instruction discovery or skill behavior, use an isolated Codex home and disposable fixtures in this repository. Select the model under evaluation explicitly and keep test state out of tracked files.

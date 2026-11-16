# DeepSeek Harness CI

This repository runs selected checks for public pull requests in `ZulaDDSH/deepseek-harness` on GitHub-hosted runners. Dispatch **Harness PR checks** with the pull request number and its exact 40-character head SHA. The workflow refuses a stale head and runs the static, unit, and Windows build lanes against that commit.

Results appear in this repository's Actions tab; they are not automatically attached to the source pull request. The workflow does not store a write token or run deployment steps. Additional lanes from the harness CI are not yet included.

The optional **regenerate workflow guest** input uploads the generated guest source as an artifact for review; it does not write to the harness repository.

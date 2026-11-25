# DeepSeek Harness CI

This repository runs selected checks for public pull requests in `ZulaDDSH/deepseek-harness` on GitHub-hosted runners. Dispatch **Harness PR checks** with the pull request number and its exact 40-character head SHA. The workflow refuses a stale head and runs the static, unit, and Windows build lanes against that commit.

Results appear in this repository's Actions tab; they are not automatically attached to the source pull request. The workflow does not store a write token or run deployment steps. Additional lanes from the harness CI are not yet included.

To show results on the source pull request, add a repository secret named `HARNESS_STATUS_TOKEN`: a fine-grained token limited to `ZulaDDSH/deepseek-harness` with **Commit statuses: Read and write** and nothing else. The workflow then marks the static, unit, and Windows lanes as pending on the pull request's head commit and reports each lane's result when it finishes, linking back to the run. The token is used only in jobs that never run pull request code. Without the secret, the workflow runs as before and reports nothing.

**Harness PR watcher** runs every 15 minutes and starts Harness PR checks for each open deepseek-harness pull request whose current head commit hasn't been tested. It skips a commit that already has a run (run names carry the PR number and SHA) or already carries `Harness CI /` statuses. Dispatch it by hand with **dry run** to see what it would start. Scheduled runs only fire from the default branch.

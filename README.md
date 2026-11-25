# DeepSeek Harness CI

This repository runs selected checks for public pull requests in `ZulaDDSH/deepseek-harness` on GitHub-hosted runners. Dispatch **Harness PR checks** with the pull request number and its exact 40-character head SHA. The workflow refuses a stale head and runs the static, unit, and Windows build lanes against that commit.

Results appear in this repository's Actions tab, and on the source pull request when the status token below is set. The workflow does not run deployment steps. Additional lanes from the harness CI are not yet included.

Enable **desktop only** to build an unsigned Windows x64 test installer from the verified pull request head. Use pull request `0` and the exact current `master` SHA to build the merged source. The installer uses nonfunctional test update and policy URLs and is uploaded as an Actions artifact.

To show results on the source pull request, add a repository secret named `HARNESS_STATUS_TOKEN`: a fine-grained token limited to `ZulaDDSH/deepseek-harness` with **Commit statuses: Read and write** and nothing else. The workflow then marks the static, unit, and Windows lanes as pending on the pull request's head commit and reports each lane's result when it finishes, linking back to the run. The token is used only in jobs that never run pull request code. Without the secret, the workflow runs as before and reports nothing.

**Harness PR watcher** starts Harness PR checks for each open deepseek-harness pull request whose current head commit hasn't been tested, checking every 2 minutes. It skips a commit that already has a run (run names carry the PR number and SHA) or already carries `Harness CI /` statuses. GitHub's schedule is best effort and can go hours without firing, so each watcher run keeps checking for about an hour and then starts the next run itself; an hourly schedule only restarts the chain if it ever stops. Start it by hand from `main` to begin the chain, or with **dry run** to see what it would start.

The optional **regenerate workflow guest** input uploads the generated guest source as an artifact for review; it does not write to the harness repository.

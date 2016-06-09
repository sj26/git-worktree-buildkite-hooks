# Git Worktree Buildkite Hooks

This changes the  default checkout behaviour in the Buildkite agent bootstrap
using [hooks](https://buildkite.com/docs/agent/hooks) to use [Git
worktrees](https://git-scm.com/docs/git-worktree). It uses a single bare clone
of each repository and creates a new worktree for each job. This means multiple
agents on the same machine can share fetched repositories, multiple pipelines
configured with the same repository can also share fetched repositories, and
starting builds with pre-fetched repositories can be a little faster.

We also optimize the case where we have already fetched a git commit so that no
remote transfer is necessary.

## Usage

Copy the hooks directory contents into your Agent's hooks directory.

For example, on OS X with a homebrew-installed buildkite-agent:

```
# If you haven't installed the agent yet:
$ brew install buildkite/buildkite-agent/buildkite-agent

# Presuming you use the default locations:
$ cd /usr/local/etc/buildkite-agent/hooks

# Now download the hooks:
$ curl -sL https://github.com/sj26/git-worktree-buildkite-plugin/raw/master/hooks/pre-checkout > pre-checkout
$ curl -sL https://github.com/sj26/git-worktree-buildkite-plugin/raw/master/hooks/checkout > checkout
$ curl -sL https://github.com/sj26/git-worktree-buildkite-plugin/raw/master/hooks/post-command > post-command
$ curl -sL https://github.com/sj26/git-worktree-buildkite-plugin/raw/master/hooks/post-artifact > post-artifact

# And make sure they're executable:
$ chmod +x pre-checkout checkout post-command post-artifact
```

On Linux your hooks directory is likely `/etc/buildkite-agent/hooks`.

## Caveats

Git worktrees are a fairly new feature. They don't always play well with git
submodules 

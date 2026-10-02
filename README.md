<p align="center">
    <a href="https://github.com/lupaxa-git-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/git-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">Git Change Branch Name</h1>

Rename a local Git branch and update the same name on the remote.

## What it Does

`git-change-branch-name` renames a branch in the current repository, pushes that new name, sets it as the upstream, and deletes the old name on the remote. Commit history is left as it is. Only the branch name changes.

Use it when a branch was pushed under the wrong name and every clone should follow the new name.

## Install

With Homebrew:

```bash
brew tap the-lupaxa-project/tap
brew trust the-lupaxa-project/tap
brew install git-change-branch-name
```

Or run the script from a clone: `./src/git-change-branch-name --help`.

## Quick Start

```bash
./src/git-change-branch-name --summary new-name          # plan only — no changes
./src/git-change-branch-name -n old-name new-name        # dry-run, commands simulated
./src/git-change-branch-name -y old-name new-name        # rename, then update origin
./src/git-change-branch-name -L -y new-name              # rename the current branch locally
```

**This deletes the old remote branch.** Prefer `--summary` or `-n` first.

## Default Behaviour

With the new branch name and no mode flags, the script will:

1. Require a Git working tree (or `--git-dir` together with `--work-tree`)
2. Take the current branch as the old name when only one name is given
3. Refuse to run if the new name is already a different branch
4. Print an action plan
5. Ask you to type `RENAME` before changing anything
6. Publish the new name to `origin` and set it as upstream
7. Delete the old name on `origin`
8. Rename the local branch

`--local-only` skips the remote. `--yes` (and `--force`) skips the confirmation. `--summary` prints the plan and exits.

If a later step fails, run the same command again. Completed steps are skipped.

## Common Options

| Flag                  | Purpose                                              |
| :-------------------- | :--------------------------------------------------- |
| `-o, --old-name NAME` | Branch to rename (default: the current branch)       |
| `-N, --new-name NAME` | New branch name                                      |
| `-r, --remote NAME`   | Remote to update (default: `origin`)                 |
| `-L, --local-only`    | Rename the local branch and do not touch the remote  |
| `-n, --dry-run`       | Simulate commands without changing anything          |
| `-S, --summary`       | Print a detailed plan based on all options and exit  |
| `-y, --yes`           | Skip the interactive `RENAME` confirmation           |
| `-f, --force`         | Same as `--yes`                                      |
| `-d, --git-dir PATH`  | Path to the repository `.git` directory              |
| `-w, --work-tree PATH`| Path to the working tree (use with `--git-dir`)      |
| `-V, --version`       | Print version and exit                               |
| `-h, --help`          | Show help and exit                                   |

```bash
./src/git-change-branch-name --help
```

## Examples

Preview renaming the current branch:

```bash
./src/git-change-branch-name --summary feature/new-name
```

Rename a branch you are not on, and leave the remote alone:

```bash
./src/git-change-branch-name --local-only --yes \
  feature/old-name feature/new-name
```

Rename a repository that is not the current directory, then update `origin`:

```bash
./src/git-change-branch-name \
  --git-dir /path/to/repo/.git \
  --work-tree /path/to/repo \
  --old-name feature/old-name \
  --new-name feature/new-name \
  --remote origin \
  --yes
```

## Safety Notes

- `--dry-run` prints the action plan and simulates commands without asking for `RENAME`.
- Real runs require typing `RENAME` unless you pass `-y` / `--yes` (or `-f` / `--force`).
- `--summary` resolves the branch and the remote, then prints the plan only.
- The new name is published before the old remote name is deleted. A failed delete can be finished by running the command again.
- Commit hashes do not change. Checkouts that tracked the old name need to fetch and use the new name.
- `--local-only` renames the local branch and leaves the remote, and its upstream, as they are.
- A repository with no remotes is renamed locally. Pass `--remote` only when that remote exists.

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>

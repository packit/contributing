# Contribution guidelines for Packit projects

Thank you for your interest in contributing to one of our projects.
The following is a set of guidelines for contributing to our projects.

Use your best judgement, and feel free to propose changes to this document in a pull request.

By contributing to this project you agree to the Developer Certificate of Origin (DCO). This document is a simple statement that you, as a contributor, have the legal right to submit the contribution. See the [DCO](DCO) file for details.

# Reporting Bugs

Before creating bug reports, please check a list of known issues to see if the
problem has already been reported (or fixed in a `master` branch).

If you're unable to find an open issue addressing the problem, open a new one.
Be sure to include a **descriptive title and a clear description**. Ideally, please
provide:

- version you are using
- the command you executed and a debug output (using option `--debug`)

If possible, add a **code sample** or an **executable test case** demonstrating the expected behavior that is not occurring.

**Note:** If you find a **Closed** issue that seems like it is the same thing that you're experiencing, open a new issue and include a link to the original issue in the body of your new one.
You can also comment on the closed issue to indicate that upstream should provide a new release with a fix.

## Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues.
When you are creating an enhancement issue, **use a clear and descriptive title** and **provide a clear description of the suggested enhancement** in as many details as possible.

# Guidelines for Developers

## Is this your first contribution?

Please take a few minutes to read GitHub's guide on [How to Contribute to Open Source](https://opensource.guide/how-to-contribute/).
It's a quick read, and it's a great way to introduce yourself to how things work behind the scenes in open-source projects.

## Dependencies

If you are introducing a new dependency, please make sure it's added to:

- `setup.cfg`

## Changelog

When you are contributing to changelog, please follow these suggestions:

- Changelogs is maintained mainly during release process.
- The changelog is meant to be read by everyone. Imagine that an average user
  will read it and should understand the changes.
- Every line should be a complete sentence. Either tell what is the change that the tool is doing or describe it precisely:
  - Bad: `Use search method in label regex`
  - Good: `X now uses search method when...`
- And finally, with the changelogs we are essentially selling our projects:
  think about a situation that you met someone at a conference and you are
  trying to convince the person to use the project and that the changelog
  should help with that.

## How to contribute code to our projects

1. Create a fork of the desired repository.
2. Create a new branch just for the bug/feature you are working on.
3. Once you have completed your work, create a Pull Request, ensuring that it meets the requirements listed below.

## Requirements for Pull Requests (PR)

- Use `pre-commit` (see [below](#pre-commit)).
- Follow git workflow (see [below](#git-workflow))
- Cover new code with a test case (new or existing one).
- All tests have to pass.
- Create a PR against the `master` branch.
- The `mergeit` label:
  - Is used to instruct CI and/or reviewer that you're really done with the PR.
  - PRs are merged if they have `mergeit` label **AND** are approved.
- Status checks SHOULD all be green.
  - Reviewer(s) have final word and HAVE TO run tests locally if they merge a PR with a red CI.

### `pre-commit`

There's a [pre-commit](https://pre-commit.com) config file in [.pre-commit-config.yaml](.pre-commit-config.yaml).

You need to install pre-commit to [utilize](https://pre-commit.com/#usage) it locally on your workstation, either from your distribution

    dnf install pre-commit

or from PyPI:

    pip3 install pre-commit

- `pre-commit install -t pre-commit -t pre-push` - to install pre-commit into your [git hooks](https://githooks.com). pre-commit will from now on run all the checkers/linters/formatters on every commit. If you later want to commit without running it, just run `git commit` with `-n/--no-verify`.
- Or if you want to manually run all the checkers/linters/formatters, run `pre-commit run --all-files`.

### Git workflow

- Write reasonable commit messages (See [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)).
- Use common sense when creating commits, not too big, not too small.
  You can also squash them at the end of review. Try to think about the reviewer
  going through the pull request and commits. For example don't make too many changes
  that aren't related, e.g. resolving 2 unrelated issues in one PR or having all
  changes in one commit is not a good idea.
- Rebasing lets you resolve any merge conflicts on your side. Also it allows CI
  to check everything is passing, i.e. no mistakes were made by fixing conflicts.
- You can mention issues that will be closed in a PR description, e.g.:
  `Closes #123`, `Fixes #123`; [more info](https://docs.github.com/en/enterprise/2.16/user/github/managing-your-work-on-github/closing-issues-using-keywords)

#### Rebasing

To keep our git history clean and easy to navigate we rebase our branches before
merging as mentioned above. `pre-commit` checks if you need to rebase.

For more info regarding rebasing and merging see:

- [Atlassian tutorial for merging and rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Git - When to Merge vs. When to Rebase](https://derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/)
- [Git team workflows: merge or rebase?](https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase)

##### pre-commit git hook and rebasing

We have switched our rebase checks from pre-commit hook to pre-push hook, so the
problem and possible solutions described below should not be relevant anymore.

In case you have encountered any problems we recommend following new installation
instructions:

```sh
$ pre-commit install -t pre-commit -t pre-push
```

And in case you have already installed pre-commit as git hooks **before** we
switched, we recommend you to try following to reinstall them:

```sh
$ pre-commit install -t pre-commit -t pre-push -f
```

<details>
<summary>Rebase to commit, but commit to rebase</summary>

In case you have installed pre-commit as a git hook and you want to commit changes,
you might encounter a problem when you need to rebase your branch, but to do that
you need to commit changes. [Closer info](https://github.com/packit/pre-commit-hooks/issues/2).

To resolve this issue there are 3 options:

1. skip the rebase check in pre-commit hook:<br>
   `env SKIP=check-rebase git commit`
2. skip whole pre-commit check with:<br>
   `git commit -n` or `git commit --no-verify`
3. stash your changes, rebase, pop the stash and commit

We recommend 3rd option, for the 1st it's easy to forget to rebase afterwards and
for the 2nd you can additionally fail other checks.

</details>

#### Examples of git history

<details>
<summary>Git history that we want to have</summary>

```
*   e3ed88b (HEAD -> contribution-guide, upstream/master, origin/master, origin/HEAD, master) Merge pull request #470 from lachmanfrantisek/fix_lru_cache
|\
| * 1ab7d9f Use parenthesis for lru_cache decorator
|/
*   e9c5bb4 Merge pull request #468 from lachmanfrantisek/hostname
|\
| * de2d6cf Add hostname property to service class
| * cd2ed17 Fix inheritance of GitlabService from BaseGitService
|/
*   028c344 Merge pull request #465 from packit/0.15.0-release
|\
| * 7b619d6 0.15.0 release
|/
*   acdf7df Merge pull request #463 from lachmanfrantisek/support-multi-part-namespaces
```

</details>

<details>
<summary>Git history that we <b>don't</b> want to have</summary>

```
*   4c8aca8 Merge pull request #120 from TomasTomecek/add-zuul
|\
| * fc2b449 use zuul for realz now
| * 2304683 add zuul config
| * 5285bd3 bump base image to F30
* |   4d0fbe2 Merge pull request #114 from lbarcziova/create_method_create_release
|\ \
| * | 36a9396 test changed
| * | 22f681d method create release for github created
* | |   2ef4ea1 Merge pull request #119 from lbarcziova/clean_utils.py
|\ \ \
| |/ /
|/| |
| * | 5f1b8f0 unused functions removed
* | |   a93c361 Merge pull request #117 from marusinm/add-getreleases-to-abstract
|\ \ \
| |/ /
|/| |
| * | 0a97236 add get_releses for Pagure
| * | 55e4c57 add get_releases/get_release into abstract.py
|/ /
* |   badeddd Merge pull request #101 from marusinm/read-permissions
```

</details>

### Checkers/linters/formatters

To make sure our code is [PEP8](https://www.python.org/dev/peps/pep-0008/) compliant, we use:

- [black code formatter](https://github.com/psf/black)
- [Flake8 code linter](http://flake8.pycqa.org)
- [mypy static type checker](http://mypy-lang.org)
- [pyupgrade to upgrade syntax](https://github.com/asottile/pyupgrade)

They are included as pre-commit tasks if necessary.

---

Thank you for your interest!

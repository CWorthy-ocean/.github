# Contributing to [C]Worthy's projects

Thank you for your interest. This page applies to every repository in the
CWorthy-ocean organization that doesn't have its own contributing guide. The
three main projects have detailed guides of their own:

- [C-Star](https://c-star.readthedocs.io/en/latest/contributing.html): the
  workflow framework.
- [ROMS-Tools](https://roms-tools.readthedocs.io/en/latest/contributing.html):
  input generation and analysis.
- [UCLA-ROMS](https://cworthy-ucla-roms.readthedocs.io/): the ocean model.

Before opening an issue or a pull request anywhere in the organization, read
our [policy on AI tools](AI_POLICY.md). In short: you may use AI tools to help
write code and to draft descriptions, you write your own replies in discussions,
a person answers for everything that gets posted, every pull request declares
its AI use in one line, and you are responsible for understanding and having
tested what you submit.

## Questions

Ask in the repository's Discussions tab where one exists, otherwise open an
issue. A maintainer who knows the answer will reply; it may take a few days.

## Bug reports

Open an issue with what you ran, what you expected, what happened, and the
versions involved (`cstar --version` prints them for the C-Star stack). A
minimal example that reproduces the problem is the most useful thing you can
include.

## Pull requests

- Comment on the issue you want to work on and wait to be assigned before you
  start, unless you are already a collaborator.
- Keep the change focused on one thing. Open an issue first for anything larger
  than a fix.
- Run the repository's tests and `pre-commit run --all-files` before you push;
  each project's guide says how.
- Fill in the pull request template, including the `AI disclosure:` line.
- Don't add AI attribution trailers to commits (see the policy).
- Expect a reviewer to ask you to explain a part of the change or show how you
  tested it.

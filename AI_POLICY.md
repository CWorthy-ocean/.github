# Policy on AI tools

[C]Worthy's open-source projects are maintained by a small team. We use AI tools
in our own work, and we value direct communication between people. This policy
covers the use of large language models (LLMs) and other generative AI tools in
contributions to any repository in the CWorthy-ocean organization: code,
documentation, issues, pull requests and discussions.

## How our software is developed

Our projects are *AI-assisted*: people design them, direct the changes and check
the results. Much of the code, and most of our pull request descriptions, are
written with AI tools. The rules below apply to the maintainers in the same way
they apply to everyone else.

## Describing versus talking

We treat two kinds of writing on GitHub differently.

**Describing something: AI drafting is fine.** This covers pull request
descriptions, notes for reviewers, file lists, test results, and bug reports
that describe what happened and how to reproduce it. You may draft these with AI
tools, as the maintainers do. Check that every statement is accurate before
anyone relies on it, keep it as short as it can be, and declare the AI use (see
[Disclosure](#disclosure)). You are responsible for what you post.

**Talking with people: write it yourself.** This covers replies to review
comments, answers to questions, comments in issue and pull request discussions,
and discussion posts. Don't post AI-generated text here, and don't pass a
reviewer's question to an AI and paste back its answer. Short, imperfect writing
that shows what you understand is better than polished text that doesn't.

If you aren't sure which kind of text you're writing, ask whether a reader would
assume a person wrote it to them. If so, write it yourself.

### Don't answer other people's questions with AI

If someone asks a question in an issue or a discussion, they want an answer from
a person who knows the project. They can ask an AI tool themselves. If you know
the answer, reply in your own words. If you don't, leave the question for someone
who does. A generated reply that sounds confident but is wrong is worse than no
reply, and it costs the maintainers more time to correct than to answer.

### A person answers for every post

Everything in our repositories is posted under a person's own account, and that
person answers for it. When that happens depends on what was posted:

- **Pull requests and issues** may be opened by a tool under your account
  before you have read them; the maintainers do this. You read and check a pull
  request before it merges, and you answer for it from the moment it is open:
  if a reviewer asks about it before you have caught up, say so rather than
  relaying the tool's answer. An issue a tool filed under your account is yours
  to confirm, correct or close.
- **Replies, review comments and discussion posts** are written by a person
  (see [Talking with people](#describing-versus-talking) above). You may use AI
  tools to help you review a change, and the maintainers do; what you post is
  your own assessment, in your own words, with any tool output worth sharing
  quoted and marked (see [Quoting AI output](#quoting-ai-output)).

What isn't allowed is unattended automation in the talking category: a bot or
agent that answers questions, posts reviews or replies in discussions without a
person writing or reading each one.

### Language help is always fine

You may use AI tools, or any other tool, to translate your writing into English
or to fix grammar and spelling. The ideas and content must still be yours, and
the result should stay close to what you wrote. Writing in your own language and
adding a translation is also welcome.

### Quoting AI output

If AI output is useful in a discussion, for example a suggested fix or an
explanation of an error, paste it into a quote or a code block. Say where it
came from and what you think of it.

## Code and documentation

You may use AI tools to help write code, tests or documentation. You are
responsible for what you submit, which means:

- **You understand the change at the level of what it does and why.** You know
  what problem it solves, how the approach works and what it could affect. You
  don't need to be able to recite every line, but you should be able to answer a
  reviewer's question about it.
- **You have tested it yourself.** The automated tests pass, and you have run
  the new or changed behavior at least once and checked that the output is
  correct and scientifically sensible. "It runs" is not enough. Our software
  produces simulations that people make decisions on; a change that passes the
  tests and gets the physics wrong is a bug.
- **Your tests test something.** A test that asserts whatever the code happens
  to do, or that can't fail, is not a test. Reviewers check this.
- The change follows the project's conventions and is no larger than it needs
  to be.
- You have the right to submit it under the project's license.

Nothing in this policy requires a paid tool. Contributions made without any AI
assistance are welcome in every project.

## Disclosure

Every pull request description carries one line, which the pull request
template provides:

```
AI disclosure: none
```

or

```
AI disclosure: <tool>; <what it did>
```

For example: `AI disclosure: Claude Code; drafted the code, tests and this
description`. One line is enough. If it's missing, a reviewer will ask for it.

**Don't put AI attribution in commit messages.** No `Co-authored-by`,
`Assisted-by`, `Generated-by` or similar trailer naming an AI tool, on any
commit. A co-author trailer asserts authorship, and GitHub displays it as such on
every commit that reaches `main`. Some tools add such a trailer by default;
remove it before you push. The disclosure line in the pull request is the one
place AI involvement is recorded.

## New contributors and trusted contributors

The rules above apply to everyone. How much we ask before reviewing a change
depends on how well we know you. AI helps with reviewing as well as writing, but
a person still has to confirm that a change is correct and fits the project.
That check is where most of our time goes, and it takes longer when we don't
know the author.

**Trusted contributors** are the members of the CWorthy-ocean GitHub
organization and the collaborators the maintainers have added to a repository,
such as partner-institution developers or people with a record of good
contributions here. They may open larger or AI-heavy changes, including
refactors. If you'd like to become a trusted collaborator, reach out to the
maintainers of the repository you work with.

**Everyone else** is very welcome. To keep reviews manageable:

- **Only take on issues you can work through yourself.** You need to understand
  the problem and be able to answer questions about your change. If you can't
  do that without an AI tool, the issue isn't a good fit yet.
- **Keep pull requests small and focused.** We won't review large AI-generated
  refactors or new features that weren't agreed in advance. Open an issue first
  for anything larger than a focused fix.
- **Expect questions.** A reviewer may ask you to explain part of the change, or
  to show how you tested it, before going further.

## Enforcement

Maintainers may hide or delete comments, and close issues or pull requests, that
don't follow this policy, without a detailed review. We use two labels so the
response is the same in every repository:

- `ai-needs-explanation`: a reviewer has asked a question only the author can
  answer. The issue or pull request stays open until they do.
- `ai-policy-not-met`: closed without review, with a link to this policy. This
  includes contributions that look like AI output nobody answers for: generic,
  off-target, or unable to answer follow-up questions.

Multiple `ai-policy-not-met` closures may lead to a block from the organization's
repositories.

---

*Adapted from the AI policies of SciPy, NumPy, matplotlib, xarray, pandas, LLVM
and MOM6.*

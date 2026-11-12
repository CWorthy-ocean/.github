# [C]Worthy AI policy

Use AI tools freely to do your work, but a person must thoughtfully initiate every
contribution and converse directly with other humans.

This policy covers every repository in the CWorthy-ocean organization. It applies to the
maintainers in the same way as to everyone else: we write and reivew much of our own code with AI tools.
We also value community, discussion, and the need for confidence in scientific models.
We want to hear from you, and want you to hear from us.

1. **AI tools may do the work.** You may use them to:
   - write code, tests and documentation
   - draft descriptions: pull requests, bug reports, test results
   - summarize information, such as logs, results or a long thread
   - help you review a change
   - translate your writing, or fix its grammar and spelling, as long as the
     ideas are yours

2. **A person stands behind it and does the talking.**
   - **Humans talk to humans.** Replies, reviews, answers, and
     discussion posts are written by a person. If AI helped you review a
     change, you may put the relevant portion of its review in a quote or a code block and say
     where it came from, but also add your own view, in your own words. No AI
     agent should directly reply, ask questions, or join a discussion on its own.
   - **Own what is posted under your account.** You are accountable for it
     from the moment it is open: check a pull request before it merges, and
     confirm, correct, or close an issue that a tool opened.
   - **Understand and test what you submit.** Be able to explain what the change does and why. Check that the results are correct and scientifically
     sensible. Passing tests are not enough. Keep changes focused and in line with the project's conventions.

4. **Disclose AI use in one place.** Every pull request description carries
   one line, which the pull request template provides: `AI disclosure: none`
   or `AI disclosure: <tool>; <what it did>`. If it's missing, a reviewer will
   ask for it. Don't add AI trailers to commits (`Co-authored-by`,
   `Assisted-by`, `Generated-by` or similar). Some tools add one by default;
   remove it before you push.

5. **Trust scales review.** Trusted contributors are the members of the
   CWorthy-ocean organization and the collaborators the maintainers have added
   to a repository. They may make larger, AI-heavy changes. Everyone else is
   very welcome: keep pull requests small, open an issue first for anything
   larger than a focused fix, and expect questions. If you'd like to become a
   trusted contributor, reach out to the maintainers of the repository you
   work with.

6. **Non-compliance gets closed.** Maintainers may hide comments, and close
   issues or pull requests, that don't follow this policy, without a detailed
   review. That includes contributions that look like AI output nobody answers
   for: generic, off-target, or unable to answer follow-up questions. Repeated
   violations may lead to a block from the organization's repositories.

---

*Adapted from the AI policies of SciPy, NumPy, matplotlib, xarray, pandas, LLVM
and MOM6.*

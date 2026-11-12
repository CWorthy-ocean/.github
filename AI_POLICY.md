# [C]Worthy AI policy

Use AI tools freely to do your work, but a person must thoughtfully initiate every
contribution and converse directly with other humans.

This policy covers every repository in the CWorthy-ocean organization. The
maintainers use AI to write code and review pull requests, and the policy
applies to them in the same way as to everyone else. We also value community
and discussion, and people have to be able to trust scientific models. We want
to hear from you, and want you to hear from us.

1. **AI tools may do the work.** You may use them to:
   - write code, tests, and documentation
   - draft descriptions: pull requests, bug reports, summaries of test results
   - summarize information, such as logs, results, or a long thread
   - help you review a change
   - translate your writing, or fix its grammar and spelling, as long as the
     ideas are yours

2. **A person stands behind it and does the talking.**
   - **Humans talk to humans.** Replies, reviews, answers, and discussion
     posts are written by a person. If you share AI output in a reply, such as
     part of an AI review, put it in a quote or a code block and say where it
     came from, but also add your own view, in your own words. No AI agent may
     directly reply, ask questions in a thread, or join a discussion on its
     own.
   - **Own what is posted under your account.** You are accountable for it
     from the moment it is open: check a pull request before it merges, and
     confirm, correct, or close an issue that a tool opened.
   - **Understand and test what you submit.** Be able to explain what the
     change does and why. Run it, and check that the results are correct and
     scientifically sensible. Passing tests are not enough. Write tests that
     would fail if the code were wrong, not ones that only confirm what it
     already does.

3. **Disclose AI use in PR descriptions, omit trailers.**
   - Every pull request description carries one line, which the pull request
     template provides: `AI disclosure: none` or
     `AI disclosure: <tool>; <what it did>`.
   - Don't add AI trailers to commits (`Co-authored-by`, `Assisted-by`,
     `Generated-by`, or similar). Some tools add one by default; remove it
     before you push.

4. **We may close what doesn't follow this policy.** Maintainers may hide
   comments, and close issues or pull requests, that don't follow it.
   That includes contributions that look like AI output nobody answers for:
   generic, off-target, or unable to answer follow-up questions.

---

*Informed by the AI policies of SciPy, NumPy, matplotlib, xarray, pandas, LLVM,
and MOM6.*

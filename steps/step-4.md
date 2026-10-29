# Step 4: Prove the contract with tests and write the evidence note

## The idea

A test is a claim that can be rerun: "if someone breaks this rule, this test fails". A test only earns that claim once you've seen it fail. Until then, all you know is that it ran. That's why this Step asks you to break the code on purpose for each rule, watch the matching test fail, and put the code back.

Good contract tests are small. They build one proposal and check what happens, with no stack and no server, because the contract is plain Python. Each test guards one rule, so when one fails you know which rule broke. A rejection test should check that construction fails and that it fails for the right reason. Otherwise a mistake in a different field makes it pass for the wrong reason.

The evidence note is the other half of the proof. It records what you saw, with the command and the time, so someone who wasn't there can check your claims against the code. The note isn't code. Writing it after your final `poe verify` run doesn't undo that run, because the run checked your code and the note changes none.

> Covered in **CI Gates and Release Verification**.

## What's expected

- Your test file sits where `poe student-tests` and `poe verify` collect it, and `poe verify` passes with it.
- There's a test for each rule the contract enforces: valid proposals of each kind, an unknown kind, input on the wrong kind, a tool call without input, a negative count, and one per decision of yours.
- For each rule, you broke the code once, saw its test fail, and restored it.
- `docs/capstone/task-01.md` has **What I built**, **How I checked it** and **What I'd defend**, each filled, with the command and time for every result.
- After your last code change, `poe contract` and `poe auth-checks` still show what Step 2 showed.

## When you're stuck

### Your tests aren't collected

<details><summary>Hint 1</summary>

Run `poe student-tests`. Does it report your test count, or zero?

</details>

<details><summary>Hint 2</summary>

Read the `student-tests` entry in `pyproject.toml`. Which folder does it collect from?

</details>

<details><summary>Hint 3</summary>

Check your file name and your test function names. Do they follow the same pattern as the existing tests under `tests/unit/`?

</details>

### Breaking the code doesn't make any test fail

<details><summary>Hint 1</summary>

Which rule did you break, and which test claims to guard it? Read that test line by line.

</details>

<details><summary>Hint 2</summary>

Does the test build exactly the case the rule rejects? A test that builds a valid proposal can't notice a missing rejection.

</details>

<details><summary>Hint 3</summary>

What does the test do if construction succeeds? A rejection test must fail when no error is raised. Look at how `tests/unit/domain/test_domain.py` asserts a rejection.

</details>

<details><summary>Hint 4</summary>

Does the test match the error message, or only the error type? Match the message, so a failure on a different field doesn't count as a pass.

</details>

### Each break takes minutes to check

<details><summary>Hint 1</summary>

Which command are you running after each break? Does it start the whole stack?

</details>

<details><summary>Hint 2</summary>

Your contract tests need no stack. Which command runs only your tests, in seconds?

</details>

<details><summary>Hint 3</summary>

Use that command for every break-and-restore, and keep `poe verify` for the final run after your last code change.

</details>

### A test passes even though it can't fail

<details><summary>Hint 1</summary>

What would have to change in the code for this test to fail? If you can't name a change, the test asserts something that's always true.

</details>

<details><summary>Hint 2</summary>

Does the test check values, or only that an object exists? A constructed proposal always has a kind.

</details>

<details><summary>Hint 3</summary>

Check the boundary from both sides: the lowest valid count and one below it. Would your tests notice if the bound moved by one?

</details>

### You don't know which `poe verify` run to cite

<details><summary>Hint 1</summary>

Which run checked your final code? Did you change any code after it, even a test?

</details>

<details><summary>Hint 2</summary>

If you changed code afterwards, run `poe verify` again. That's the run to cite.

</details>

<details><summary>Hint 3</summary>

Write the note after that run, with its command and time. The note isn't code, so writing it doesn't undo the run. Then run `poe contract` and `poe auth-checks` once more, because `poe verify` runs neither.

</details>

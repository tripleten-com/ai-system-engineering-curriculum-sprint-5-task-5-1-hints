# Step 4: Prove the contract with tests and write the evidence note

## Breaking the code doesn't make any test fail

<details><summary>Hint 1</summary>

Which rule did you break, and which test claims to guard it? Read that test: does it build the exact case the rule rejects?

</details>

<details><summary>Hint 2</summary>

Does the test expect a rejection, but would it also pass if construction succeeded? Look at what your test asserts when no error is raised.

</details>

<details><summary>Hint 3</summary>

Does the test fail for the reason you expect? Match the error message, not only the error type, so a failure on a different field doesn't count as a pass.

</details>

## Each break takes minutes to check

<details><summary>Hint 1</summary>

Does this test need the stack? The contract is plain Python. Which command runs only your tests?

</details>

## A test passes even though it can't fail

<details><summary>Hint 1</summary>

What would have to change in the code for this test to fail? If you can't name a change, the test asserts something that's always true.

</details>

<details><summary>Hint 2</summary>

Check the lowest valid count as well as a negative one. Would your tests notice if the bound moved by one?

</details>

## You don't know which `poe verify` run to cite

<details><summary>Hint 1</summary>

Which run checked your final code? The note isn't code, so writing it afterwards doesn't change what that run checked.

</details>

<details><summary>Hint 2</summary>

Did you change any code after that run, even a comment in a test? If so, run it again and cite the new run.

</details>

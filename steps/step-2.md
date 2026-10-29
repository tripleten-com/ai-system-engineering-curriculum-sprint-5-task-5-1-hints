# Step 2: Inspect the ports and controls the agent will use

## `poe queue-contract` passes, but you never see the success message

<details><summary>Hint 1</summary>

Where does that message get printed? The check runs inside a one-off container. Look at what the command prints in your terminal and what the test it runs asserts.

</details>

<details><summary>Hint 2</summary>

Open the test the command runs and find where it reads the container's output. If the message were missing, would the test still pass?

</details>

## Your port list doesn't match what a reviewer expects

<details><summary>Hint 1</summary>

Did you take the methods from the port or from an adapter? An adapter can carry helpers the port never promises.

</details>

<details><summary>Hint 2</summary>

Open each file in `src/ports/` and list only what the `Protocol` declares. Then check your list against `src/ports/__init__.py`: does it export exactly the five ports you listed?

</details>

## A token row looks wrong

<details><summary>Hint 1</summary>

What's the difference between "we don't know who you are" and "we know who you are, and you may not do this"? Which status code goes with each?

</details>

<details><summary>Hint 2</summary>

For each fixture name, ask whether the token itself is genuine. A genuine token for the wrong service, the wrong role or without the right scope is a different case from a forged or expired one. Which of the fixtures were issued for a different service altogether?

</details>

<details><summary>Hint 3</summary>

Compare every row with the list in the lesson, not just the first few. A table that prints a row for each fixture hasn't passed until each status matches.

</details>

## The version id and fingerprint don't match

<details><summary>Hint 1</summary>

Which exception made the worker's most recent authentication? Run the auth check first, then read the authentication record.

</details>

<details><summary>Hint 2</summary>

Was the secret rotated since the worker last called the model? Compare the current and previous versions that `poe secret-status` prints with the one `poe provider-auth-check` reports.

</details>

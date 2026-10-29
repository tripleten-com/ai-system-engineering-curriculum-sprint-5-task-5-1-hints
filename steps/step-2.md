# Step 2: Inspect the ports and controls the agent will use

## The idea

A port is a promise written as code. Each file in `src/ports/` declares a `Protocol`: the methods any implementation must offer, with their inputs and outputs, and nothing about how they work. An adapter is one way of keeping that promise, for example the emulator for the model, or LocalStack's S3 for the object store. The application, and soon your agent, only ever calls the port, so you can swap the adapter without touching the caller.

That's why you list the port's methods and not the adapter's. The port is everything the agent is allowed to rely on. An adapter may carry extra helpers, but the agent must never depend on them.

The controls from Project 4 are the other half of the baseline. Token verification decides who may read an exception. The secret store decides which provider key the worker presents. You record what they do today so that, after every change you make in this Project, you can show they still do exactly that. A `401` means the caller couldn't be identified at all. A `403` means the caller was identified and isn't allowed.

> Covered in **Ports and Adapters: Coldline's Shape**, **Identity and Token Verification** and **Trust Boundaries & Access Control**.

## What's expected

- `poe contract`, `poe queue-contract`, `poe runtime-contract` and `poe fidelity-check` each end in a pass, run as supplied.
- Your notes list the five ports by class name, each with the methods its `Protocol` declares today.
- You can say in one or two sentences what `docs/fidelity/ModelProvider.md` says the emulator can't prove.
- `poe auth-checks` prints one row per token fixture, and every row's status matches the list in the lesson: `200` for `dispatcher-valid` only.
- `poe secret-status` and `poe provider-auth-check` show the same version id and fingerprint, and neither prints a secret value.

## When you're stuck

### A contract check fails, or passes without the message you expected

<details><summary>Move 1</summary>

Which of the four checks failed, and at which test? Read the test's name in the output.

</details>

<details><summary>Move 2</summary>

Is the stack running? Two of the four checks need it: `poe queue-contract` and `poe runtime-contract`. `poe contract` and `poe fidelity-check` only read files.

</details>

<details><summary>Move 3</summary>

If `poe queue-contract` passes but you never saw its success sentence, open the test it runs. Does the test read the container's output and assert the sentence for you?

</details>

<details><summary>Move 4</summary>

These checks run unchanged from Project 4. If one fails on a fresh start, it's the environment, not your code: restart the stack, run it again, and if it still fails, keep the output and contact course support.

</details>

### Your port list doesn't match what a reviewer expects

<details><summary>Move 1</summary>

Did you take the methods from the port or from an adapter?

</details>

<details><summary>Move 2</summary>

Open each file in `src/ports/` and find the class that is a `Protocol`. List only the methods it declares.

</details>

<details><summary>Move 3</summary>

Check your list against `src/ports/__init__.py`. Does it export exactly the five ports you listed?

</details>

### You can't say what the emulator can't prove

<details><summary>Move 1</summary>

Open `docs/fidelity/ModelProvider.md`. Which part lists what the emulator proves, and which part lists what no local check can show?

</details>

<details><summary>Move 2</summary>

For each limit, ask: what would you need a real model for, to see it? Behavior under load, answers you didn't script, and cost are typical.

</details>

<details><summary>Move 3</summary>

Write the limits in your own words, one or two sentences. They go into your note's **What I'd defend** as the limits of every check in this Task.

</details>

### A token row looks wrong

<details><summary>Move 1</summary>

What's the difference between "we don't know who you are" and "we know who you are, and you may not do this"? Which status code goes with each?

</details>

<details><summary>Move 2</summary>

For each fixture name, ask whether the token itself is genuine. A forged or expired token is a different case from a genuine token for the wrong job.

</details>

<details><summary>Move 3</summary>

Which fixtures are genuine tokens issued for another service altogether? The API can't accept those as a caller, so they're not a permission problem.

</details>

<details><summary>Move 4</summary>

Compare every row with the lesson's list, one by one. A table that prints a row for each fixture only passes when every status matches.

</details>

### The version id and fingerprint don't match

<details><summary>Move 1</summary>

When did the worker last present a key? `poe provider-auth-check` reports the most recent authentication.

</details>

<details><summary>Move 2</summary>

Run `poe auth-checks` first. It creates an exception, so the worker authenticates again. Then read the authentication record.

</details>

<details><summary>Move 3</summary>

Was the secret rotated since then? Compare the current and previous versions in `poe secret-status` with the one the record reports. The record should match the current version.

</details>

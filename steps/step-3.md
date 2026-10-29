# Step 3: Write the action contract

## The idea

A model answers in text, and text can say anything. The agent you're about to build turns the model's answer into an action, so it needs the answer in a shape a program can check without interpreting sentences. That shape is the action contract.

The contract makes three promises:

- **A closed list of kinds:** the model may only suggest calling the tool, asking for approval, refusing or finishing. Anything else isn't a proposal.
- **A fixed shape per proposal:** every proposal has a kind, a reason for the person reading the run, a token count, and, only for a tool call, the tool's input.
- **Rules checked on construction:** a bad combination, such as a tool call with nothing to call, fails at the moment someone tries to build it, not later in the middle of a run.

Checking on construction is the key idea. If the type itself refuses bad values, every piece of code that receives one can trust it, and no part of the agent needs its own copy of the rules. The domain models beside yours already work this way: look at how they bound a number or reject a pair of values that don't fit together.

The request is the other direction: what the agent sends when it asks for one proposal. It holds only what an answerer needs to pick one answer.

> Covered in **A Typed Contract from a Non-Deterministic Source**, in **Guarding Model Output and Personal Data**.

## What's expected

- In the Python shell, `from domain.contracts import ActionKind, ProposedAction, ProposalRequest` works, or the same from the module you chose.
- One proposal of each kind constructs with the fields it requires.
- An unknown kind, a `tool_input` on a kind other than `call_tool`, a `call_tool` without one, and a negative count each raise an error on construction.
- What happens to a count of `"7"` or `true`, to an empty `reason` and to an empty `{}` is something you decided, and you can show the behavior in the shell.
- `ProposalRequest` holds what an answerer needs to find one list of answers and one entry in it, and you can say why each field is there.
- `poe unit` and `poe contract` still pass, so the platform's own contracts are untouched.

## When you're stuck

### You're not sure where the contract goes

<details><summary>Move 1</summary>

What's in `src/domain/contracts.py` today? Read it before you write anything.

</details>

<details><summary>Move 2</summary>

Search the supplied code and tests for imports from that file. What would break if one of those names disappeared?

</details>

<details><summary>Move 3</summary>

Add your three types to that file, or to a new module beside it. Leave every existing type exactly as it is, and run `poe unit` to confirm nothing else changed.

</details>

### A fifth kind can still be created

<details><summary>Move 1</summary>

Try building a proposal with a misspelled kind in the shell. Does it construct?

</details>

<details><summary>Move 2</summary>

What type does your `kind` field have? If it's a plain string, what stops any string from getting in?

</details>

<details><summary>Move 3</summary>

Look at how `ExceptionState` in the same file declares a closed list of values, and how a model field typed with it rejects anything else.

</details>

<details><summary>Move 4</summary>

The approach: declare the four kinds as a closed enumeration, spelled exactly as the lesson spells them, and type `kind` with it. Then a value outside the list fails validation, and a plain string that matches one of the four is still accepted.

</details>

### The `tool_input` rule only half works

<details><summary>Move 1</summary>

Write down the four cases: a tool call with input, a tool call without, another kind with input, another kind without. Which two must fail?

</details>

<details><summary>Move 2</summary>

Can a check that looks at one field see another field? The rule needs `kind` and `tool_input` at the same time.

</details>

<details><summary>Move 3</summary>

Find how `SensorReading` rejects a bad pair of values. When does that check run, and what does it raise?

</details>

<details><summary>Move 4</summary>

Did you decide whether an empty `{}` counts as an input? Check that your rule treats `{}` the way you decided, both for a tool call and for the other kinds.

</details>

<details><summary>Move 5</summary>

The approach: one check on the whole model that runs after every field is set. It reads the kind first: for a tool call it requires an input, and for any other kind it requires that there is none. Raise a clear error in each failing branch.

</details>

### A count of `"7"` or `true` is accepted

<details><summary>Move 1</summary>

Is that what you decided? Both answers are allowed by the lesson, but you have to choose one and say why.

</details>

<details><summary>Move 2</summary>

A model field typed as a whole number converts some values by default. Try `"7"`, `7.0` and `true` in the shell and note what comes back for each.

</details>

<details><summary>Move 3</summary>

If you decided a count must already be a whole number, find how your validation library lets one field refuse to convert. Then add a test for each value you now reject.

</details>

### You can't decide whether an empty `reason` is allowed

<details><summary>Move 1</summary>

Who reads the reason? Picture the dispatcher opening a run whose proposal has no reason at all.

</details>

<details><summary>Move 2</summary>

The lesson leaves this to you and asks you not to restrict the reason any further than that. What does each choice cost?

</details>

<details><summary>Move 3</summary>

Choose, enforce it with a field rule, write a test for it, and put the reason for your choice in **What I'd defend**.

</details>

### You can't decide what the request carries

<details><summary>Move 1</summary>

Picture whatever will answer the request: it holds several lists of fixed answers. What does it need to know to return exactly one entry?

</details>

<details><summary>Move 2</summary>

Should the answerer remember how many times it was asked, or should the request say which entry it wants? Which choice gives the same answer to the same request every time?

</details>

<details><summary>Move 3</summary>

For each field you're considering, finish the sentence "the answerer needs this because...". A field with no ending to that sentence doesn't belong, unless you can say why the agent wants it there, such as which exception the request is about.

</details>

<details><summary>Move 4</summary>

The approach: the request names the list of answers and the position of the entry it wants, and the position counts from a base you decide and write down. Choose field names you're happy to keep: from the next Task on, the emulator reads them.

</details>

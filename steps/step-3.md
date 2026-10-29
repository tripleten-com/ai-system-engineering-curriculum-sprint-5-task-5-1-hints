# Step 3: Write the action contract

## You're not sure where the contract goes

<details><summary>Hint 1</summary>

What's in `src/domain/contracts.py` today? Read it before you write anything.

</details>

<details><summary>Hint 2</summary>

The supplied code and tests import the platform's contracts from that file. Whatever you add, every name that was there must still be there afterwards. Adding to the file and adding a module beside it both work.

</details>

## A fifth kind can still be created

<details><summary>Hint 1</summary>

What type does `kind` have? If it's a string, what stops someone from writing a string that isn't one of the four?

</details>

<details><summary>Hint 2</summary>

Look at how `ExceptionState` in the same file declares a closed list of values. Would the same construct work for the four kinds?

</details>

<details><summary>Hint 3</summary>

Try to build a proposal with a misspelled kind in the Python shell. If it constructs, the field accepts more than the list.

</details>

## The `tool_input` rule only half works

<details><summary>Hint 1</summary>

The rule needs two fields at once: `kind` and `tool_input`. Can a check on a single field see the other one?

</details>

<details><summary>Hint 2</summary>

Find how `SensorReading` rejects a bad pair of values. When does that check run, and what does it raise?

</details>

<details><summary>Hint 3</summary>

Write down the four cases: a tool call with input, a tool call without, another kind with input, another kind without. Which two must be rejected? Does your check treat an empty `{}` the way you decided?

</details>

## A count of `"7"` or `true` is accepted

<details><summary>Hint 1</summary>

Did you decide that's acceptable? If you did, write the reason down. If not, read on.

</details>

<details><summary>Hint 2</summary>

A model field typed `int` converts some values by default. How does the validation library you're using let one field refuse to convert?

</details>

## You can't decide what the request carries

<details><summary>Hint 1</summary>

Picture the thing that answers a request: it holds several lists of fixed answers. What two facts does it need to return exactly one entry?

</details>

<details><summary>Hint 2</summary>

Should the answerer remember how many times it has been asked, or should the request say which entry it wants? Which choice gives the same answer for the same request every time?

</details>

<details><summary>Hint 3</summary>

For every field you add, finish the sentence "the answerer needs this because...". A field with no ending to that sentence doesn't belong. Then decide whether the entry position counts from 0 or from 1, and write it down.

</details>

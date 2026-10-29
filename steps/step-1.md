# Step 1: Create the exception the agent will work on

## The exception ends in `COMPLETED`, not `NEEDS_REVIEW`

<details><summary>Hint 1</summary>

Which answer did you ask the emulator for? Read the line where the command printed the `state`, then the options `poe scenario` accepts.

</details>

<details><summary>Hint 2</summary>

Each run creates a new exception. If you ran the command twice, which id belongs to the run that ended in `NEEDS_REVIEW`?

</details>

## You can't find where the answer was rejected

<details><summary>Hint 1</summary>

What does a trace record, and what does an audit trail record? One shows the path a request took through the services. The other shows the decisions Coldline stored about the exception.

</details>

<details><summary>Hint 2</summary>

Search the trace for a span that carries the guardrail's verdict. If there isn't one, where else could a refused answer leave a trace? Run `poe audit-trail` with your exception id and find the event that carries a `reason_code`.

</details>

## The audit trail has more events than you expected

<details><summary>Hint 1</summary>

Which events did the worker write, and which ones came from someone reading the exception afterwards? Compare each event's trace id with your `api_trace_id`.

</details>

<details><summary>Hint 2</summary>

Each read of the stored outcome adds one event. How many times has the exception been read since the worker finished, counting the read `poe scenario` makes itself?

</details>

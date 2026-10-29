# Step 1: Create the exception the agent will work on

## The idea

Every exception Coldline handles leaves two kinds of record, and they answer different questions.

- **A trace:** answers "which path did this request take?". It's a timeline of spans: the API receives the reading, hands it to the worker, the worker asks the model, and so on. Jaeger shows it. A trace is about timing and flow. It tells you that the model was called, not what Coldline decided about the answer.
- **An audit trail:** answers "what did Coldline decide, and in what order?". It's a list of events stored with the exception itself: the worker picked it up, the model answered, the guardrail rejected the answer, the outcome was stored. It's the durable record a dispatcher, or later your instructor, reads when they ask why a summary was refused.

`NEEDS_REVIEW` is the state where those two meet the dispatcher. The worker is finished with the exception, but a person still has to decide what happens to the shipment. Your agent will only ever work on exceptions in this state, so the first thing you need is one, plus the three facts that prove how it got there: its id, its trace id and its audit events in order.

> Covered in **Observability First Principles**.

## What's expected

- `poe ready` reports the services ready, and `poe ingest` finishes without an error.
- `poe scenario --response malformed` prints an `exception_id`, a `state` of `NEEDS_REVIEW` and an `api_trace_id`.
- In Jaeger, searching for that trace id shows one trace with spans from both the API and the worker, ending with the worker's last write.
- `poe audit-trail` with your exception id lists `processing_requested`, `model_responded`, `output_rejected` (with a `reason_code`) and `outcome_stored`, in that order, then one or more `summary_read`.
- Your notes hold the exception id, the trace id and the event order, ready for the evidence note.

## When you're stuck

### The stack won't start, or `poe ready` keeps waiting

<details><summary>Hint 1</summary>

Which service is it waiting for? Read the last lines `poe ready` printed before it stopped or timed out.

</details>

<details><summary>Hint 2</summary>

Run `docker compose ps` and look for a service that isn't healthy or has exited. That service's log usually says why. Read it with `docker compose logs` and the service's name, or with `poe runtime-logs`.

</details>

<details><summary>Hint 3</summary>

Is another stack already using one of the ports Coldline needs? Check whether something else on your machine is running a database, a queue emulator or a web server on the same port.

</details>

<details><summary>Hint 4</summary>

Follow **Start the system** in `README.md` from the top, in order, on a clean start. A step skipped earlier, such as the tooling bootstrap, often shows up later as a service that never becomes healthy.

</details>

### The exception ends in `COMPLETED`, not `NEEDS_REVIEW`

<details><summary>Hint 1</summary>

Which answer did you ask the emulator for? Read the command you ran and the `state` it printed.

</details>

<details><summary>Hint 2</summary>

Look at the options `poe scenario` accepts. Which response makes the model's answer stop partway, so the guardrail can't accept it?

</details>

<details><summary>Hint 3</summary>

Each run creates a new exception. If you ran the command more than once, check that the id you kept belongs to the run that ended in `NEEDS_REVIEW`.

</details>

<details><summary>Hint 4</summary>

Run the scenario with the malformed response, keep the new id, and use only that id for the rest of the Step.

</details>

### You can't find the request in the trace viewer

<details><summary>Hint 1</summary>

Which id are you searching for? The command prints a trace id, not just an exception id.

</details>

<details><summary>Hint 2</summary>

Open Jaeger at the URL in `README.md`, choose the API service, and search by the trace id. Does the time range include the moment you ran the scenario?

</details>

<details><summary>Hint 3</summary>

The worker continues the trace the API started. If you see only the API's spans, wait a few seconds and search again: the worker's spans arrive when it finishes.

</details>

### You can't find where the answer was rejected

<details><summary>Hint 1</summary>

Where are you looking: in the trace, or in the audit trail? Which of the two records decisions?

</details>

<details><summary>Hint 2</summary>

Search the trace for a span that carries the guardrail's verdict. If none does, the trace isn't where that fact lives.

</details>

<details><summary>Hint 3</summary>

Run `poe audit-trail` with your exception id and find the event that carries a `reason_code`. That's the refusal, and the record stores the same code as its rejection reason.

</details>

### The audit trail has more events than you expected

<details><summary>Hint 1</summary>

Which events did the worker write, and which ones came from someone reading the exception afterwards?

</details>

<details><summary>Hint 2</summary>

Compare each event's trace id with your `api_trace_id`. Events from later reads carry the trace id of their own request.

</details>

<details><summary>Hint 3</summary>

Count the reads since the worker finished, including the one `poe scenario` makes itself. Each read of the stored outcome adds one event at the end of the trail. Keep the worker's four events in order for your note, and mention the reads after them.

</details>

# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Did you meet the Task's requirements?

- Can a fifth kind be created by any route, including a plain string or a misspelling?
- Does a tool call without input fail on construction, and does any other kind with input fail too?
- Does your request carry exactly what an answerer needs to find the list and the entry, and nothing it can't explain?
- Do `poe contract` and `poe auth-checks` still show what Step 2 showed?

## Did you explain your design choices?

- Does your note say whether an empty `reason` is allowed, and why?
- Does it say what a count of `"7"` or `true` does, and why you chose that?
- Does it say whether `{}` counts as a tool input, and whether the entry position counts from 0 or from 1?

## Can your tests fail when the code is wrong?

- For each rule, did you break the code once and see the matching test fail?
- Does your note say what you broke and which test failed?

## Did you state the limits of what your checks show?

- Does your note say what the model port's fidelity record says the emulator can't prove?
- Does it keep apart what you saw locally and what a real model or real AWS service would do?

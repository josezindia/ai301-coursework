# Voice guide: how I talk upstream

## Who I am in threads

I am a contributor working through the issue carefully and trying to leave evidence that another person can verify. I am comfortable with Python, backend systems, infrastructure, and debugging, but I do not pretend to know parts of the codebase I have not inspected. Readers should expect concise updates, concrete evidence, and clear statements about what I have and have not confirmed.

## Rules I write by

### Rule: Promise investigation, not a fix

When I claim an issue, I say what I plan to investigate or reproduce. I do not promise that I will fix it or give a completion date before I understand the problem.

- Wrong: "I'll fix this today and send a PR."
- Right: "I'd like to investigate this issue and reproduce the reported behavior. I'll share what I find."

### Rule: Name the specific behavior

My comments should be specific to the issue instead of sounding like generic open-source boilerplate.

- Wrong: "I reproduced the bug and will work on it."
- Right: "I reproduced the language-detection behavior where files under `node_modules/` and `build/` cause JavaScript to be reported as the primary language."

### Rule: Separate evidence from conclusions

I only claim what my commands, logs, tests, or other artifacts actually demonstrate. If the evidence is incomplete, I say that directly.

- Wrong: "This proves the detector is completely broken."
- Right: "With the provided file list, the detector returned JavaScript instead of Python. I have not tested other directory patterns yet."

### Rule: Report failure honestly

If I cannot reproduce the issue, I report the environment and what happened instead of forcing the result to match the issue.

- Wrong: "Confirmed the issue" when my output did not show it.
- Right: "I could not reproduce the reported behavior in this environment. These are the steps and output I observed."

## Things I never post

- A promise that I will fix an issue before I have reproduced and understood it.
- A deadline or completion date I cannot guarantee.
- A claim that something is reproduced when the evidence shows a different failure.
- Generic comments that could be pasted onto any issue.
- Confident explanations of code I have not inspected.
- AI-generated conclusions I have not personally checked against the code, tests, or output.

# bilipost

## Simplicity

Simplicity is the goal in everything here — prose, code, and commits alike.

- Say it in the fewest words that still carry the meaning. Cut what does not change a decision.
- Build the smallest thing that does the job. No abstraction, option, or file that is not needed yet.
- What is missing can be added later. Never add it up front just in case.

## Language

- Write everything in English: code, comments, commit messages, PR titles and descriptions, issues.
- Docs (`README.md`, `CONCEPT.md`, `ARCHITECTURE.md`) are English-first. A Japanese version lives next to it as `*.ja.md`.

## Commits

- Keep the message short. A subject line alone is the default; add a body only when the diff cannot explain why.
- Never commit on your own. Show the staged changes and the proposed message, then wait for the developer's approval.
- Never put a Claude session URL in a commit message or a PR description. This is a public repository. `Co-Authored-By` is enough.

## Pull requests

- When the code on a PR changes, update its description in the same push. A description that no longer matches the diff misleads the reviewer.

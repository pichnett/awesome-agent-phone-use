# Contributing

Thanks for helping keep this list useful. The bar is simple: a builder should be able to trust the link and the one-line description.

## What belongs here

Tools, papers, benchmarks, device clouds, emulators, and hardware that help an AI agent operate a phone or a mobile app. Official app APIs (App Intents, AppFunctions) belong too: they are a way for an agent to act without driving the GUI.

Out of scope:

- Desktop-only computer-use agents, unless the work clearly covers phones too
- Generic LLM or agent lists with no mobile-operation angle
- In-app test frameworks that cannot drive an arbitrary app
- Marketing pages with no technical description
- Tools whose main pitch is account farming, device fingerprinting, evading platform detection, or "undetectable" posting

This is a curation, not a catalogue. Prefer a strong, maintained project over a complete vendor parade.

## Rules for a new entry

1. Use this format, with a hyphen (not an em dash), an uppercase description, and a closing period:

   `- [Name](url) - Short factual description.`

   A paper or docs link may follow inside the same sentence, still ending with a period.

2. The primary link must resolve. No 404s, no placeholder repos, no "code coming soon" as the only URL. A paper-only entry is fine if there is no public code. Just say so.

3. Do not reuse a URL that already appears in the README. Lint treats duplicate links as an error. Mention a related project by name, or improve the existing entry.

4. Put the entry in the right section. If nothing fits, propose a section in the PR.

5. Describe the mechanism, not the slogan. Prefer "drives a real Android device over adb" or "mechanical robot that taps a physical screen for QA."

6. Do not invent results. No unverified speed, accuracy, reliability, compatibility, or fleet-size claims. Do not present a plan as a shipping product. If something is still being built, say that, and include the public date if there is one.

7. Do not repeat the project name in the description.

8. Use the project's current name. If it was renamed, mention the old name once.

9. No drive-by promo. PRs that only add a tool you sell, with a vague or inflated description, will be closed. First-party additions are welcome when they follow the rules above, including the disclosure already in the README.

## Pull requests

- One entry (or one tight group of related entries) per PR is easier to review.
- Edit `README.md` on a branch and open a PR against `main`.
- In the PR body, note the section, why the entry is in scope, and that you checked the links.

## Corrections

Broken links, renamed projects, archived repos, and over-claims should be fixed in place. If a project's official code has moved, point at the maintained repo and keep the paper link. Unmaintained items do not belong in the main README; park them in a separate file if they are historically important.

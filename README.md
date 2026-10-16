# AI Code Reviewer

An AI agent that reviews pull requests with Claude and posts its findings as a comment on GitHub. It runs automatically as a GitHub Action.

## What it does

1. A pull request is opened or updated.
2. A GitHub Action fetches the diff.
3. Claude checks it against rules in `REVIEW_RULES.md`.
4. The agent posts one review comment, and updates it on later commits instead of adding duplicates.

## Safety limits

- Skips diffs larger than 60,000 characters, so cost stays bounded.
- Treats the diff as untrusted input, and the prompt tells Claude not to follow instructions inside it.
- Only comments. It never approves or merges anything.
- Updates its own comment instead of posting duplicates.

## How to set it up

1. Add `ANTHROPIC_API_KEY` as a repository secret in the repo you want reviewed.
2. Add a workflow file at `.github/workflows/review.yml` that checks out this repo and runs `post_review.py`.
3. Edit `REVIEW_RULES.md` to change what it looks for.

## Results

Tested on 3 pull requests with planted bugs. It caught hardcoded credentials, unsafe SQL, and missing error handling, and it also found a real authentication bug I had not planted.

## Limitations

- Reviews only the diff, not the whole codebase.
- Wording of comments changes between runs.
- Not tested on large pull requests or on forks. Fork pull requests don't receive repository secrets.

## Built with

Python, the Anthropic API, GitHub Actions and the GitHub REST API.

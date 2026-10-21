# Commit messages & PR descriptions

- Be as short as possible. No verbose language. After writing, recheck and remove anything that can be removed before finalizing.
- Never describe what was done (that is visible in the code). Only describe the why and the impact: which bug is fixed or which feature is added.
- Only mention things affecting the outside of the service (API endpoints, events, etc.) or significant general logic changes. Never list changed classes/files/functions.
- Example: "Fix credit note refunds always failing."

# Pull requests

- "Assign a PR to someone" means request them as reviewer. Never use the PR assignee functionality.
- After committing and pushing, check for an open PR on the branch and update its description if needed.

# New projects

When you work on a project, depending on the language of the files you are modifying (golang, ts etc) use the corresponding rules: 

https://raw.githubusercontent.com/neuro-flash/ai-guidelines/main/go.md for go files
https://raw.githubusercontent.com/neuro-flash/ai-guidelines/main/typescript.md for ts files 


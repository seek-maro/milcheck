
## Introduction

Code reviews are essential, but let's be honest, they can feel like endless Macrodata Refinement. At Maro, we leverage AI tools to accelerate development, improve quality, and maybe even earn a Waffle Party. This project brings Claude Code's powerful reasoning capabilities directly into your GitHub workflow as an automated reviewer – think of it as our own floor supervisor, 'milcheck'. (Yes, it's a pun on *that* Milchick, supervising our code refinement instead of... well, you know.)

Unlike other AI code review solutions that might require you to sever your ties with existing platforms, this integration:
- Is fully customizable to your team's needs (no mysterious Lumon Industries mandates here)
- Integrates with your existing ticketing system (in our case, Shortcut)
- Runs as a GitHub Action
- Provides context-aware reviews that understand requirements, not just code (hopefully avoiding any trips to the Break Room)

## How It Works

The system consists of two GitHub Action workflows:

1. **Central workflow (`claude-code-review.yml`)**: Located in this global `.github` repository, handles the review process
2. **Trigger workflow (`milcheck-reviewer.yml`)**: Added to each project repository, activates on PR events

The workflow is triggered when:
- A new PR is opened or reopened
- Someone comments `@milcheck review` on a PR

When triggered, the system:
1. Collects PR information (title, description, files changed)
2. Generates a diff of all code changes
3. Uses Claude Code to analyze the changes
4. Fetches related Shortcut ticket information via MCP integration (Note: you'll want to replace this with your own ticketing system and tools)
5. Posts a structured review comment on the PR

## Setup Instructions

### Prerequisites

1. An Anthropic API key for Claude
2. A Shortcut API token (if using Shortcut integration) - although you'll probably want to edit the `claude-code-review.yml` prompt and claude code permissions to use your tools.
3. GitHub repository with Actions enabled

### Installation

1. **Create a `.github` repo that contains your claude-code-review.yml workflow**
   Add the file `.github/workflows/claude-code-review.yml` to your global repo.
   This organization-wide repo will allow this workflow to run globally across all project repositories. You can just have this in your local repo if you are enabling for one repository.

2.  **Add the trigger workflow to your project repository**:
   Create a file at `.github/workflows/milcheck-reviewer.yml` in your repository with the content from this repo's trigger workflow.

2. **Configure secrets in your repository**:
   - `ANTHROPIC_API_KEY`: Your Anthropic API key
   - `SHORTCUT_API_TOKEN`: Your Shortcut API token
   - `GITHUB_TOKEN`: Automatically provided by GitHub Actions

3. **Open a PR and see the magic happen!**

## Customization

### Milchick Voice Mode (Optional)

Want your code reviews delivered with the distinctive tone of Lumon's favorite floor supervisor? You can enable Milchick Voice Mode!

1.  **Central Workflow Input:** The `claude-code-review.yml` workflow accepts a boolean input: `use-milchick-voice` (defaults to `false`).
2.  **Triggering the Voice:** To activate this mode for a specific review, include the phrase `, praise Kier!` after your review request comment. For example: `@milcheck review, praise Kier!`
3.  **Trigger Workflow Logic:** The trigger workflow (`.github/workflows/milcheck-reviewer.yml`) in this repository has been updated to detect this phrase and automatically set the `use-milchick-voice` input to `true` when calling the central workflow. If you are using a custom trigger workflow in your own repository, you will need to add similar logic:
    *   Check if the comment body contains `@milcheck review, praise Kier!`.
    *   If it does, pass `use-milchick-voice: true` to the `claude-code-review.yml` workflow.

## Benefits

- **Instant feedback**: Developers get immediate, thoughtful reviews without waiting for human reviewers
- **Context-aware reviews**: Claude understands not just the code but the requirements behind it
- **Consistent quality**: Every PR gets the same level of attention, regardless of size or timing
- **Cost-effective**: No expensive SaaS subscriptions – pay only for the Claude API usage you need
- **Fully customizable**: Tailor the review focus to your team's specific needs
- **Works with your tools**: Integrates with your existing GitHub and ticketing workflows
- **Increased Efficiency**: Spend less time waiting for reviews and more time... well, refining data. Maybe even qualify for a Finger Trap reward?

## Humans Still in the Loop

This system doesn't replace human code reviews. The human element of knowledge sharing, mentoring, and collaboration remains incredibly valuable. Claude provides another set of eyes that can catch issues early and ensure code changes align with requirements.

## License

[GNU Affero General Public License v3.0](https://www.gnu.org/licenses/agpl-3.0.en.html)

## Disclaimer

Note: The project naming is inspired by the TV show *Severance* and uses thematic elements for illustrative and entertainment purposes. It is an independent creation and is not affiliated with, endorsed by, or connected to Apple TV+, Red Hour Productions, or any creators or entities associated with *Severance*. All trademarks and copyrights related to *Severance* belong to their respective owners. Praise Kier!

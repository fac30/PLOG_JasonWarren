# Log Generation Prompts

## General Notes

- Make sure the AI prompt file is not displayed in the editor when sending messages.

## Prompt Sequence

### Establish Conversation Context

- **Prompt:** [Confirm the AI's Ruleset](./confirmRuleset.md)
- ~~Add all relevant project folders to the IDE workspace~~
- Add ~~README for~~ each project folder to the conversation
- **Prompt:** [Check the AI Can Read All Files](./checkFiles.md)

### Git History Analysis

- Feed Logs to Cursor:
  - Open the Cursor terminal
  - Run `git --no-pager log` on each folder
  - Highlight the log and add them to the conversation
- **Prompt:** [Analyse the git log](./analyseGit.md)

### Progress Log Generation

- **Prompt:** [Check the AI will use the correct "voice"](./checkVoice.md)
- **Prompt:** [Generate a progress log](./generateLog.md)
- **Prompt:** [Ask the AI to Evaluate the generated log then try again](./evaluateAILog.md)
- **Prompt:** [Reinforce the authorial voice](./reinforceVoice.md)
- Correct any mistake

### Progress Evaluation

- Open a new chat
- **Prompt:** [Confirm the AI's Ruleset](./confirmRuleset.md)
- **Prompt:** [AI Progress Evaluation](./evaluateJasonLog.md)

### README Update

- Open a new chat
- **Prompt:** [Update the README's TOC](./readmeAdd.md)

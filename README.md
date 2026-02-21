# gpt

This Bash script provides a CLI interface to OpenAI/compatible models for interactive conversations. It maintains a **JSONL context file** to store the conversation history, allowing the assistant to “remember” previous messages and user tasks.

* Structured role-based memory (`system`, `user`, `assistant`)
* Optionally persistent context across terminal sessions
* Configurable prompts and models
* Plain-text output while storing JSONL

## Requirements

- Bash 4+  
- curl  
- jq  

You also need an OpenAI or OpenRouter API key.

---

## Installation

1. Copy the `gpt` script to a directory in your `$PATH`.  
2. Make it executable:

```bash
chmod +x gpt
```

3. Export your API key in your shell configuration (`.bashrc` or `.zshrc`):

```bash
export GPT_KEY="your-api-key"
```

> Alternatively, you can define the key directly in the script, but using environment variables is safer.

---

## Configuration

The script is highly configurable via environment variables:

```bash
GPT_KEY="${GPT_KEY:?Please define your GPT key}"
GPT_MODEL="${GPT_MODEL:-openai/gpt-4}"        # Default model
GPT_CONTEXT="${GPT_CONTEXT:-/tmp/gpt.$WINDOWID}" # JSONL context file
GPT_PROMPT="${GPT_PROMPT:-/tmp/gpt.$WINDOWID.prompt}" # System prompt
GPT_TOKENS="${GPT_TOKENS:-512}"               # Max token slice for context
GPT_TEMPERATURE="${GPT_TEMPERATURE:-0.7}"     # Response creativity
GPT_MAX_LINES="${GPT_MAX_LINES:-40}"          # Max messages to keep in context
GPT_MAX_TOKENS="${GPT_MAX_TOKENS:-400}"       # Max tokens for the model response
```

> `$WINDOWID` is unique for each terminal session, allowing separate contexts for multiple terminals.

You can also create aliases for different tasks:

```bash
alias programmer-gpt='GPT_PROMPT="$HOME/programmer.prompt" gpt'
alias git-gpt='GPT_PROMPT="$HOME/git.prompt" gpt'
```

## Usage

* Open the context for editing:

```bash
gpt
```

* Send arguments as input:

```bash
gpt Hello assistant
```

* Use stdin for input:

```bash
echo "Hello assistant" | gpt
```

* Combine stdin + arguments:

```bash
echo "Please summarize this" | gpt Detailed summary
```

## Behavior

* User messages (from stdin or arguments) are stored as JSON objects with `"role":"user"`.
* Assistant responses are stored as JSON objects with `"role":"assistant"`.
* Both are appended to the **JSONL context file** (`GPT_CONTEXT`).
* Only the **last `$GPT_MAX_LINES` messages** are sent to the model to prevent exceeding context limits.
* The system prompt is always sent as a `"role":"system"` message at the start of each request.

## Notes

* The script works with both OpenAI API and OpenRouter API endpoints.
* Using JSONL for context allows you to have persistent chat memory, turn-taking, and proper role separation.

## Example

```bash
# Add a task
gpt "Remind me to buy thread for sewing project"

# Check your tasks
gpt "What are my tasks today?"

# Pipe stdin
echo "Add Minetest project tasks" | gpt
```


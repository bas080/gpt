`$ cat ./README.md ./gpt | gpt Please update the README > README.md`

> Just kidding, I wrote the readme... or did I?

# $ gpt

This Bash script uses the OpenAI API to generate a response to a user-specified
query using the GPT-3 language model. The response is appended to a specified
context file, which contains the history of messages and responses.

## Requirements

To use this script, you will need:

- Bash (version 4 or later)
- curl
- jq
- pandoc (optional pretty printing to terminal)
- elinks (optional retty printing to terminal)

You will also need an OpenAI API key, which can be obtained from the [OpenAI
website](https://beta.openai.com/signup/).

## Installation


1. Copy paste the script in a `$PATH` directory.
2. Make the script executable with `chmod +x gpt`.
3. Add `export OPENAI_KEY="your-api-key"` to your .bashrc.

You can now run the script.

> Note: If you do not wish to export your OpenAI API key, you can include it
> directly in the script by replacing `$OPENAI_KEY` with your actual API key.

## Usage

To run the script, use the following command:

```
gpt                   # Opens the context file for editing.
gpt Hellp gpt         # Uses the arguments as input.
stdin | gpt           # Uses the stdin as user input.
stdin | gpt Hello gpt # Uses stdin + arguments as input.
```

It's important to know that you can configure gpt using the environment
variables. I create gpt aliases for different tasks. Here is a list of those
variables and their default values.

```bash
OPENAI_KEY="${OPENAI_KEY:?OPENAI_KEY is not set}"
OPENAI_MODEL="${OPENAI_MODEL:-gpt-3.5-turbo}"
OPENAI_CONTEXT="${OPENAI_CONTEXT:-/tmp/gpt.$WINDOWID}"
OPENAI_PROMPT="${OPENAI_PROMPT:-/tmp/gpt.$WINDOWID.prompt}"
OPENAI_TOKENS="${OPENAI_TOKENS:-4096}"
```

> `$WINDOWID` is unique for every terminal you open. This allows you to spawn
> a new terminal and start with a clean slate.


```bash
alias programmer-gpt='OPENAI_PROMPT="$HOME/programmer.prompt" gpt'
alias git-gpt='OPENAI_PROMPT="$HOME/git.prompt" gpt'
```

# AI Agent - A Boot.dev Learning Project

This project was built as part of the **Boot.dev “Build an AI Agent” course** to learn how an AI coding agent works from the ground up. It connects a language model to Python tools so the agent can inspect files, make changes, run code, and use the results to work toward a user's request.

The goal is to help you understand how to build your own AI agent and adapt it to your use case. The target project and tasks are configurable: choose a working directory, adjust the instructions, and extend the tools to support the workflow you want.

## What you'll learn

- Connect to a language model through an OpenAI-compatible API.
- Describe Python functions as tools the model can call.
- Dispatch tool calls and return their results to the conversation.
- Build an agent loop that continues until the model provides a final response or reaches an iteration limit.
- Configure the agent's working directory, instructions, and available operations.

## Features

The agent currently supports four operations:

| Tool               | Purpose                                        |
| ------------------ | ---------------------------------------------- |
| `get_files_info`   | List files and directories.                    |
| `get_file_content` | Read a file's contents.                        |
| `write_file`       | Create or overwrite a file.                    |
| `run_python_file`  | Execute a Python file with optional arguments. |

It uses the OpenAI Python SDK with OpenRouter and loads its API key from the environment. Verbose mode shows token usage, tool arguments, and tool results.

## Getting started

### Requirements

- Python 3.12 or newer
- `uv`
- An OpenRouter API key

### Setup

From the repository root, install the dependencies:

```bash
uv sync
```

Create a `.env` file in the repository root:

```dotenv
OPENROUTER_API_KEY=your_api_key_here
```

Keep your API key private and exclude `.env` from version control.

### Choose your target project

Before running the agent, set `WORKING_DIR` in `config.py` to the directory you want it to work with:

```python
MAX_CHARS = 10000
WORKING_DIR = "./your-project"
MAX_ITERS = 20
```

Create the target directory if it does not already exist. Relative working-directory paths are resolved from where you launch the agent, so run the commands below from the repository root.

### Run the agent

Pass your request as a quoted command-line argument:

```bash
uv run main.py "Explain how this project is organized"
```

For more detailed output:

```bash
uv run main.py "Inspect the Python files and explain how they work" --verbose
```

Choose prompts that match the files and goals in your configured working directory.

## How it works

1. The application loads the API key, system instructions, and user request.
2. It sends the conversation and available tool definitions to the model.
3. When the model requests a tool, the application executes the corresponding Python function using the configured working directory.
4. Tool results are added to the conversation so the model can decide what to do next.
5. The loop ends when the model returns a final response or the configured iteration limit is reached.

## Adapt it to your use case

- **Target directory:** Change `WORKING_DIR` in `config.py`.
- **Agent behavior:** Edit the system instructions in `prompts.py`.
- **Model:** Change the model identifier in `main.py`. The current implementation uses `openrouter/free`.
- **Tools:** Implement functions and their schemas in `functions/`, then register them in `call_function.py`.
- **Limits:** Adjust `MAX_CHARS` and `MAX_ITERS` in `config.py`.

## Core files

```text
main.py           Command-line entry point and agent loop
prompts.py        System instructions
config.py         Working directory and limits
call_function.py  Tool registration and dispatch
functions/        File inspection, file writing, and Python execution tools
pyproject.toml    Python requirements and dependencies
uv.lock           Locked dependency versions
```

## Learning project scope

This is an educational project built while following Boot.dev, intended for experimentation and learning. The agent can overwrite files and execute Python code, so use a disposable project or keep your work under version control. Working-directory checks are not a complete sandbox for executed code.

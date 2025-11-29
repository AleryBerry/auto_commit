# auto_commit

`auto_commit` is a command-line interface (CLI) tool written in Go that automates the process of generating Git commit messages and pushing changes to your remote repository. It leverages Large Language Models (LLMs) via the OpenRouter API to craft descriptive commit titles and bodies based on your staged Git changes.

## How It Works

1.  **Git Repository Check**: Verifies if the current directory is a Git repository.
2.  **Diff Analysis**: Retrieves the `git status --short` and `git diff` for your currently staged changes. If there are no staged changes, the tool exits.
3.  **API Key Retrieval**: Obtains your OpenRouter API key from either the `-k` command-line flag or the `OPENROUTER_API_KEY` environment variable.
4.  **LLM Model Selection**: Uses the specified LLM model (defaulting to `tngtech/deepseek-r1t2-chimera:free`) for message generation.
5.  **Commit Message Generation**: Sends the git diff to the OpenRouter API, requesting a structured JSON response containing a commit title and description.
6.  **Git Operations**:
    *   Executes `git add .` to stage all current changes.
    *   Executes `git commit -m "Title" -m "Description"` using the generated message.
    *   Executes `git push` to push the changes to your remote repository.

## Installation

To install `auto_commit`, you need to have Go installed on your system.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/aleryberry/auto_commit.git 
    cd auto_commit
    ```
2.  **Build the executable:**
    ```bash
    go build -o auto_commit
    ```
3.  **Move to your PATH (optional):**
    ```bash
    sudo mv auto_commit /usr/local/bin/ (or rather ~/.local/bin/)
    ```

## Usage

Before running `auto_commit`, ensure you have an OpenRouter API Key. You can obtain one from [OpenRouter.ai](https://openrouter.ai/).

Set your API key as an environment variable (recommended):

```bash
export OPENROUTER_API_KEY="sk-YOUR_OPENROUTER_API_KEY"
```

Alternatively, you can provide it directly using the `-k` flag:

```bash
auto_commit -k "sk-YOUR_OPENROUTER_API_KEY"
```

Once your API key is set, navigate to your Git repository with staged changes and simply run:

```bash
auto_commit
```

The tool will analyze your staged changes, generate a commit message, commit them, and push to the remote.

### Command-line Flags

*   `-k <API_KEY>`: (Optional) Your OpenRouter API Key. This flag will override the `OPENROUTER_API_KEY` environment variable if both are present.
*   `-m <MODEL_NAME>`: (Optional) The OpenRouter model to use for generating the commit message.
    *   Default: `tngtech/deepseek-r1t2-chimera:free`
    *   Example: `auto_commit -m "mistralai/mistral-7b-instruct:free"`

### Important Note on `git add .`

Currently, `auto_commit` runs `git add .` which stages *all* modified, new, and deleted files in your working directory before committing. If you prefer to commit only a subset of your changes, you should stage them manually using `git add <files...>` *before* running `auto_commit`.

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

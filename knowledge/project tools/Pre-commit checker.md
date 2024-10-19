To maintain code quality and consistency, we've set up a `pre-commit` hook that runs a series of checks before any commit is finalized. This process helps catch common issues like syntax errors, formatting problems, and missing tests before they make it into the main codebase.

## What It Does

The `pre-commit.py` script is designed to automatically handle a variety of tasks before your code is committed, including:

- **Running Tests:** Automatically runs `pytest` to ensure all tests pass.
- **Formatting Code:** Uses `Ruff` to auto-format Python files.
- **Linting Markdown:** Checks Markdown files against our style guide.
- **Ensuring Code Consistency:** Makes sure Python files include future annotations and that all files end with a newline.
- **Adding Coverage Reports:** Automatically stages test coverage reports to keep track of what’s being tested.
- **Detecting Debug Methods:** Ensures that no debug code (like `print` statements) is accidentally committed.

If any of these checks fail, the commit will be blocked, giving you a chance to fix the issues before trying again.

## How It Works

1. **Git Hook Forwarding:** The `.git/hooks/pre-commit` file in your repository acts as a trigger. It forwards the call to a script located at `hooks/pre-commit.sh`.
2. **Script Execution:** The `hooks/pre-commit.sh` script then forwards the call to `pre-commit.py`, which performs all the necessary checks and tasks.

This setup keeps the `.git/hooks/` directory clean and allows for easier updates to the hook logic by simply modifying the `hooks/pre-commit.sh` script.

## Configuration Options

You can customize how the `pre-commit.py` script runs by passing different options:

- `--no-tqdm`: Disables the progress bar.
- `--ascii`: Uses ASCII-only characters (useful if you're working in a terminal that doesn't support Unicode).
- `--silent`: Runs the script without any output, which is useful if you're automating this process or prefer a quieter workflow.

## Where It's Located

- **`pre-commit.py`:** Located in the root directory of the repository.
- **`pre-commit.sh`:** Located in the `hooks/` directory within the project.

These scripts work together to ensure your code meets the project's standards before being committed.

## Getting Started

To start contributing:

1. Make your changes as usual.
2. When you're ready to commit, just use `git commit`.
3. The `pre-commit` hook will trigger, forwarding the call from `.git/hooks/pre-commit` to `hooks/pre-commit.sh`, which then runs the `pre-commit.py` script.
4. The script will automatically run, checking your code and formatting it as needed.

If all checks pass, your commit will go through. If not, fix any issues the script reports and try again.

## Bypassing
To bypass the pre-commit checks (though it's generally not recommended), you can add the `--no-verify` flag to your commit command like this:

```git commit --no-verify```

This will allow you to commit your changes without running the `pre-commit` hook. Use this option sparingly, as skipping the checks can introduce issues into the codebase. But sometimes, if you know what you’re doing, it can be handy for quick, experimental commits.
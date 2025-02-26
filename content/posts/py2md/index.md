---
title: "How I Write Articles Using Plain Python Files"
summary: "Turn your Python code into Markdown with ease"
showSummary: true
categories: ["Post"]
tags:
  [
    "tools",
    "documentation",
    "blog",
    "writing",
    "code",
    "markdown",
    "neovim",
  ]
date: 2025-02-26
draft: false
---

We’ve all been there: you find an exciting coding experiment, quickly copy the code, try to execute it, and it fails. Not because of missing dependencies, but due to missing functions, variables, or an improper code order. I **hate** that.

As someone who values simplicity and efficiency, I've often struggled with tools like Jupyter Notebooks. They offer robust features but can complicate version control, file management, and often require heavyweight IDEs. As a Neovim enthusiast, I prefer plain text solutions. This led me to a novel approach:

**What if you could write your entire article in a simple Python file—using comment blocks for text and converting it to Markdown?**

This method simplifies writing, keeps everything in a single file, and integrates perfectly with version control. Plus, it ensures that the file is executable and easy to run, making it a self-contained and efficient solution.

Here's the plan:

- Use top-level block comments (enclosed by triple quotes) for text.
- Treat the rest as code.

A program to achieve this needs to:

1. Extract text from top-level block comments.
2. Wrap Python code in Markdown code blocks.

Let's explore how to do this using standard Python libraries.

```python

from dataclasses import dataclass
from pathlib import Path
import argparse
import re
from typing import List
import typer

app = typer.Typer()

```
We structure the file into blocks. Each block is either a _text_ (Markdown) block or a _code_ block.

```python

@dataclass
class Block:
    content: str
    is_code: bool

```
Check if the given line marks the start or end of a block comment.

```python

def is_comment(line: str) -> bool:
    return line.startswith('"""') or line.startswith("'''")

```
Remove triple-quote syntax from a line. If the line consists solely of triple quotes
(with possible whitespace), return an empty string.

```python

def remove_comment_syntax(line: str) -> str:
    if line.strip() in ('"""', "'''"):
        return ""
    for token in ['"""', "'''"]:
        if line.lstrip().startswith(token):
            line = line.lstrip()[len(token) :]
            break
    for token in ['"""', "'''"]:
        if line.rstrip().endswith(token):
            line = line.rstrip()[: -len(token)]
            break
    return line

```
Unescape mathjax notation in a text by replacing double backslashes with a single backslash.
This is applied only to non-code (Markdown) text.

```python

def unescape_mathjax(text: str) -> str:
    return text.replace("\\\\", "\\")

```
Extract blocks of code and text from the given file.
The control flow has been inverted so that non-comment lines are handled immediately.

```python

def extract_blocks(file_path: Path) -> List[Block]:
    with file_path.open("r", encoding="utf-8") as f:
        lines = f.readlines()

    blocks: List[Block] = []
    in_comment_block = False
    comment_lines: List[str] = []
    code_lines: List[str] = []

    for line in lines:
        # Non-comment markers: process immediately.
        if not is_comment(line):
            if in_comment_block:
                comment_lines.append(line)
            else:
                code_lines.append(line)
            continue

        # Here, the line is a comment marker.
        if in_comment_block:
            comment_lines.append(remove_comment_syntax(line))
            blocks.append(Block("".join(comment_lines), is_code=False))
            comment_lines.clear()
            in_comment_block = False
            continue

        # Starting a new comment block.
        if code_lines:
            blocks.append(Block("".join(code_lines), is_code=True))
            code_lines.clear()
        comment_lines.append(remove_comment_syntax(line))
        in_comment_block = True

    if code_lines:
        blocks.append(Block("".join(code_lines), is_code=True))
    return blocks

```
Build a Markdown file from the blocks.
Wrap code blocks in Markdown code fences and process mathjax unescaping in text blocks.

```python

def build_markdown(blocks: List[Block], output_path: Path) -> None:
    output: List[str] = []
    for block in blocks:
        if not block.content.strip():
            continue
        if block.is_code:
            output.append("\n```python\n" + block.content + "\n```\n")
        else:
            # Unescape mathjax by removing redundant backslashes.
            output.append(unescape_mathjax(block.content))
    markdown_text = "".join(output)
    markdown_text = re.sub(r"\n{3,}", "\n\n", markdown_text)
    with output_path.open("w", encoding="utf-8") as f:
        f.write(markdown_text)

```
Tie everything together: parse arguments, extract blocks, and build Markdown output.

```python

@app.command()
def main(python_file: Path, markdown_file: Path) -> None:
    blocks = extract_blocks(python_file)
    build_markdown(blocks, markdown_file)

if __name__ == "__main__":
    app()

```
You can now run this script on any Python file from the terminal.
For example, if saved as `py2md.py`, run:

```sh
python3 py2md.py py2md.py out.md
```

Alternatively, install it directly from GitHub:

```sh
pip install git+https://github.com/weygoldt/py2md
```

Then, within the repository, simply run:

```sh
py2md py2md/main.py README.md
```

This command will generate the article you’re reading now as a Markdown file. Happy writing!

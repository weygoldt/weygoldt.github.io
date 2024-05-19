---
title: "How to write coding articles in plain Python files"
summary: "How to convert inline a Python file with some inline block comments to a beautiful markdown document"
# showSummary: true
categories: ["Post"]
tags: ["python", "markdown", "writing", "automation", "reproducibility"]
date: 2024-05-18
draft: false
---

We’ve all been there: you find an exciting coding experiment, quickly copy the
code, try to execute it, and it fails. Not because of missing dependencies, but
due to missing functions, variables, or messed-up code order. I **hate** that.

As a somebody who values simplicity and efficiency, I’ve often struggled with
tools like Jupyter Notebooks. They offer robust features but can complicate
version control, file management, and require heavy IDEs. As a Neovim
enthusiast, I prefer plain text solutions. This led me to a novel approach:

**What if you could write your entire article in a simple Python file,
using comment blocks for text and converting it to Markdown?**

This method simplifies writing, keeps everything in a single file, and
integrates perfectly with version control. Plus, it ensures that the file is
executable and easy to run, making it a self-contained and efficient solution.

Here's the plan:

- Use top-level block comments (enclosed by triple quotes) for text.
- Treat the rest as code.

A program to achieve this needs to:

1. Extract text from top-level block comments.
2. Wrap Python code in Markdown code blocks.

Let's explore how to do this using standard Python libraries.

## Imports and helper functions

We'll employ `dataclasses` to manage our content blocks, `argparse` to handle
command line inputs for source and destination files and `re` for some regex
magic. Ready to revolutionize your writing process? Let’s get started!

```python
from dataclasses import dataclass
import argparse
import re
```

As we have to keep the order of the content intact, my approach essentially
structures the content of the file into blocks. Each block can either be a
_content_ or a _code_ block. So first, lets define a simple dataclass that will
hold data of each block, witch is really only its content and its category:

```python
@dataclass
class Block:
    content: str
    is_code: bool
```

As a next step, lets define a function that helps us parse the command line
arguments. The only thing we need right now is the location of a Python
file that you would like to test this with and the location to where the output
file should go.

```python
def argparser():
    parser = argparse.ArgumentParser(
        description="Convert python files into markdown",
    )
    parser.add_argument(
        "--python",
        "-p",
        type=str,
        help="Path to the python file.",
    )
    parser.add_argument(
        "--markdown",
        "-m",
        type=str,
        help="Path to the output file.",
    )
    args = parser.parse_args()
    return args
```

We also need a small utility function that checks if the current line
is the line that indicates the beginning or end of a comment or not:

```python
def is_comment(line):
    return line.startswith('"""') or line.startswith("'''")
```

And a small function that strips the comment syntax from the line.

```python
def remove_comment_syntax(line):
    if line.startswith('"""'):
        line = line.replace('"""', "")
    elif line.startswith("'''"):
        line = line.replace("'''", "")
    elif line.endswith('"""'):
        line = line.replace('"""', "")
    elif line.endswith("'''"):
        line = line.replace("'''", "")
    return line
```

## Extract blocks of code and text

Now lets write the main functionality: We will parse the file line by line.
As soon as we encounter the start or stop of a comment, we flip a boolean
switch that keeps track of whether we are currently inside of a comment
block or inside of a code block. As soon as we detect the start of
a code comment block, we start to collect all the lines of code
into the `comment_block` list. If we encounter the stop of the
comment, we concatenate them into a single string in store them,
in addition tho the information of the block type, into an instance of the
`Block` class, wich we collect in our `blocks` list. The same applies to the
code blocks. We need to add an additional last `if`-statement at the end:
because of the logic I used to parse the file, the function "assumes" that
the last block is always a comment. If, after the loop is finished,
we still have content in our `code_block` list, we concatenate this as
well and store it in a block.

```python
def extract_comments(file_path):
    with open(file_path, "r") as file:
        code_lines = file.readlines()

    blocks = []

    in_comment_block = False

    comment_block = []
    code_block = []

    for i, line in enumerate(code_lines):
        # if this line marks the start or end of a comment block ...
        if is_comment(line):
            # ... and we are not already in a comment block, enter it
            if not in_comment_block:
                if i != 0:
                    # only append the code block if we are not in the first line
                    block = Block(
                        content="".join(code_block),
                        is_code=True,
                    )
                    blocks.append(block)
                    code_block = []

                comment_block.append(remove_comment_syntax(line))
                in_comment_block = True

            # ... and we are already in comment block, this is the end so we
            # exit it
            else:
                block = Block(
                    content="".join(comment_block),
                    is_code=False,
                )
                blocks.append(block)
                comment_block = []
                in_comment_block = False

        # if this is not the start or stop of a comment ...
        else:
            # ... but we currently are in a comment block, add the text to the
            # markdown list
            if in_comment_block:
                comment_block.append(line)

            # ... but we are not currently in a comment, then this is code
            else:
                code_block.append(line)

    # handle the end when the last block is not a comment but a code block
    if len(code_block) > 0:
        block = Block(
            content="".join(code_block),
            is_code=True,
        )
        blocks.append(block)
        code_block = []

    return blocks
```

This is probably not the most elegant implementation of this algorithm but
I whipped it together quickly and for now it works well. The output of this
function is now a list of instances of our `Block` class, each containing the
block content and whether it is code or content (i.e., Markdown text). What we
now need, is a function that puts this together to a Markdown file. For this,
we can simply iterate over our blocks and wrap each block into a Markdown code
block, if it contains code. Easy as that!

````python
def build_markdown(blocks, path):
    file = ""
    for i, block in enumerate(blocks):
        if len(block.content) == 0:
            continue

        # wrap the code in markdown code blocks
        if block.is_code:
            file += "\n```python\n"
            file += block.content
            file += "\n```\n"

        elif not block.is_code:
            file += block.content

    # remove more that 3 consecutive line breaks
    file = re.sub(r"\n{3,}", "\n\n", file)

    with open(path, "w") as mdfile:
        mdfile.write(file)
````

## Putting it all together

Now let us add a `main` function that ties this all together: It first parses
the arguments, then runs the function that extracts the blocks of text and code
and then puts it all back together into a Markdown file.

```python
def main():
    args = argparser()
    blocks = extract_comments(args.python)
    build_markdown(blocks, args.markdown)
```

Adding this piece of code now executes the main function if the script is
called directly.

```python
if __name__ == "__main__":
    main()
```

You can now call this script from your terminal on any Python file. In fact,
the article you are currently reading was produced by the very Python code
you've just seen. You can check out the full content on my
[GitHub](https://github.com/weygoldt/py2md). To use the script, save it as
`py2md.py` and run it on itself with the following command:

```sh
python3 py2md.py -p py2md.py -m out.md
```

Alternatively, you can install it as a package directly from GitHub with
the following command.

```sh
pip install git+github.com/weygoldt/py2md
```

Then you can use the script from your terminal directly. For example,
to generate the package `README.md`, I simply called from within the
repository:

```sh
py2md -p py2md/main.py -m README.md
```

This will generate the article you are reading now, formatted as a Markdown
file. Naturally, this method works with other Python files as well.

## Conclusion

By leveraging the simplicity of plain Python files and the power of basic
Python libraries, you can streamline your writing process and maintain full
control over versioning. This method not only simplifies the integration of
code and text but also ensures that your articles remain easy to **manage** and
most importantly, **reproduce**. This is particularly important for
data-centric projects, where being able to generate your report from the
terminal, complete with all plots and without errors, is **immensely
satisfying**. Happy writing!

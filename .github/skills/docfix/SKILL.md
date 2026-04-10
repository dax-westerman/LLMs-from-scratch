---
name: docfix
summary: >
  Wraps all comments and docstrings in a file or notebook cell to the max-line-length specified in [tool.flake8] of pyproject.toml.
description: |
  This skill finds all comments (lines starting with #, triple-quoted docstrings, and markdown cells in notebooks) in a file or cell, and rewrites them so that no line exceeds the configured max-line-length from [tool.flake8] in pyproject.toml. It preserves indentation and formatting, and only wraps text within comment/doc blocks (not code).

  Typical workflow:
  1. Read [tool.flake8] from pyproject.toml to determine max-line-length.
  2. For each file or cell:
     - Identify all comment lines and docstrings (Python: #, """...""", '''...''').
     - For notebooks, also process markdown cells.
     - For each comment/doc block, reflow text to wrap at max-line-length, preserving indentation and bullet lists.
     - Replace the original block with the wrapped version.
  3. Output a diff or apply changes in-place.

  Decision points:
  - If [tool.flake8] is missing, default to 79.
  - If a block contains code examples or tables, do not wrap those lines.
  - For notebook markdown, preserve LaTeX/math blocks.

  Completion criteria:
  - All comment/doc lines are wrapped to the correct length.
  - No code or formatting is broken.
  - Output is ready for linting with flake8.

examples:
  - prompt: /docfix ch02/01_main-chapter-code/ch02.ipynb
    description: "Wrap all comments and markdown in the notebook to the max-line-length from pyproject.toml."
  - prompt: /docfix pkg/llms_from_scratch/model.py
    description: "Wrap all comments and docstrings in model.py."

# Related customizations
# - Consider a hook to auto-run docfix on save
# - Extend for other languages (e.g., JS, C++)

---
description: "Use when writing or modifying Python code. Covers file organization, immutability, naming, error handling, and input validation conventions."
paths: 
 - "**/*.py"
---

# Code Organization

- Many small files over few large files
- High cohesion, low coupling
- 200-400 lines typical, 800 max per file
- Organize by feature/domain, not by type
- No circular import

# Code Style

- No emojis in code, comments, or documentation
- Immutability always — never mutate objects or arrays
- Follow PEP 8 style guidelines
- Use 4 spaces for indentation (never tabs)
- Use meaningful, descriptive variable and function names
- Avoid abbreviations
- Use snake_case for functions/variables, PascalCase for classes, UPPER_CASE for constants
- Limit line length to 88 characters (ruff formatter standard)
- Use database transactions and atomic operations when performing database updates to ensure data integrity and consistency
- No "fake" privacy: do not prefix module-level functions, constants, variables, classes, or import aliases with a leading underscore. Python does not enforce privacy, the underscore only hides names from `import *` and adds noise. A leading underscore is reserved for intentionally-unused names (e.g. `*_args`, `**_kwargs`, throwaway loop/unpacking targets like `for _key, value in ...`)

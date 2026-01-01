# Python Script Header Template

> Template Version: 1.0  
> Applies To: All `.py` files in ai-models-wiki  
> Last Updated: 2025-12-31

---

## Template

```python
#!/usr/bin/env python3
"""
Script Name  : script_name.py
Description  : [One-line description of what the script does]
Repository   : ai-models-wiki
Author       : VintageDon (https://github.com/vintagedon)
ORCID        : 0009-0008-7695-4093
Created      : YYYY-MM-DD
Phase        : [Phase XX - Phase Name]
Link         : https://github.com/radioastronomyio/aimodels-wiki

Description
-----------
[2-4 sentences explaining the script's purpose, what it operates on,
and what outputs it produces. Include any important behavioral notes.]

Usage
-----
    python script_name.py [options]

Examples
--------
    python script_name.py
        [Description of what this invocation does]

    python script_name.py --verbose
        [Description of what this invocation does]
"""

# =============================================================================
# Imports
# =============================================================================

from pathlib import Path

# =============================================================================
# Configuration
# =============================================================================

# [Configuration constants with inline comments]

# =============================================================================
# Functions
# =============================================================================


def main() -> None:
    """Entry point for script execution."""
    pass


# =============================================================================
# Entry Point
# =============================================================================

if __name__ == "__main__":
    main()
```

---

## Field Descriptions

| Field | Required | Description |
|-------|----------|-------------|
| Script Name | Yes | Filename for reference (snake_case) |
| Description | Yes | Single line, verb-led description |
| Repository | Yes | Repository name |
| Author | Yes | Name with GitHub profile link |
| ORCID | Yes | Author ORCID identifier |
| Created | Yes | Creation date (YYYY-MM-DD) |
| Phase | Yes | Development phase this script belongs to |
| Link | Yes | Full repository URL |
| Description section | Yes | Expanded multi-line explanation |
| Usage section | Yes | Command syntax |
| Examples section | Yes | At least one usage example |

---

## Phase Reference

| Phase | Name |
|-------|------|
| Phase 01 | Ideation and Setup |
| Phase 02 | Site Development |
| Phase 03 | Analytics and Comparison |
| Phase 04 | Production Deployment |

---

## Section Comments

Use banner comments to separate logical sections:

```python
# =============================================================================
# Section Name
# =============================================================================
```

Standard sections (in order):

1. **Imports** — Standard library, third-party, local imports (in that order)
2. **Configuration** — Constants, paths, settings
3. **Functions** — Function and class definitions
4. **Entry Point** — `if __name__ == "__main__":` block

---

## Docstring Style

Use NumPy-style docstrings for functions:

```python
def parse_model_card(
    yaml_path: Path,
    validate: bool = True
) -> dict:
    """
    Parse a model card YAML file into a structured dictionary.

    Parameters
    ----------
    yaml_path : Path
        Path to the YAML model card file.
    validate : bool, optional
        Whether to validate against schema. Default is True.

    Returns
    -------
    dict
        Parsed model card data with normalized fields.

    Raises
    ------
    ValueError
        If YAML is malformed or fails validation.
    FileNotFoundError
        If yaml_path does not exist.

    Examples
    --------
    >>> card = parse_model_card(Path("anthropic-claude-sonnet-45-model-card.yaml"))
    >>> card["model_identity"]["name"]
    'Claude Sonnet 4.5'
    """
    pass
```

---

## Notes

- Use `#!/usr/bin/env python3` for portability
- Module docstring goes immediately after shebang
- Keep Description line under 80 characters
- Use present tense, active voice ("Parses..." not "This script parses...")
- Use `pathlib.Path` instead of string paths
- Use type hints for all function parameters and return values
- Follow PEP 8 style guide

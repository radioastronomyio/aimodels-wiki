# PowerShell Script Header Template

> Template Version: 1.0  
> Applies To: All `.ps1` files in ai-models-wiki  
> Last Updated: 2025-12-31

---

## Template

```powershell
<#
.SYNOPSIS
    [One-line description of what the script does]

.DESCRIPTION
    [2-4 sentences explaining the script's purpose, what it operates on,
    and what outputs it produces. Include any important behavioral notes.]

.NOTES
    Repository  : ai-models-wiki
    Author      : VintageDon (https://github.com/vintagedon)
    ORCID       : 0009-0008-7695-4093
    Created     : YYYY-MM-DD
    Phase       : [Phase XX - Phase Name]

.EXAMPLE
    .\script-name.ps1

    [Description of what this invocation does]

.EXAMPLE
    .\script-name.ps1 -Parameter Value

    [Description of what this invocation does]

.LINK
    https://github.com/radioastronomyio/aimodels-wiki
#>

# =============================================================================
# Configuration
# =============================================================================

# [Configuration variables with inline comments]

# =============================================================================
# Functions
# =============================================================================

# [Function definitions if needed]

# =============================================================================
# Execution
# =============================================================================

# [Main script logic]
```

---

## Field Descriptions

| Field | Required | Description |
|-------|----------|-------------|
| `.SYNOPSIS` | Yes | Single line, verb-led description |
| `.DESCRIPTION` | Yes | Expanded explanation of purpose and behavior |
| `.NOTES` | Yes | Static metadata (repository, author, ORCID, dates, phase) |
| `.EXAMPLE` | Yes | At least one usage example with description |
| `.LINK` | Yes | Repository URL |
| `.PARAMETER` | If applicable | Document any script parameters |

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

```powershell
# =============================================================================
# Section Name
# =============================================================================
```

Standard sections (in order):

1. **Configuration** — Variables, paths, settings
2. **Functions** — Helper function definitions (if any)
3. **Execution** — Main script logic

---

## Notes

- PowerShell comment-based help (`.SYNOPSIS`, `.DESCRIPTION`, etc.) enables `Get-Help script-name.ps1`
- Keep `.SYNOPSIS` under 80 characters
- Use present tense, active voice ("Converts..." not "This script converts...")
- Phase field should match work-log phase names exactly

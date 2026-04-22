# Contributing to RTL-First Dev Kit

Thank you for improving the only production-grade RTL toolkit built for AI coding assistants.

## What lives in this repo

| Directory | Contents |
|-----------|----------|
| `tailwind/` | Tailwind 4.x logical-property mapping tables and usage patterns |
| `flutter/` | Flutter directional layout API patterns and migration notes |
| `nextjs/` | Next.js `dir` propagation, App Router, and i18n routing patterns |
| `biome/` | Biome lint policy rules that block physical-direction regressions |
| `ci/` | CI workflow snippets for RTL validation in real pipelines |
| `skills/` | Claude Code / Gemini CLI skill files that load this toolkit automatically |
| `cheatsheet/` | Quick-reference cards for the most common patterns |

---

## How to contribute

### 1. Adding a new mapping or pattern

1. Open the relevant file (`tailwind/mapping.md`, `flutter/patterns.md`, etc.).
2. Follow the existing table or code-block format exactly — no custom formatting.
3. Always include both the **physical** anti-pattern and the **logical** equivalent side-by-side.
4. If you add a Flutter pattern, test it in an actual `Directionality` context. If you add a Tailwind pattern, verify it against Tailwind 4.x (not v3).

### 2. Fixing an example

Open a pull request with the corrected snippet. In the PR description state:
- Which file and line changed
- Why the old example was wrong (e.g., "was using `ml-` which is physical")
- What it should be (e.g., "should use `ms-` which is logical")

### 3. Creating a new skill file

A skill file teaches AI coding assistants how to apply this toolkit automatically. Each skill lives at `skills/<skill-name>/SKILL.md`.

Every `SKILL.md` must:

1. **Start with YAML frontmatter** (`---` block) containing at least `name`, `description`, and `triggers`.
2. **Include the security guardrail comment** immediately after the frontmatter closing `---`:
   ```
   <!-- SECURITY GUARDRAIL: ... -->
   ```
   The comment text should describe what the skill does and does not do. See `skills/rtl-fix/SKILL.md` for a concrete example.
3. **List specific triggers** — phrases that should cause an AI assistant to load this skill automatically (e.g., `"rtl fix"`, `"code review"`, `"tsx file edit"`).

CI will fail if either requirement is missing.

### 4. Improving CI or the Biome policy

- `ci/` contains workflow snippets, not the actual `.github/workflows/` files. Improvements to the repo's own CI go directly to `.github/workflows/ci.yml`.
- Biome rule changes go to `biome/policy.md` with an explanation of which physical-direction pattern they catch and why it cannot be auto-fixed.

---

## Coding conventions

- **No `ml-`, `mr-`, `pl-`, `pr-`, `left-`, `right-`** in any example unless you are explicitly showing the anti-pattern.
- All anti-pattern examples must be clearly marked with a `// BEFORE` or `BAD:` comment.
- All logical-property examples must be marked with `// AFTER` or `GOOD:`.
- Keep snippets short enough to copy directly into a project. If an example needs more than ~20 lines, split it.

---

## Pull request checklist

- [ ] CI passes locally (run `python3 .github/validate-skills.py` if available, or check the CI log)
- [ ] All new `SKILL.md` files have frontmatter and a `<!-- SECURITY GUARDRAIL:` comment
- [ ] No physical-direction classes appear in "correct" examples
- [ ] PR description explains the change in one sentence

---

## Running CI validation locally

```bash
python3 - <<'PY'
from pathlib import Path
GUARDRAIL_PREFIX = '<!-- SECURITY GUARDRAIL:'
errors = []
skill_files = [f for f in Path('.').rglob('SKILL.md') if '.git' not in f.parts]
for skill_file in skill_files:
    content = skill_file.read_text(encoding='utf-8')
    if not content.startswith('---'):
        errors.append(f'{skill_file}: missing YAML frontmatter')
    elif GUARDRAIL_PREFIX not in content:
        errors.append(f'{skill_file}: missing SECURITY GUARDRAIL comment')
if errors:
    for e in errors: print(f'ERROR: {e}')
    raise SystemExit(1)
print(f'All {len(skill_files)} SKILL.md files valid')
PY
```

---

## License

By contributing you agree your changes are licensed under the [MIT License](LICENSE) already covering this repository.

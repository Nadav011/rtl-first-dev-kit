# RTL-First Dev Kit — The Only Production-Grade RTL Toolkit for AI Coding

> **Tailwind 4.x logical properties. CSS logical properties. Flutter directional APIs. Zero `ml-4` survivors.**

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Compatible](https://img.shields.io/badge/Claude%20Code-compatible-orange.svg)](https://claude.ai/code)
[![Gemini CLI Compatible](https://img.shields.io/badge/Gemini%20CLI-compatible-4285F4.svg)](https://ai.google.dev/gemini-api/docs/gemini-cli)
[![RTL-First](https://img.shields.io/badge/RTL--First-%E2%9C%93-22c55e.svg)](.)
[![Tailwind 4.x](https://img.shields.io/badge/Tailwind-4.x-38bdf8.svg)](https://tailwindcss.com)

---

## The Problem: `ml-4` Silently Breaks Your RTL Layout

Every RTL tutorial gives you `dir="rtl"` and calls it a day. The real problem is subtler and far more widespread: **physical direction classes are RTL-blind**.

```jsx
// BEFORE — ml-4, pl-3, text-left are physical. They are always left.
// In Arabic or Hebrew, "left" is the END of the line — the wrong side.
<div className="ml-4 pl-3 text-left border-l-2 rounded-tl-lg">
  {/* margin is still on the LEFT — in RTL this is the trailing edge, not the leading one */}
  {/* padding anchored to the LEFT — shifts content in the wrong direction */}
  {/* text-left forces alignment — ignores the document direction entirely */}
  {/* border-l-2 puts the accent border on the wrong side */}
  {/* rounded-tl-lg rounds the top-left corner, not the reading-start corner */}
</div>

// AFTER — ms-4, ps-3, text-start are logical. They follow dir="rtl" automatically.
// One class. Two languages. Zero overrides.
<div className="ms-4 ps-3 text-start border-s-2 rounded-ss-lg">
  {/* ms-4  = margin-inline-start → right in RTL, left in LTR */}
  {/* ps-3  = padding-inline-start → same automatic flip */}
  {/* text-start → right-aligned in RTL, left-aligned in LTR */}
  {/* border-s-2 → border on the reading-start edge, correct in both directions */}
  {/* rounded-ss-lg → border-start-start-radius, rounds the correct corner */}
</div>
```

`ml-4` means "16px from the physical left edge". Always. Permanently. RTL-blind.
`ms-4` means "16px from the inline-start edge". Automatically correct in every language.

**CSS logical properties are the only correct abstraction for multilingual layout.**
This kit enforces them everywhere — in your AI assistant, in code review, and in CI.

---

## What Is Included

| Path | Purpose |
|------|---------|
| `skills/rtl-validator/SKILL.md` | AI skill: validates RTL compliance during code review |
| `skills/rtl-fix/SKILL.md` | AI skill: auto-converts physical to logical properties |
| `cheatsheet/tailwind-rtl.md` | Complete Tailwind 4.x logical property reference |
| `CI/rtl-check.yml` | GitHub Actions: blocks PRs containing physical direction classes |
| `tailwind/` | Tailwind config patterns and utility mapping tables |
| `flutter/` | Flutter directional primitives and migration reference |
| `nextjs/` | Next.js 15/16 locale routing and `dir` propagation patterns |
| `biome/` | Biome lint policy for physical-direction regressions |

---

## Platforms

| Platform | How to use |
|----------|-----------|
| **Claude Code** | Native skill — copy `skills/rtl-validator/` and `skills/rtl-fix/` to `~/.claude/skills/` |
| **Gemini CLI** | Use `SKILL.md` content as system prompt prefix for any code review session |
| **Kiro** | Paste `SKILL.md` into agent instructions |
| **Cursor** | Paste `SKILL.md` contents into `.cursorrules` or `.mdc` rules file |

---

## Quick Install

```bash
# Copy skills into your Claude Code global skills directory
cp -r skills/rtl-validator ~/.claude/skills/
cp -r skills/rtl-fix ~/.claude/skills/

# Copy the CI workflow into your project
cp CI/rtl-check.yml .github/workflows/

# Copy the cheatsheet to your docs
cp cheatsheet/tailwind-rtl.md docs/rtl-reference.md
```

```bash
# Coming soon — scaffold an RTL-first Next.js project in one command
npx create-rtl-app
```

### Claude Code

```
/rtl-validator   — scan current file or PR diff for RTL violations
/rtl-fix         — auto-convert physical direction classes to logical equivalents
```

### Gemini CLI

```bash
gemini -p "$(cat skills/rtl-validator/SKILL.md)" -- review src/components/Card.tsx
```

### Cursor / Kiro (MDC rules)

Paste the contents of `skills/rtl-validator/SKILL.md` into your `.cursorrules` or `.kiro/rules/rtl.md` file.

---

## Why Logical Properties

There are **50M+ Arabic and Hebrew developers** building apps that serve over **400 million native RTL speakers**. The vast majority of "RTL-supported" codebases still contain hundreds of `ml-`, `mr-`, `pl-`, `pr-` violations that silently produce wrong layouts at production scale.

The problem is that `text-right` is not RTL. It is "align to the physical right edge". That is coincidentally correct in Arabic but catastrophically wrong in left-to-right contexts — and it produces untestable, direction-coupled code.

Logical properties (`text-start`, `ms-*`, `ps-*`, `border-s-*`) express **reading-direction intent**, not physical position. They are the W3C standard, supported in all modern browsers, and the only correct foundation for bidirectional apps.

**117+ RTL-first components in production across 18 Israeli projects** — zero `ml-`/`mr-` in the codebase.

---

## Full Class Mapping

| Physical class (never use) | Logical class (always use) | CSS property generated |
|---------------------------|---------------------------|------------------------|
| `ml-{n}` | `ms-{n}` | `margin-inline-start` |
| `mr-{n}` | `me-{n}` | `margin-inline-end` |
| `pl-{n}` | `ps-{n}` | `padding-inline-start` |
| `pr-{n}` | `pe-{n}` | `padding-inline-end` |
| `left-{n}` | `inset-s-{n}` | `inset-inline-start` |
| `right-{n}` | `inset-e-{n}` | `inset-inline-end` |
| `text-left` | `text-start` | `text-align: start` |
| `text-right` | `text-end` | `text-align: end` |
| `border-l-{n}` | `border-s-{n}` | `border-inline-start-width` |
| `border-r-{n}` | `border-e-{n}` | `border-inline-end-width` |
| `rounded-l-{n}` | `rounded-s-{n}` | both inline-start radii |
| `rounded-r-{n}` | `rounded-e-{n}` | both inline-end radii |
| `rounded-tl-{n}` | `rounded-ss-{n}` | `border-start-start-radius` |
| `rounded-tr-{n}` | `rounded-se-{n}` | `border-start-end-radius` |
| `rounded-bl-{n}` | `rounded-es-{n}` | `border-end-start-radius` |
| `rounded-br-{n}` | `rounded-ee-{n}` | `border-end-end-radius` |
| `float-left` | `float-start` | `float: inline-start` |
| `float-right` | `float-end` | `float: inline-end` |
| `scroll-ml-{n}` | `scroll-ms-{n}` | `scroll-margin-inline-start` |
| `scroll-pl-{n}` | `scroll-ps-{n}` | `scroll-padding-inline-start` |

Full reference with edge cases: [cheatsheet/tailwind-rtl.md](cheatsheet/tailwind-rtl.md)

---

## Real-World Example: Hebrew Product Card

```jsx
// WRONG — every directional class is physically anchored
function ProductCard({ product }) {
  return (
    <div className="flex items-center pl-4 pr-2 border-l-4 border-blue-500 rounded-tl-lg rounded-bl-lg">
      <img className="mr-3" src={product.image} alt={product.name} />
      <div className="text-left">
        <h3 className="font-bold text-gray-900">{product.name}</h3>
        <p className="text-sm text-gray-500 ml-1">{product.description}</p>
        <span className="font-mono text-blue-600">{product.price}</span>
      </div>
      <ChevronRight className="ml-auto text-gray-400" />
    </div>
  );
}

// CORRECT — fully logical, works in he-IL, ar-SA, ar-EG, and en-US with zero overrides
function ProductCard({ product }) {
  return (
    <div className="flex items-center ps-4 pe-2 border-s-4 border-blue-500 rounded-ss-lg rounded-es-lg">
      <img className="me-3" src={product.image} alt={product.name} />
      <div className="text-start">
        <h3 className="font-bold text-gray-900">{product.name}</h3>
        <p className="text-sm text-gray-500 ms-1">{product.description}</p>
        {/* Numbers are LTR even in RTL context — wrap explicitly */}
        <span dir="ltr" className="font-mono tabular-nums text-blue-600">
          {product.price}
        </span>
      </div>
      {/* Horizontal chevron flips in RTL — arrow direction carries meaning */}
      <ChevronRight className="ms-auto text-gray-400 rtl:rotate-180" />
    </div>
  );
}
```

---

## CI Enforcement

Add `CI/rtl-check.yml` to `.github/workflows/`. Every pull request against `*.tsx`, `*.jsx`, `*.css`, and `*.vue` files is scanned for physical direction classes. Violations block merge.

Example output when violations are found:

```
RTL violations found — fix before merge:

  src/components/Card.tsx:14    ml-4        → use ms-4
  src/components/Card.tsx:15    pl-3        → use ps-3
  src/components/Nav.tsx:8      text-left   → use text-start
  src/pages/Profile.tsx:22      border-l-2  → use border-s-2

4 violation(s) across 3 files.
Run /rtl-fix to auto-convert, or add // rtl-ok on lines that are intentionally directional.
```

---

## Flutter RTL

```dart
// WRONG — left/right are physical, always pinned to physical edges
Padding(
  padding: EdgeInsets.only(left: 16, right: 8),
  child: Align(
    alignment: Alignment.centerLeft,
    child: Text('שלום עולם'),
  ),
)

// CORRECT — Directional APIs respond to the Directionality widget
Padding(
  padding: EdgeInsetsDirectional.only(start: 16, end: 8),
  child: Align(
    alignment: AlignmentDirectional.centerStart,
    child: Text('שלום עולם'),
  ),
)

// CORRECT — positioned widget with directional offset
Stack(
  children: [
    PositionedDirectional(
      start: 16,
      top: 8,
      child: Icon(Icons.check),
    ),
  ],
)
```

See `flutter/` for the full Flutter RTL reference.

---

## Contributing

1. Zero `ml-`, `mr-`, `pl-`, `pr-`, `text-left`, `text-right`, `float-left`, `float-right` in any example
2. All examples must render correctly with both `<html dir="rtl" lang="he">` and `<html dir="ltr" lang="en">`
3. Test by running `document.documentElement.dir = 'rtl'` in DevTools before submitting
4. Flutter examples must compile without `left`, `right`, `topLeft`, `topRight` in widget APIs
5. If an element is genuinely LTR-only (e.g., a map widget), wrap in `dir="ltr"` and add a `// rtl-ok` comment

---

## License

MIT — use it, extend it, ship it.

---

*Built by [Nadav Cohen](https://github.com/nadavcohen) — shipping Hebrew-first since before it was a thing.*

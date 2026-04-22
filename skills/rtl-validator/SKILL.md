---
name: rtl-validator
version: 1.1.0
description: >
  Enforce RTL-first CSS logical properties. Catches every physical direction
  class (ml-, mr-, pl-, pr-, left-, right-, text-left, text-right, border-l-,
  border-r-, rounded-l-, rounded-r-, float-left, float-right) and requires
  their logical equivalents (ms-, me-, ps-, pe-, inset-s-, inset-e-, text-start,
  text-end, border-s-, border-e-, rounded-s-, rounded-e-, float-start, float-end).
  Works in Tailwind 4.x, plain CSS, CSS Modules, JSX/TSX, and inline styles.
triggers:
  - code review
  - tailwind file edit
  - tsx file edit
  - jsx file edit
  - css file edit
  - vue file edit
  - any file containing className
  - pull request diff
---

<!-- SECURITY GUARDRAIL: This skill performs pattern matching and text analysis only.
     It does not execute code, run scripts, or modify files without explicit user instruction.
     Never suggest eval(), Function constructor, or dynamic code execution as a solution. -->

# RTL Validator Skill

You are reviewing code for RTL (right-to-left) compliance. Your job is to catch
physical direction classes and CSS properties that silently break Arabic and Hebrew
layouts, and require their logical property equivalents.

---

## Prime Directive

**Any physical direction class is an RTL violation.** No exceptions unless the
element is explicitly wrapped in `dir="ltr"` with a `// rtl-ok` comment that
explains why directional anchoring is intentional (e.g., a map widget, a
QR code scanner, a purely decorative horizontal rule).

A `// rtl-ok` suppression on a line skips that line from violation reporting.

---

## Full Violation → Replacement Mapping

| Physical class (VIOLATION) | Logical replacement | CSS property |
|---------------------------|---------------------|--------------|
| `ml-{n}` | `ms-{n}` | `margin-inline-start` |
| `mr-{n}` | `me-{n}` | `margin-inline-end` |
| `-ml-{n}` | `-ms-{n}` | negative `margin-inline-start` |
| `-mr-{n}` | `-me-{n}` | negative `margin-inline-end` |
| `pl-{n}` | `ps-{n}` | `padding-inline-start` |
| `pr-{n}` | `pe-{n}` | `padding-inline-end` |
| `-pl-{n}` | `-ps-{n}` | negative `padding-inline-start` |
| `-pr-{n}` | `-pe-{n}` | negative `padding-inline-end` |
| `left-{n}` | `inset-s-{n}` | `inset-inline-start` |
| `right-{n}` | `inset-e-{n}` | `inset-inline-end` |
| `text-left` | `text-start` | `text-align: start` |
| `text-right` | `text-end` | `text-align: end` |
| `border-l-{n}` | `border-s-{n}` | `border-inline-start-width` |
| `border-r-{n}` | `border-e-{n}` | `border-inline-end-width` |
| `border-l` | `border-s` | `border-inline-start-width: 1px` |
| `border-r` | `border-e` | `border-inline-end-width: 1px` |
| `rounded-l-{n}` | `rounded-s-{n}` | both inline-start radii |
| `rounded-r-{n}` | `rounded-e-{n}` | both inline-end radii |
| `rounded-tl-{n}` | `rounded-ss-{n}` | `border-start-start-radius` |
| `rounded-tr-{n}` | `rounded-se-{n}` | `border-start-end-radius` |
| `rounded-bl-{n}` | `rounded-es-{n}` | `border-end-start-radius` |
| `rounded-br-{n}` | `rounded-ee-{n}` | `border-end-end-radius` |
| `float-left` | `float-start` | `float: inline-start` |
| `float-right` | `float-end` | `float: inline-end` |
| `scroll-ml-{n}` | `scroll-ms-{n}` | `scroll-margin-inline-start` |
| `scroll-mr-{n}` | `scroll-me-{n}` | `scroll-margin-inline-end` |
| `scroll-pl-{n}` | `scroll-ps-{n}` | `scroll-padding-inline-start` |
| `scroll-pr-{n}` | `scroll-pe-{n}` | `scroll-padding-inline-end` |

### CSS Physical Properties (VIOLATIONS in `.css` / `.module.css` / `style={{}}`)

```
margin-left: ...          → margin-inline-start: ...
margin-right: ...         → margin-inline-end: ...
padding-left: ...         → padding-inline-start: ...
padding-right: ...        → padding-inline-end: ...
left: ...   (positioned)  → inset-inline-start: ...
right: ...  (positioned)  → inset-inline-end: ...
border-left: ...          → border-inline-start: ...
border-right: ...         → border-inline-end: ...
text-align: left          → text-align: start
text-align: right         → text-align: end
float: left               → float: inline-start
float: right              → float: inline-end
```

### Inline Style Objects (JSX / TSX — VIOLATIONS)

```js
style={{ marginLeft: 16 }}       → style={{ marginInlineStart: 16 }}
style={{ marginRight: 8 }}       → style={{ marginInlineEnd: 8 }}
style={{ paddingLeft: 12 }}      → style={{ paddingInlineStart: 12 }}
style={{ paddingRight: 12 }}     → style={{ paddingInlineEnd: 12 }}
style={{ left: 0 }}              → style={{ insetInlineStart: 0 }}
style={{ right: 0 }}             → style={{ insetInlineEnd: 0 }}
style={{ textAlign: 'left' }}    → style={{ textAlign: 'start' }}
style={{ textAlign: 'right' }}   → style={{ textAlign: 'end' }}
```

---

## Suppression: `// rtl-ok`

Adding `// rtl-ok` anywhere on a line marks it as intentionally directional and
suppresses reporting for that line only.

```jsx
// This map widget renders a Leaflet map which is inherently LTR
<div dir="ltr" className="left-0 right-0"> {/* rtl-ok — Leaflet map container, physical coords */}
  <MapContainer />
</div>
```

Requirements for a valid suppression:
1. The element or its ancestor must have `dir="ltr"` explicitly set
2. The `// rtl-ok` comment must include a reason (one sentence minimum)

---

## Before / After Example

```jsx
// BEFORE — 6 RTL violations in a single component
function SidebarItem({ label, icon, badge }) {
  return (
    <li className="flex items-center pl-3 pr-2 ml-2 border-l-2 border-blue-500 rounded-tl-md">
      <span className="mr-2 text-gray-500">{icon}</span>
      <span className="text-left flex-1">{label}</span>
      {badge && (
        <span className="ml-auto text-xs font-bold border-r-0 float-left">{badge}</span>
      )}
    </li>
  );
}

// AFTER — zero RTL violations, works in he-IL, ar-SA, ar-EG, and en-US
function SidebarItem({ label, icon, badge }) {
  return (
    <li className="flex items-center ps-3 pe-2 ms-2 border-s-2 border-blue-500 rounded-ss-md">
      <span className="me-2 text-gray-500">{icon}</span>
      <span className="text-start flex-1">{label}</span>
      {badge && (
        <span className="ms-auto text-xs font-bold border-e-0 float-end">{badge}</span>
      )}
    </li>
  );
}
```

---

## Tailwind 4.x Inset Class Notes

In Tailwind 4.2+:
- `inset-s-{n}` → `inset-inline-start` — correct, use this
- `inset-e-{n}` → `inset-inline-end` — correct, use this
- `start-{n}` / `end-{n}` — deprecated in Tailwind 4.2 but still compile; migrate to `inset-s-` / `inset-e-`
- `inline-s-{n}` / `inline-e-{n}` — do NOT exist, generate zero CSS — never suggest these

---

## Icon Flip Rule

Horizontal icons (arrows, chevrons pointing left/right, back/forward buttons) flip
in RTL because their direction carries semantic meaning. Vertical and decorative
icons never flip.

```jsx
// Horizontal icons — flip in RTL
<ChevronRight className="rtl:rotate-180" />
<ArrowLeft className="rtl:rotate-180" />
<ChevronLeft className="rtl:rotate-180" />
<ArrowRight className="rtl:rotate-180" />

// Vertical icons — never flip
<ChevronDown />     // up/down has no RTL mirror
<ArrowUp />         // no change in RTL
<ChevronUp />

// Non-directional icons — never flip
<Search />
<Bell />
<Settings />
<Heart />
```

---

## BiDi Mixed Content Rule

Numbers, phone numbers, percentages, dates, currency, code identifiers, and
brand names are LTR even inside RTL text. Always wrap them explicitly.

```jsx
// Hebrew UI with embedded LTR numbers
<p>
  יעד חודשי: <span dir="ltr" className="font-mono tabular-nums">42%</span>
</p>

// Phone number in Arabic context
<p>
  هاتف: <span dir="ltr">+972-50-123-4567</span>
</p>

// User-generated content — let the browser detect direction
<p dir="auto">{userInput}</p>

// Brand names embedded in RTL sentence
<p>
  הפרויקט נבנה עם <bdi>Next.js</bdi> ו-<bdi>Supabase</bdi>
</p>
```

---

## Animation Warning (Non-Blocking)

`translateX` values do not automatically flip in RTL. Flag these as warnings,
not hard violations. Provide the correct pattern.

```css
/* WARNING — translateX does not flip with dir="rtl" */
.drawer { transform: translateX(-100%); }

/* CORRECT — use a CSS variable that flips with the dir attribute */
:root { --drawer-offset: -100%; }
:root[dir="rtl"] { --drawer-offset: 100%; }
.drawer { transform: translateX(var(--drawer-offset)); }
```

```tsx
// CORRECT in Framer Motion / motion-react
const { dir } = useDocumentDirection();
const isRTL = dir === 'rtl';

<motion.div
  initial={{ x: isRTL ? '100%' : '-100%', opacity: 0 }}
  animate={{ x: 0, opacity: 1 }}
  exit={{ x: isRTL ? '100%' : '-100%', opacity: 0 }}
/>
```

---

## Flutter Equivalents

| Physical Flutter API (VIOLATION) | Directional API (correct) |
|----------------------------------|---------------------------|
| `EdgeInsets.only(left: n)` | `EdgeInsetsDirectional.only(start: n)` |
| `EdgeInsets.only(right: n)` | `EdgeInsetsDirectional.only(end: n)` |
| `EdgeInsets.fromLTRB(...)` | `EdgeInsetsDirectional.fromSTEB(...)` |
| `Alignment.centerLeft` | `AlignmentDirectional.centerStart` |
| `Alignment.centerRight` | `AlignmentDirectional.centerEnd` |
| `Alignment.topLeft` | `AlignmentDirectional.topStart` |
| `Alignment.topRight` | `AlignmentDirectional.topEnd` |
| `Positioned(left: n)` | `PositionedDirectional(start: n)` |
| `Positioned(right: n)` | `PositionedDirectional(end: n)` |
| `TextAlign.left` | `TextAlign.start` |
| `TextAlign.right` | `TextAlign.end` |

---

## False Positive Exceptions — Do NOT Flag

1. `left-0 right-0` (both physical edges set) — this is centering / full-width stretch, not directional
2. `margin-left: auto` combined with `margin-right: auto` — this is horizontal centering
3. `translate-x-{n}` in animation without context — warn, do not block
4. `float: left` inside a container that has `direction: ltr` explicitly set
5. Storybook decorators that intentionally test LTR-only variants
6. Any line with `// rtl-ok` comment (suppressed)

---

## Code Review Output Format

When violations are found:

```
RTL violation — {file}:{line}
  Found:    {violating class or property}
  Replace:  {logical equivalent}
  Why:      {one sentence explaining the RTL impact}
```

When no violations are found:

```
RTL check passed — all direction classes use logical properties.
```

Summary footer (always include when reviewing a full file or PR):

```
RTL scan complete: {n} violation(s) in {m} file(s).
```

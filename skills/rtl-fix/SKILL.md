---
name: rtl-fix
version: 1.1.0
description: >
  Auto-converts physical direction classes to CSS logical property equivalents.
  Replaces ml-/mr-/pl-/pr- spacing, left-/right- positioning, text-left/text-right
  alignment, border-l-/border-r- borders, rounded-l-/rounded-r- radii, float-left/
  float-right, and scroll-ml-/scroll-pl- scroll utilities with their Tailwind 4.x
  logical equivalents. Handles edge cases and notes what cannot be auto-fixed.
triggers:
  - rtl fix
  - fix rtl
  - convert physical classes
  - rtl-fix
  - make rtl safe
---

<!-- SECURITY GUARDRAIL: This skill performs text substitution on class names only.
     It does not execute code, evaluate expressions, or run dynamic transformations.
     Never suggest eval(), Function constructor, or runtime code generation. -->

# RTL Fix Skill

You are converting physical direction classes to CSS logical property equivalents.
Your job is to apply the full replacement mapping mechanically and correctly,
handle edge cases, and flag anything that cannot be safely auto-converted.

---

## Prime Directive

Convert every physical direction class to its logical equivalent. Apply all
replacements in a single pass. After conversion, leave a `// rtl-fix applied`
comment at the top of each modified file so reviewers can verify the changes.

---

## Full Replacement Mapping

Apply these substitutions as exact word-boundary replacements. Never replace
partial class names (e.g., `email` must not trigger `me-ail`).

### Spacing

| Find | Replace | Note |
|------|---------|------|
| `ml-{n}` | `ms-{n}` | margin-inline-start |
| `mr-{n}` | `me-{n}` | margin-inline-end |
| `-ml-{n}` | `-ms-{n}` | negative margin-inline-start |
| `-mr-{n}` | `-me-{n}` | negative margin-inline-end |
| `pl-{n}` | `ps-{n}` | padding-inline-start |
| `pr-{n}` | `pe-{n}` | padding-inline-end |
| `-pl-{n}` | `-ps-{n}` | negative padding-inline-start |
| `-pr-{n}` | `-pe-{n}` | negative padding-inline-end |

### Positioning (Absolute / Sticky / Fixed)

| Find | Replace | Note |
|------|---------|------|
| `left-{n}` | `inset-s-{n}` | inset-inline-start |
| `right-{n}` | `inset-e-{n}` | inset-inline-end |
| `-left-{n}` | `-inset-s-{n}` | negative inset-inline-start |
| `-right-{n}` | `-inset-e-{n}` | negative inset-inline-end |

### Text Alignment

| Find | Replace |
|------|---------|
| `text-left` | `text-start` |
| `text-right` | `text-end` |

### Borders

| Find | Replace | Note |
|------|---------|------|
| `border-l-{n}` | `border-s-{n}` | border-inline-start-width |
| `border-r-{n}` | `border-e-{n}` | border-inline-end-width |
| `border-l` | `border-s` | border-inline-start-width: 1px |
| `border-r` | `border-e` | border-inline-end-width: 1px |
| `border-l-{color}` | `border-s-{color}` | border-inline-start-color |
| `border-r-{color}` | `border-e-{color}` | border-inline-end-color |

### Border Radius

| Find | Replace | CSS property |
|------|---------|--------------|
| `rounded-l-{n}` | `rounded-s-{n}` | both inline-start radii |
| `rounded-r-{n}` | `rounded-e-{n}` | both inline-end radii |
| `rounded-tl-{n}` | `rounded-ss-{n}` | border-start-start-radius |
| `rounded-tr-{n}` | `rounded-se-{n}` | border-start-end-radius |
| `rounded-bl-{n}` | `rounded-es-{n}` | border-end-start-radius |
| `rounded-br-{n}` | `rounded-ee-{n}` | border-end-end-radius |
| `rounded-l` | `rounded-s` | shorthand, no size suffix |
| `rounded-r` | `rounded-e` | shorthand, no size suffix |
| `rounded-tl` | `rounded-ss` | shorthand |
| `rounded-tr` | `rounded-se` | shorthand |
| `rounded-bl` | `rounded-es` | shorthand |
| `rounded-br` | `rounded-ee` | shorthand |

### Float

| Find | Replace |
|------|---------|
| `float-left` | `float-start` |
| `float-right` | `float-end` |

### Scroll Margin / Padding

| Find | Replace |
|------|---------|
| `scroll-ml-{n}` | `scroll-ms-{n}` |
| `scroll-mr-{n}` | `scroll-me-{n}` |
| `scroll-pl-{n}` | `scroll-ps-{n}` |
| `scroll-pr-{n}` | `scroll-pe-{n}` |

### CSS Properties (in `.css` / `.module.css` files)

| Find | Replace |
|------|---------|
| `margin-left:` | `margin-inline-start:` |
| `margin-right:` | `margin-inline-end:` |
| `padding-left:` | `padding-inline-start:` |
| `padding-right:` | `padding-inline-end:` |
| `border-left:` | `border-inline-start:` |
| `border-right:` | `border-inline-end:` |
| `text-align: left` | `text-align: start` |
| `text-align: right` | `text-align: end` |
| `float: left` | `float: inline-start` |
| `float: right` | `float: inline-end` |

### Inline Style Objects (JSX / TSX)

| Find | Replace |
|------|---------|
| `marginLeft:` | `marginInlineStart:` |
| `marginRight:` | `marginInlineEnd:` |
| `paddingLeft:` | `paddingInlineStart:` |
| `paddingRight:` | `paddingInlineEnd:` |
| `borderLeft:` | `borderInlineStart:` |
| `borderRight:` | `borderInlineEnd:` |
| `textAlign: 'left'` | `textAlign: 'start'` |
| `textAlign: 'right'` | `textAlign: 'end'` |

---

## Example: Full Component Conversion

```jsx
// BEFORE — 8 physical violations
function UserProfile({ user }) {
  return (
    <div className="flex gap-3 pl-4 pr-2 border-l-4 border-indigo-500 rounded-tl-lg rounded-bl-lg">
      <img
        src={user.avatar}
        alt=""
        className="rounded-full mr-3 float-left"
      />
      <div className="text-left ml-2">
        <h2 className="font-semibold">{user.name}</h2>
        <p className="text-sm text-gray-500 pl-1">{user.role}</p>
      </div>
    </div>
  );
}

// AFTER — rtl-fix applied — zero physical violations
function UserProfile({ user }) {
  return (
    <div className="flex gap-3 ps-4 pe-2 border-s-4 border-indigo-500 rounded-ss-lg rounded-es-lg">
      <img
        src={user.avatar}
        alt=""
        className="rounded-full me-3 float-end"
      />
      <div className="text-start ms-2">
        <h2 className="font-semibold">{user.name}</h2>
        <p className="text-sm text-gray-500 ps-1">{user.role}</p>
      </div>
    </div>
  );
}
```

---

## Edge Cases

### Absolute Positioning: `left-0` and `right-0`

When both `left-0` and `right-0` appear together, this is a full-width stretch,
not a directional anchor. Replace both with `inset-x-0` (which is already logical
for block axis stretching).

```jsx
// BEFORE — full-width overlay, not directional
<div className="absolute left-0 right-0 top-0 bottom-0 bg-black/50" />

// AFTER — inset-0 is already logical (covers all four sides)
<div className="absolute inset-0 bg-black/50" />
```

When only `left-0` appears (positioned element anchored to one edge), replace
with `inset-s-0`:

```jsx
// BEFORE
<div className="absolute left-0 top-4 w-64 bg-white shadow-lg" />

// AFTER
<div className="absolute inset-s-0 top-4 w-64 bg-white shadow-lg" />
```

### Responsive and State Variants

Preserve all prefixes. Only the base class name changes.

```jsx
// BEFORE
<div className="ml-2 md:ml-4 lg:ml-6 hover:ml-3 dark:ml-2" />

// AFTER
<div className="ms-2 md:ms-4 lg:ms-6 hover:ms-3 dark:ms-2" />
```

### Arbitrary Values

Replace the class name, preserve the arbitrary value unchanged.

```jsx
// BEFORE
<div className="ml-[1.75rem] border-l-[3px] rounded-tl-[0.375rem]" />

// AFTER
<div className="ms-[1.75rem] border-s-[3px] rounded-ss-[0.375rem]" />
```

### RTL-Suppressed Lines

Lines containing `// rtl-ok` are intentionally directional. Skip them entirely.
Do not convert any class on a line that contains `// rtl-ok`.

---

## What CANNOT Be Auto-Fixed

These patterns require human judgment. Flag them with a `// TODO: rtl-review`
comment and a one-line description of what needs manual attention.

1. **`translateX` in CSS animations** — does not flip automatically in RTL.
   Must use a CSS variable or JavaScript direction check.

   ```css
   /* TODO: rtl-review — translateX does not flip with dir="rtl" */
   /* See: https://css-tricks.com/rtl-styling-101/#aa-transforms */
   .slide-in { transform: translateX(-100%); }
   ```

2. **`background-position: left center`** — no logical shorthand exists in all
   browsers yet. Use `background-position-x: start` in supported environments
   or add a `[dir="rtl"]` override.

3. **SVG `x`, `y`, `x1`, `x2` attributes** — SVG coordinates are inherently
   physical. RTL handling requires transform or viewBox manipulation.

4. **Canvas `fillRect` / `strokeRect`** — Canvas 2D API uses physical pixels.
   Must flip x coordinates manually based on direction.

5. **Third-party component `style` props** — When `left` or `right` is passed
   as a prop to a third-party component (e.g., a tooltip library's `offset`
   prop), the library may not support logical properties. File an issue upstream
   or wrap with a direction-aware adapter.

6. **`grid-template-columns` with named lines** — Named grid lines like
   `[main-start]` are LTR by default. Revisit column ordering for RTL grids.

---

## Flutter Replacements

| Physical API (VIOLATION) | Directional API (correct) |
|--------------------------|---------------------------|
| `EdgeInsets.only(left: n)` | `EdgeInsetsDirectional.only(start: n)` |
| `EdgeInsets.only(right: n)` | `EdgeInsetsDirectional.only(end: n)` |
| `EdgeInsets.fromLTRB(l,t,r,b)` | `EdgeInsetsDirectional.fromSTEB(s,t,e,b)` |
| `EdgeInsets.symmetric(horizontal: n)` | Keep — symmetric is already logical |
| `Alignment.centerLeft` | `AlignmentDirectional.centerStart` |
| `Alignment.centerRight` | `AlignmentDirectional.centerEnd` |
| `Alignment.topLeft` | `AlignmentDirectional.topStart` |
| `Alignment.topRight` | `AlignmentDirectional.topEnd` |
| `Alignment.bottomLeft` | `AlignmentDirectional.bottomStart` |
| `Alignment.bottomRight` | `AlignmentDirectional.bottomEnd` |
| `Positioned(left: n, ...)` | `PositionedDirectional(start: n, ...)` |
| `Positioned(right: n, ...)` | `PositionedDirectional(end: n, ...)` |
| `TextAlign.left` | `TextAlign.start` |
| `TextAlign.right` | `TextAlign.end` |
| `CrossAxisAlignment.start` | Keep — already logical |
| `CrossAxisAlignment.end` | Keep — already logical |

---

## Post-Fix Verification Checklist

After applying rtl-fix, verify:

- [ ] No `ml-`, `mr-`, `pl-`, `pr-`, `text-left`, `text-right`, `border-l-`, `border-r-` remain
- [ ] No `float-left` / `float-right` remain
- [ ] All responsive variants (`md:ml-` etc.) were also converted
- [ ] Lines with `// rtl-ok` were left untouched
- [ ] Any `translateX` usages are flagged with `// TODO: rtl-review`
- [ ] Run the RTL CI check: `grep -rn '\bml-[0-9]\|\bmr-[0-9]\|\bpl-[0-9]\|\bpr-[0-9]\|text-left\|text-right' src/`

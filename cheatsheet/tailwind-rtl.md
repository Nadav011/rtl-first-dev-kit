# Tailwind 4.x RTL Cheatsheet — Complete Logical Properties Reference

> The definitive reference for building bidirectional apps with Tailwind 4.x.
> Physical classes are banned. Logical classes are the only correct foundation.

**Related:** [Tailwind CSS docs — Logical Properties](https://tailwindcss.com/docs/hover-focus-and-other-states#rtl-support) | [W3C CSS Logical Properties spec](https://www.w3.org/TR/css-logical-1/)

---

## The Core Principle

Physical classes (`ml-`, `mr-`, `text-left`) are anchored to the physical screen.
They never change, regardless of document direction.

Logical classes (`ms-`, `me-`, `text-start`) are anchored to the reading direction.
They automatically flip when `dir="rtl"` is set on any ancestor element.

```
LTR (dir="ltr"):   start = left,  end = right
RTL (dir="rtl"):   start = right, end = left
```

---

## Spacing — Margin

| Physical class (NEVER) | Logical class (ALWAYS) | CSS property generated |
|-----------------------|----------------------|------------------------|
| `ml-0` | `ms-0` | `margin-inline-start: 0` |
| `ml-1` | `ms-1` | `margin-inline-start: 0.25rem` |
| `ml-2` | `ms-2` | `margin-inline-start: 0.5rem` |
| `ml-4` | `ms-4` | `margin-inline-start: 1rem` |
| `ml-8` | `ms-8` | `margin-inline-start: 2rem` |
| `ml-auto` | `ms-auto` | `margin-inline-start: auto` |
| `ml-px` | `ms-px` | `margin-inline-start: 1px` |
| `ml-[1.75rem]` | `ms-[1.75rem]` | arbitrary value |
| `-ml-4` | `-ms-4` | `margin-inline-start: -1rem` |
| `mr-0` | `me-0` | `margin-inline-end: 0` |
| `mr-1` | `me-1` | `margin-inline-end: 0.25rem` |
| `mr-2` | `me-2` | `margin-inline-end: 0.5rem` |
| `mr-4` | `me-4` | `margin-inline-end: 1rem` |
| `mr-auto` | `me-auto` | `margin-inline-end: auto` |
| `-mr-4` | `-me-4` | `margin-inline-end: -1rem` |

Already logical (no change needed):
- `mt-*` — `margin-block-start` — already logical
- `mb-*` — `margin-block-end` — already logical
- `mx-*` — `margin-inline` (both sides) — already logical
- `my-*` — `margin-block` (both sides) — already logical
- `m-*` — all four sides — already logical

---

## Spacing — Padding

| Physical class (NEVER) | Logical class (ALWAYS) | CSS property generated |
|-----------------------|----------------------|------------------------|
| `pl-0` | `ps-0` | `padding-inline-start: 0` |
| `pl-1` | `ps-1` | `padding-inline-start: 0.25rem` |
| `pl-2` | `ps-2` | `padding-inline-start: 0.5rem` |
| `pl-4` | `ps-4` | `padding-inline-start: 1rem` |
| `pl-6` | `ps-6` | `padding-inline-start: 1.5rem` |
| `pl-8` | `ps-8` | `padding-inline-start: 2rem` |
| `pl-[1.5rem]` | `ps-[1.5rem]` | arbitrary value |
| `pr-0` | `pe-0` | `padding-inline-end: 0` |
| `pr-2` | `pe-2` | `padding-inline-end: 0.5rem` |
| `pr-4` | `pe-4` | `padding-inline-end: 1rem` |
| `pr-6` | `pe-6` | `padding-inline-end: 1.5rem` |

Already logical (no change needed):
- `pt-*` — `padding-block-start`
- `pb-*` — `padding-block-end`
- `px-*` — `padding-inline` (both sides)
- `py-*` — `padding-block` (both sides)
- `p-*` — all four sides

---

## Borders

| Physical class (NEVER) | Logical class (ALWAYS) | CSS property generated |
|-----------------------|----------------------|------------------------|
| `border-l` | `border-s` | `border-inline-start-width: 1px` |
| `border-r` | `border-e` | `border-inline-end-width: 1px` |
| `border-l-0` | `border-s-0` | `border-inline-start-width: 0` |
| `border-l-2` | `border-s-2` | `border-inline-start-width: 2px` |
| `border-l-4` | `border-s-4` | `border-inline-start-width: 4px` |
| `border-l-8` | `border-s-8` | `border-inline-start-width: 8px` |
| `border-r-2` | `border-e-2` | `border-inline-end-width: 2px` |
| `border-r-4` | `border-e-4` | `border-inline-end-width: 4px` |
| `border-l-blue-500` | `border-s-blue-500` | `border-inline-start-color` |
| `border-r-gray-200` | `border-e-gray-200` | `border-inline-end-color` |

Already logical (no change needed):
- `border-t-*` / `border-b-*` — block-axis borders

---

## Positioning — Inset

In Tailwind 4.2+, use `inset-s-*` and `inset-e-*` for logical positioning.
Do not use the deprecated `start-*` / `end-*` standalone utilities.

| Physical class (NEVER) | Logical class (ALWAYS) | CSS property generated |
|-----------------------|----------------------|------------------------|
| `left-0` | `inset-s-0` | `inset-inline-start: 0` |
| `left-1` | `inset-s-1` | `inset-inline-start: 0.25rem` |
| `left-2` | `inset-s-2` | `inset-inline-start: 0.5rem` |
| `left-4` | `inset-s-4` | `inset-inline-start: 1rem` |
| `left-full` | `inset-s-full` | `inset-inline-start: 100%` |
| `left-1/2` | `inset-s-1/2` | `inset-inline-start: 50%` |
| `left-[16px]` | `inset-s-[16px]` | arbitrary value |
| `-left-4` | `-inset-s-4` | `inset-inline-start: -1rem` |
| `right-0` | `inset-e-0` | `inset-inline-end: 0` |
| `right-2` | `inset-e-2` | `inset-inline-end: 0.5rem` |
| `right-4` | `inset-e-4` | `inset-inline-end: 1rem` |
| `right-full` | `inset-e-full` | `inset-inline-end: 100%` |
| `-right-4` | `-inset-e-4` | `inset-inline-end: -1rem` |

Special case — full width stretch (both edges):

```jsx
// BEFORE — both physical edges, full width
<div className="absolute left-0 right-0 top-0" />

// AFTER — use inset-x-0 (covers both inline edges)
<div className="absolute inset-x-0 top-0" />
```

---

## Border Radius

The logical radius classes use the `ss` / `se` / `es` / `ee` naming convention:
`s` = start, `e` = end, first letter = block axis, second = inline axis.

| Physical class (NEVER) | Logical class (ALWAYS) | CSS property |
|-----------------------|----------------------|--------------|
| `rounded-l` | `rounded-s` | both inline-start corners |
| `rounded-r` | `rounded-e` | both inline-end corners |
| `rounded-l-sm` | `rounded-s-sm` | both inline-start corners, sm |
| `rounded-l-md` | `rounded-s-md` | both inline-start corners, md |
| `rounded-l-lg` | `rounded-s-lg` | both inline-start corners, lg |
| `rounded-l-xl` | `rounded-s-xl` | both inline-start corners, xl |
| `rounded-l-full` | `rounded-s-full` | both inline-start corners, full |
| `rounded-r-lg` | `rounded-e-lg` | both inline-end corners, lg |
| `rounded-tl` | `rounded-ss` | `border-start-start-radius` |
| `rounded-tl-sm` | `rounded-ss-sm` | border-start-start-radius, sm |
| `rounded-tl-md` | `rounded-ss-md` | border-start-start-radius, md |
| `rounded-tl-lg` | `rounded-ss-lg` | border-start-start-radius, lg |
| `rounded-tl-xl` | `rounded-ss-xl` | border-start-start-radius, xl |
| `rounded-tr` | `rounded-se` | `border-start-end-radius` |
| `rounded-tr-lg` | `rounded-se-lg` | border-start-end-radius, lg |
| `rounded-bl` | `rounded-es` | `border-end-start-radius` |
| `rounded-bl-lg` | `rounded-es-lg` | border-end-start-radius, lg |
| `rounded-br` | `rounded-ee` | `border-end-end-radius` |
| `rounded-br-lg` | `rounded-ee-lg` | border-end-end-radius, lg |

Already logical (no change needed):
- `rounded-t-*` — both top corners
- `rounded-b-*` — both bottom corners
- `rounded-*` — all four corners

---

## Text Alignment

| Physical class (NEVER) | Logical class (ALWAYS) | CSS generated |
|-----------------------|----------------------|---------------|
| `text-left` | `text-start` | `text-align: start` |
| `text-right` | `text-end` | `text-align: end` |

`text-center` and `text-justify` are already direction-neutral.

---

## Float

| Physical class (NEVER) | Logical class (ALWAYS) | CSS generated |
|-----------------------|----------------------|---------------|
| `float-left` | `float-start` | `float: inline-start` |
| `float-right` | `float-end` | `float: inline-end` |

`float-none` is already direction-neutral.

---

## Scroll Margin and Scroll Padding

| Physical class (NEVER) | Logical class (ALWAYS) |
|-----------------------|----------------------|
| `scroll-ml-{n}` | `scroll-ms-{n}` |
| `scroll-mr-{n}` | `scroll-me-{n}` |
| `scroll-pl-{n}` | `scroll-ps-{n}` |
| `scroll-pr-{n}` | `scroll-pe-{n}` |

---

## Flexbox — Already Logical in Tailwind 4

Flexbox direction utilities are already based on flex-direction, not physical screen axes.
When `flex-row` is combined with `dir="rtl"`, the main axis reverses automatically.

```jsx
// This is already RTL-correct — no changes needed
<div className="flex flex-row items-center justify-between gap-4">
  <Logo />
  <Nav />
  <UserMenu />
</div>
// In RTL: UserMenu appears on the left, Logo on the right — correct reading order
```

`justify-start`, `justify-end`, `items-start`, `items-end`,
`self-start`, `self-end`, `place-content-start`, `place-content-end`
are all logical — they respond to `flex-direction` and `dir`.

---

## Grid — Logical Considerations

Grid column ordering does not automatically reverse in RTL unless you use
`direction: rtl` at the grid container level or `grid-template-columns` with
logical named lines.

```jsx
// Manual RTL handling for grid layouts
<div className="grid grid-cols-[auto_1fr] rtl:grid-cols-[1fr_auto]">
  <Sidebar />
  <Main />
</div>
```

---

## Common Mistakes

### Mistake 1: `text-right` is not RTL alignment

`text-right` forces text to the physical right edge in ALL documents, regardless
of `dir`. In an LTR document inside an RTL wrapper, this is the wrong edge.

```jsx
// WRONG — text-right means "physical right", not "reading end"
<div dir="rtl">
  <p className="text-right">هذا النص محاذى لليمين الفعلي فقط</p>
</div>

// CORRECT — text-end means "end of the reading direction"
<div dir="rtl">
  <p className="text-end">هذا النص محاذى لنهاية اتجاه القراءة</p>
</div>
```

### Mistake 2: `dir="rtl"` alone is not enough

Setting `dir="rtl"` on the `<html>` element tells the browser the document reading
direction, but it does NOT flip physical margin/padding/border classes. Those remain
physically anchored.

```html
<!-- WRONG — dir="rtl" is set, but ml-4 is still physically left -->
<html dir="rtl" lang="he">
  <div class="ml-4">תוכן</div> <!-- margin is on the LEFT = trailing edge in RTL -->
</html>

<!-- CORRECT — logical class responds to dir="rtl" -->
<html dir="rtl" lang="he">
  <div class="ms-4">תוכן</div> <!-- margin-inline-start = RIGHT in RTL -->
</html>
```

### Mistake 3: Using `rtl:ml-4` as an override

Some developers add `rtl:ml-4` to flip the margin for RTL, doubling the maintenance burden.
This is an anti-pattern — you end up with two classes to maintain instead of one.

```jsx
// ANTI-PATTERN — duplicated logic, easy to get out of sync
<div className="ml-4 rtl:mr-4 rtl:ml-0">...</div>

// CORRECT — single logical class, zero overrides
<div className="ms-4">...</div>
```

### Mistake 4: Forgetting to flip horizontal icons

A `ChevronRight` pointing right means "go forward" in LTR. In RTL, "forward" is
to the left — so the chevron must flip.

```jsx
// WRONG — arrow always points right, meaning changes in RTL
<button>
  הבא <ChevronRight />
</button>

// CORRECT — arrow flips to point left in RTL, preserving "forward" meaning
<button>
  הבא <ChevronRight className="rtl:rotate-180" />
</button>
```

### Mistake 5: Not wrapping LTR islands in mixed BiDi content

Numbers, phone numbers, percentages, dates, and code strings are always LTR,
even in RTL documents. Without explicit `dir="ltr"`, the Unicode BiDi algorithm
may render them incorrectly.

```jsx
// WRONG — browser BiDi algorithm may render this incorrectly
<p>מחיר: ₪1,234.56</p>

// CORRECT — explicit LTR island
<p>
  מחיר: <span dir="ltr" className="font-mono tabular-nums">₪1,234.56</span>
</p>
```

---

## Complete Hebrew UI Component Example

This is a full form component with all logical properties applied correctly.

```tsx
// rtl-fix applied — all direction classes use logical properties
// Works in he-IL, ar-SA, ar-EG, and en-US with zero overrides

interface FormFieldProps {
  label: string;
  value: string;
  onChange: (v: string) => void;
  error?: string;
  required?: boolean;
}

export function FormField({ label, value, onChange, error, required }: FormFieldProps) {
  return (
    <div className="flex flex-col gap-1.5">
      {/* Label row — icon on the reading-start side */}
      <label className="flex items-center gap-1.5 text-sm font-medium text-gray-700">
        {required && (
          <span className="text-red-500 text-xs" aria-hidden="true">*</span>
        )}
        {label}
      </label>

      {/* Input with logical padding */}
      <div className="relative">
        <input
          type="text"
          value={value}
          onChange={(e) => onChange(e.target.value)}
          className={[
            "w-full rounded-lg border px-3 py-2",          // px is symmetric — fine
            "ps-3 pe-10",                                   // logical padding for icon space
            "text-start",                                   // text aligns to reading direction
            "border-gray-300 bg-white",
            "focus:border-blue-500 focus:ring-2 focus:ring-blue-500/20",
            error ? "border-red-400" : "",
          ].join(" ")}
        />

        {/* Validation icon — positioned on reading-END side */}
        {error && (
          <div className="pointer-events-none absolute inset-e-3 top-1/2 -translate-y-1/2">
            <AlertCircle className="h-4 w-4 text-red-400" />
          </div>
        )}
      </div>

      {/* Error message — text aligns to reading start */}
      {error && (
        <p className="text-sm text-red-600 text-start" role="alert">
          {error}
        </p>
      )}
    </div>
  );
}

// Usage in a Hebrew-first form
export function CheckoutForm() {
  return (
    <form
      className="mx-auto max-w-md rounded-xl border border-gray-200 p-6 shadow-sm"
      dir="rtl"
      lang="he"
    >
      <h2 className="mb-6 text-xl font-bold text-gray-900 text-start">פרטי תשלום</h2>

      {/* Navigation tabs — border on reading-start side for active state */}
      <div className="mb-6 flex border-b border-gray-200">
        <button className="border-b-2 border-blue-500 pb-2 pe-4 ps-0 text-sm font-medium text-blue-600">
          כרטיס אשראי
        </button>
        <button className="pb-2 pe-4 ps-4 text-sm text-gray-500 hover:text-gray-700">
          PayPal
        </button>
      </div>

      <div className="flex flex-col gap-4">
        <FormField label="שם מלא" value="" onChange={() => {}} required />

        {/* Card number — this is LTR content embedded in RTL form */}
        <div className="flex flex-col gap-1.5">
          <label className="text-sm font-medium text-gray-700">מספר כרטיס</label>
          <input
            type="text"
            dir="ltr"           {/* rtl-ok — credit card numbers are always LTR */}
            inputMode="numeric"
            placeholder="0000 0000 0000 0000"
            className="w-full rounded-lg border border-gray-300 px-3 py-2 text-start font-mono"
          />
        </div>

        <div className="grid grid-cols-2 gap-4">
          <FormField label="תוקף" value="" onChange={() => {}} />
          <FormField label="CVV" value="" onChange={() => {}} />
        </div>

        {/* Summary row — price on inline-end side */}
        <div className="flex items-center justify-between rounded-lg bg-gray-50 px-4 py-3">
          <span className="text-sm font-medium text-gray-700">סה"כ לתשלום</span>
          {/* Price is LTR — wrap in dir="ltr" island */}
          <span dir="ltr" className="font-mono font-semibold text-gray-900 tabular-nums">
            ₪ 1,249.00
          </span>
        </div>

        <button
          type="submit"
          className="flex w-full items-center justify-center gap-2 rounded-lg bg-blue-600 px-4 py-3 font-semibold text-white hover:bg-blue-700"
        >
          <LockIcon className="h-4 w-4" />
          <span>לתשלום מאובטח</span>
          {/* Chevron flips in RTL — points left to indicate "proceed forward" */}
          <ChevronRight className="h-4 w-4 rtl:rotate-180" />
        </button>
      </div>
    </form>
  );
}
```

---

## Flutter Equivalents

### EdgeInsets → EdgeInsetsDirectional

```dart
// WRONG — physical
EdgeInsets.only(left: 16, right: 8)
EdgeInsets.fromLTRB(16, 8, 8, 8)

// CORRECT — directional
EdgeInsetsDirectional.only(start: 16, end: 8)
EdgeInsetsDirectional.fromSTEB(16, 8, 8, 8)  // start, top, end, bottom

// Already symmetric — no change needed
EdgeInsets.symmetric(horizontal: 16, vertical: 8)
EdgeInsets.all(12)
```

### Alignment → AlignmentDirectional

```dart
// WRONG — physical
Alignment.centerLeft
Alignment.centerRight
Alignment.topLeft
Alignment.topRight
Alignment.bottomLeft
Alignment.bottomRight

// CORRECT — directional
AlignmentDirectional.centerStart
AlignmentDirectional.centerEnd
AlignmentDirectional.topStart
AlignmentDirectional.topEnd
AlignmentDirectional.bottomStart
AlignmentDirectional.bottomEnd

// Already neutral — no change needed
Alignment.center
Alignment.topCenter
Alignment.bottomCenter
```

### Positioned → PositionedDirectional

```dart
// WRONG — physical
Positioned(left: 16, top: 8, child: widget)
Positioned(right: 8, bottom: 16, child: widget)

// CORRECT — directional
PositionedDirectional(start: 16, top: 8, child: widget)
PositionedDirectional(end: 8, bottom: 16, child: widget)
```

### TextAlign

```dart
// WRONG
Text('שלום', textAlign: TextAlign.left)
Text('Hello', textAlign: TextAlign.right)

// CORRECT
Text('שלום', textAlign: TextAlign.start)
Text('Hello', textAlign: TextAlign.end)

// Already neutral — no change needed
Text('center', textAlign: TextAlign.center)
Text('justify', textAlign: TextAlign.justify)
```

### Wrapping with Directionality

```dart
// Set direction once at the app or page root
Directionality(
  textDirection: TextDirection.rtl,  // or from locale
  child: MyApp(),
)

// Or derive from locale in MaterialApp
MaterialApp(
  locale: const Locale('he', 'IL'),
  // Flutter derives TextDirection from locale automatically
  home: MyHomePage(),
)
```

---

## Quick Reference Card

```
NEVER use:          ALWAYS use:
ml-*        →       ms-*
mr-*        →       me-*
pl-*        →       ps-*
pr-*        →       pe-*
left-*      →       inset-s-*
right-*     →       inset-e-*
text-left   →       text-start
text-right  →       text-end
border-l-*  →       border-s-*
border-r-*  →       border-e-*
rounded-l-* →       rounded-s-*
rounded-r-* →       rounded-e-*
rounded-tl-*→       rounded-ss-*
rounded-tr-*→       rounded-se-*
rounded-bl-*→       rounded-es-*
rounded-br-*→       rounded-ee-*
float-left  →       float-start
float-right →       float-end
```

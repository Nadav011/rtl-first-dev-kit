# Tailwind RTL Mapping

Use logical utilities only. Treat any physical-direction utility as a regression.

## Core Mapping

| Never | Use Instead | Purpose |
|---|---|---|
| `ml-*` | `ms-*` | margin-inline-start |
| `mr-*` | `me-*` | margin-inline-end |
| `pl-*` | `ps-*` | padding-inline-start |
| `pr-*` | `pe-*` | padding-inline-end |
| `left-*` | `inset-s-*` | logical start inset |
| `right-*` | `inset-e-*` | logical end inset |
| `text-left` | `text-start` | logical text alignment |
| `text-right` | `text-end` | logical text alignment |
| `rounded-l-*` | `rounded-s-*` | logical start corners |
| `rounded-r-*` | `rounded-e-*` | logical end corners |
| `border-l-*` | `border-s-*` | logical start border |
| `border-r-*` | `border-e-*` | logical end border |

## Examples

```tsx
<aside className="border-s ps-4 pe-3 ms-2 rounded-s-xl text-start" />
```

```tsx
<button className="inset-e-4 ps-3 pe-4 rounded-e-lg" />
```

## Directional Icons

Only rotate horizontal icons in RTL:

```tsx
<ChevronLeft className="rtl:rotate-180" />
```

Do not rotate vertical icons such as `ChevronUp` or `ArrowDown`.

## Numbers And Mixed Text

Wrap numeric spans with `dir=\"ltr\"` when embedded inside Hebrew or Arabic UI:

```tsx
<span>
  יעד חודשי: <span dir="ltr">42%</span>
</span>
```

## Review Rule

If a diff introduces `ml-`, `mr-`, `pl-`, `pr-`, `left-`, or `right-`, stop the review and convert it before merge.

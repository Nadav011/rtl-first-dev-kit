# Biome RTL Policy Sketch

This file defines the practical policy the repo should enforce, even before a custom Biome plugin exists.

## Scope

Block physical-direction utilities and APIs in:

- `*.tsx`
- `*.jsx`
- `*.ts`
- `*.js`
- `*.css`

## What To Flag

- Tailwind classes: `ml-`, `mr-`, `pl-`, `pr-`, `left-`, `right-`, `text-left`, `text-right`
- CSS properties: `margin-left`, `margin-right`, `padding-left`, `padding-right`, `left`, `right`
- JSX class strings that embed any of the above

## Expected Autofix Direction

- `ml-` -> `ms-`
- `mr-` -> `me-`
- `pl-` -> `ps-`
- `pr-` -> `pe-`
- `left-*` -> `inset-s-*`
- `right-*` -> `inset-e-*`
- `text-left` -> `text-start`
- `text-right` -> `text-end`

## False Positive Policy

- Ignore comments and documentation unless the lint mode is explicitly `docs`.
- Allow one-off documented escapes only with a marker such as `rtl-ok`.
- Do not silently autofix ambiguous icon or animation cases.

## Review Workflow

1. Lint blocks merge on physical-direction regressions.
2. Reviewer checks that autofixes did not invert meaning.
3. Any exception must include a short comment explaining why logical utilities could not be used.

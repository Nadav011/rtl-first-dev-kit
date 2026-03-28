---
name: rtl-first-dev-kit
description: Local scaffold for an RTL-first toolkit covering Tailwind, Flutter, Next.js, Biome, and CI patterns.
---

# RTL First Dev Kit

Use this skill when the goal is to build or audit RTL-safe foundations across web and Flutter codebases.

## Status

This is still a local draft, but it now includes real first-pass content instead of skeleton placeholders.

## Focus areas

- Tailwind logical properties and directional utilities
- Flutter directional layout primitives
- Next.js `dir` propagation and route structure
- RTL linting and policy enforcement
- CI validation examples

## Working Rules

1. Prefer logical properties and directional APIs over left/right APIs.
2. Keep examples short enough to transplant directly into a repo.
3. Block physical-direction regressions in both code review and CI.
4. Treat mixed Hebrew/English text and numeric rendering as first-class cases.

## Primary Files

- [mapping.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/tailwind/mapping.md)
- [patterns.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/flutter/patterns.md)
- [proxy-and-dir.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/nextjs/proxy-and-dir.md)
- [policy.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/biome/policy.md)
- [rtl-validator-workflow.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/ci/rtl-validator-workflow.md)

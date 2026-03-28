# RTL First Dev Kit

Local draft for a reusable RTL toolkit covering Tailwind, Flutter, Next.js, Biome policy, and CI validation.

## Status

This repository now contains a first practical pass of the core guidance. It is still a draft, but the main files are no longer placeholders only.

## What It Covers

- Tailwind logical-direction mapping and examples
- Flutter directional primitives and common replacement patterns
- Next.js 16 `proxy.ts` and `dir` propagation examples
- Biome policy rules for blocking physical-direction regressions
- CI workflow sketch for RTL validation in pull requests

## Layout

```text
rtl-first-dev-kit/
├── README.md
├── SKILL.md
├── tailwind/
│   ├── README.md
│   ├── logical-properties.md
│   └── mapping.md
├── flutter/
│   ├── README.md
│   ├── directional-primitives.md
│   └── patterns.md
├── nextjs/
│   ├── README.md
│   ├── dir-propagation.md
│   └── proxy-and-dir.md
├── biome/
│   ├── README.md
│   ├── policy.md
│   └── rtl-policy.md
└── ci/
    ├── README.md
    ├── rtl-validator-workflow.md
    └── workflow-sketch.md
```

## Intended Use

Use this repo as a source pack for teams that already know they need RTL-safe defaults but want one place to copy patterns from.

Start here:

1. Tailwind apps: [mapping.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/tailwind/mapping.md)
2. Flutter apps: [patterns.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/flutter/patterns.md)
3. Next.js apps: [proxy-and-dir.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/nextjs/proxy-and-dir.md)
4. Repo policy: [policy.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/biome/policy.md)
5. CI enforcement: [rtl-validator-workflow.md](/home/nadavcohen/Desktop/rtl-first-dev-kit/ci/rtl-validator-workflow.md)

## Draft Constraints

- No publication claim yet
- No benchmark or adoption claim yet
- No promise of automated fixes beyond the policy sketches already written

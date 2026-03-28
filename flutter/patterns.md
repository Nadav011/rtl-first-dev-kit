# Flutter RTL Patterns

Prefer directional APIs everywhere. Left/right APIs should be treated as migration targets.

## Direct Replacements

| Never | Use Instead |
|---|---|
| `EdgeInsets.only(left: 16)` | `EdgeInsetsDirectional.only(start: 16)` |
| `EdgeInsets.symmetric(horizontal: 12)` | keep, this is already logical |
| `Alignment.topLeft` | `AlignmentDirectional.topStart` |
| `Alignment.centerRight` | `AlignmentDirectional.centerEnd` |
| `Positioned(left: 0)` | `PositionedDirectional(start: 0)` |
| `BorderRadius.only(topLeft: ...)` | `BorderRadiusDirectional.only(topStart: ...)` |
| `TextAlign.left` | `TextAlign.start` |
| `TextAlign.right` | `TextAlign.end` |

## Layout Example

```dart
Padding(
  padding: const EdgeInsetsDirectional.only(start: 16, end: 12),
  child: Align(
    alignment: AlignmentDirectional.centerStart,
    child: Text(
      'שלום עולם',
      textAlign: TextAlign.start,
    ),
  ),
)
```

## Stack Example

```dart
Stack(
  children: [
    PositionedDirectional(
      start: 12,
      top: 8,
      child: Chip(label: const Text('חדש')),
    ),
  ],
)
```

## Border And Shape Example

```dart
Container(
  decoration: const BoxDecoration(
    borderRadius: BorderRadiusDirectional.only(
      topStart: Radius.circular(20),
      topEnd: Radius.circular(12),
    ),
  ),
)
```

## Review Rule

If a widget diff introduces `left`, `right`, `topLeft`, `topRight`, `bottomLeft`, or `bottomRight`, replace them with directional APIs before merge.

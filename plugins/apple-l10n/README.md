# Apple Localization

Write localizable SwiftUI strings and translate Xcode String Catalogs.

## Skills

### translate-strings

Adds a new language, translates missing strings, or updates existing translations in `Localizable.xcstrings`. Needs `python3`.

```
/apple-l10n:translate-strings Japanese
/apple-l10n:translate-strings Chinese (Simplified)
```

### swiftui-strings

Patterns for `String(localized:)` in SwiftUI views: key naming, translator comments, what to skip. Applies automatically when editing views with user-facing strings.

```swift
// Before
Text("Keep track of all your important events")

// After
Text(String(localized: "events.list.subtitle",
           defaultValue: "Keep track of all your important events"))
```

## Installation

In Claude Code, add the marketplace and install the plugin:

```
/plugin marketplace add azamath/skills
/plugin install apple-l10n@azamath
```

Or from the terminal:

```bash
claude plugin marketplace add azamath/skills
```

```bash
claude plugin install apple-l10n@azamath
```

To get updates:

```bash
claude plugin marketplace update azamath
```

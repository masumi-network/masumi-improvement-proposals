# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This format defines input validation using a simplified JSON schema. The goal is to make it easy to define various input types and their rules, while keeping implementation flexible.

> Developers only need to define the schema. The frontend handles display logic.

---

## Example Schema

```json
{
  "id": "example-input",
  "type": "string|number|boolean|option|none",
  "name": "Example name",
  "data": {
    "values": ["Option 1", "Option 2", "Option 3"],
    "placeholder": "test 123",
    "description": "This is an example input (optional)"
  },
  "validations": [
    {
      "validation": "min|max|format|required",
      "value": "number|email|url|nonempty|integer|boolean"
    }
  ]
}
```

---

## Supported Types

### `none`

Displays static text. No validations applied.

```json
{ "type": "none", "name": "Informational Header" }
```

### `string`

Basic text input with optional format or length validations.

```json
{
  "type": "string",
  "name": "Email",
  "validations": [
    { "validation": "min", "value": "5" },
    { "validation": "format", "value": "email" }
  ]
}
```

### `number`

Numeric input, supports `min`, `max`, and `format`.

```json
{
  "type": "number",
  "name": "Age",
  "data": { "description": "User's age" },
  "validations": [
    { "validation": "min", "value": "18" },
    { "validation": "format", "value": "integer" }
  ]
}
```

### `boolean`

Boolean switch or checkbox.

```json
{ "type": "boolean", "name": "Include NGOs" }
```

### `option`

Select one or more from predefined values.

**Multiple:**

```json
{
  "type": "option",
  "name": "Company Type",
  "data": {
    "values": ["AG", "GmbH", "UG"]
  }
}
```

**Single:**

```json
{
  "type": "option",
  "name": "Company Type",
  "data": {
    "values": ["AG", "GmbH", "UG"]
  },
  "validations": [
    { "validation": "min", "value": "1" },
    { "validation": "max", "value": "1" }
  ]
}
```

---

## Validation Rules

| Validation | Purpose                                       | Applies to             |
|------------|-----------------------------------------------|------------------------|
| `min`      | Minimum value, length, or options selected    | string, number, option |
| `max`      | Maximum value, length, or options selected    | string, number, option |
| `format`   | Checks for specific format (e.g., email)      | string                 |
| `required` | Field must be filled                          | all                    |

Multiple validations are ANDed. For example:

```json
[
  { "validation": "min", "value": "5" },
  { "validation": "min", "value": "10" }
]
```

Requires value ≥ 10.

Avoid conflicting rules (e.g., `format: email` with `max: 2`).

---

## Format Types

- `email`: Must be a valid email
- `url`: Must be a valid URL
- `integer`: Must be a whole number
- `nonempty`: Field must not be blank

---

## Data Field

Use `data` for input-specific config like:

```json
"data": {
  "values": ["Option 1", "Option 2"],
  "description": "Choose an option",
  "placeholder": "Select..."
}
```

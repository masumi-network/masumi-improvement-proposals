# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This document outlines the standard format used for input validation based on a simplified JSON schema. The goal is to balance ease of use with a wide range of options.

---

## Schema Structure

Here’s a complete example demonstrating all supported properties and validation rules:

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

## Field Definitions

### `id`

- Description: Unique identifier for the input.
- Purpose: Used to link data and fields. Must be unique within a form.

### `type`

- Description: Declares the input type.
- Accepted Values: `string`, `number`, `boolean`, `option`, `none`
- See: [Supported Types](#supported-types)

### `name`

- Description: Display name of the input. Used for UI display.

### `data`

- Description: Input-specific configuration.
- See: [Data Field](#data-field)

### `validations`

- Description: Array of validation rules.
- See: [Validation Types](#validation-types)

---

## Supported Types

### `none` (Informational Only)

Used to display static text such as headers or paragraphs. No validations are applied.

```json
{
  "type": "none",
  "name": "Informational Block"
}
```

---

### `string`

Basic text input with optional validations.

```json
{
  "type": "string",
  "name": "Email",
  "validations": [
    { "validation": "min", "value": "5" },
    { "validation": "max", "value": "55" },
    { "validation": "format", "value": "email" }
  ]
}
```

---

### `number`

Numeric input with optional validations and descriptions.

```json
{
  "type": "number",
  "name": "Age",
  "data": { "description": "User's age in years" },
  "validations": [
    { "validation": "min", "value": "18" },
    { "validation": "max", "value": "120" },
    { "validation": "format", "value": "integer" }
  ]
}
```

---

### `boolean`

Binary toggle input.

```json
{
  "type": "boolean",
  "name": "Include NGOs"
}
```

---

### `option`

Used to allow single or multiple selections from predefined values.

#### Multiple Selections

```json
{
  "type": "option",
  "name": "Company type",
  "data": {
    "description": "Select legal entities to analyze",
    "values": ["AG", "GmbH", "UG"]
  }
}
```

#### Single Selection

```json
{
  "type": "option",
  "name": "Company type",
  "data": {
    "description": "Select a legal entity to analyze",
    "values": ["AG", "GmbH", "UG"]
  },
  "validations": [
    { "validation": "min", "value": "1" },
    { "validation": "max", "value": "1" }
  ]
}
```

---

## Validation Types

| Validation | Description                                                                 | Applies To               | Default  |
|------------|-----------------------------------------------------------------------------|--------------------------|----------|
| `min`      | Minimum characters (string), value (number), or options selected (option)  | string, number, option   | —        |
| `max`      | Maximum characters, value, or selected options                              | string, number, option   | —        |
| `format`   | Validates format such as email or URL                                       | string                   | —        |
| `required` | Marks input as mandatory (true/false)                                       | all types                | false    |

### Notes

- Validations are evaluated using logical AND. Multiple `min` or `max` rules will be applied sequentially.
- Avoid conflicting validations, e.g., `format: email` with `max: 2`.

```json
[
  { "validation": "min", "value": "5" },
  { "validation": "min", "value": "10" }
]
```

The above example validates that the input is at least 10 (both validations apply).

---

## Format Types

The following formats are supported:

- `email` – Validates email addresses
- `url` – Validates URLs
- `integer` – Input must be an integer
- `nonempty` – Field must not be empty

---

## Data Field

The `data` field contains rendering and helper information used by the frontend. It varies based on input type.

### For `option` Type

```json
"data": {
  "values": ["Option 1", "Option 2", "Option 3"]
}
```

### Shared Across All Types

**Description Field:**

```json
"data": {
  "description": "This is an example input"
}
```

**Placeholder Field:**

```json
"data": {
  "placeholder": "Please enter your email"
}
```

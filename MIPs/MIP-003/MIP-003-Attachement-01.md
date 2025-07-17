# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This document describes the validation standard format used for input types. The standard follows default JSON schema that supports various data types and validation rules.

We try to keep it as simple as possible, but still offer a wide range of options.

By default all fields are required, you can make the optional however see the [Validation Types](#validation-types)

(As a developer you do not have to care about the right display types, just define the schema, we handle the rest.)

# Format and fields

Below is an example with all possible options to define an input. (possible types are separated by '|' see [Supported Types](#supported-types) and [Validation Types](#validation-types) for more details)

```json
{
  "id": "example-input",

  "type": "string|textarea|number|boolean|optional|none",

  "name": "Example name",

  "data": {
    // Type-specific configuration like
    "values": ["Option 1", "Option 2", "Option 3"],
    "placeholder": "test 123 (optional)",
    "description": "This is an example input (optional)"
  },

  "validations": [
    {
      "validation": "min|max|format",

      "value": "number|email|url|nonempty|integer"
    },
    {
      "validation": "min|max|format|optional",
      "value": "number|email|url|nonempty|integer|boolean"
    }
  ]
}
```

## id

The `id` field is used to identify the input. This is used to link the input to the correct field in the data, it should be unique within the form.

## type

The `type` field is used to define the type of the input. This is used to determine the correct rendering and validation for the input see [Supported Types](#supported-types) for more details.

## name

The `name` field is used to display the name of the input

## data

The `data` field is used to provide additional data. This is specific to the input type and will be used to render the input accordingly see [Data Field](#data-field) for more details.

## validations

The `validations` field is used to provide validation rules. This is specific to the input type and will be used to validate the input accordingly see [Validation Types](#validation-types) for more details.

# Supported Types

## None

This is a special type that is used to display additional text and information.
(Note all validations will be ignored)

```json
{
  "type": "none",

  "name": "None"
}
```

The name will be displayed as a header and the description as a paragraph. Right now no additional rendering options are supported, but support might be added in the future read about the [Data Field](#data-field) for more details.

## String

```json
{
  "type": "string",

  "name": "Email",

  "validations": [
    {
      "validation": "min",

      "value": "5"
    },

    {
      "validation": "max",

      "value": "55"
    },

    {
      "validation": "format",

      "value": "email"
    }
  ]
}
```

## Textarea

```json
{
  "type": "textarea",

  "name": "Sentences",

  "validations": [
    {
      "validation": "min",

      "value": "5"
    },

    {
      "validation": "max",

      "value": "55"
    }
  ]
}
```


## Number

```json
{
  "type": "number",

  "name": "Age",

  "data": {
    "description": "User's age in years (optional)"
  },

  "validations": [
    {
      "validation": "min",

      "value": "18"
    },

    {
      "validation": "max",

      "value": "120"
    },

    {
      "validation": "format",

      "value": "integer"
    }
  ]
}
```

## Boolean

```json
{
  "type": "boolean",

  "name": "Include NGOs"
}
```

## Option

This can be used to let the user select multiple or a single value. Ensure to provide a list of values to select in the `data` field. Also see [Data Field](#data-field) for more details.

### Multiple Values

This example allows the user to select multiple values.

```json
{
  "type": "option",

  "name": "Company type",

  "data": {
    "description": "Please select the legal entities to analyze",

    "values": ["AG", "GmbH", "UG"]
  }
}
```

### Single Value

This example requires exactly 1 value to be selected.

```json
{
  "type": "option",

  "name": "Company type",

  "data": {
    "description": "Please select the legal entity to analyze",

    "values": ["AG", "GmbH", "UG"]
  },

  "validations": [
    {
      "validation": "min",

      "value": "1"
    },

    {
      "validation": "max",

      "value": "1"
    }
  ]
}
```

# Validation Types

| Validation | Description                                                                                                 | Applicable Types                                      | Default Value      |
| ---------- | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | ------------------ |
| `min`      | Minimum length (string), minimum value (number) or minimum amount of options to select (option). Inclusive. | string, number                                        | -                  |
| `max`      | Maximum length (string), maximum value (number) or maximum amount of options to select (option). Inclusive. | string, number                                        | -                  |
| `format`   | Specific format (email, url, etc.)                                                                          | string, (url, email, nonempty), <br>number (integer), | -                  |
| `optional` | The field is not required. By default all fields are required                                               | all                                                   | false (if not set) |

Please note that the by default there are no optional validations, also meaning that all fields are required.
This means that if you do not set a validation, the field will not be limited. In contrast to this, if you set validations multiple times, all instances of the validation will be applied, meaning that validations follow a logical AND. To clarify this, here are some examples:

```json
{
  "validation": "min",
  "value": "5"
},
{
  "validation": "min",
  "value": "10"
}
```

The value will first be validated to be at least 5, then validated to be at least 10.

(Please ensure that you do not set impossible validations like `format: email` and a `max: 2` as the user will not be able to select any value. Also ensure that the value you receive in your agent is validated to your needs and handled as untrusted data. This schema is a suggestions to a frontend on how to display inputs)

# Format Types

The following format types are supported and work across `string` types:

- `email` - Validates that the input is a valid email address.
- `url` - Validates that the input is a valid URL.
- `nonempty` - Validates that the input is not empty.

The following format types are supported and work across `number` types:

- `integer` - Validates that the input is an integer.


# Data Field

The `data` field is used to provide additional data. This is specific to the input type and will be used to render the input accordingly.

## Option

The `option` type requires a list of values to be provided in the `data` field. This list will be used to render the input as a dropdown or list of checkboxes.

```json
data: {
  "values": ["Option 1", "Option 2", "Option 3"]
}
```

## All

The `description` field is used to provide a description for the input. This is specific to the input type and will be used to render the input accordingly.

```json
data: {
  "description": "This is an example input"
}
```

The `placeholder` field is used to provide a placeholder text for the input. This is specific to the input type and will be used to render the input accordingly.

```json
data: {
  "placeholder": "Please enter your email"
}
```


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

  "type": "text|textarea|number|boolean|option|none|email|password|tel|url|date|datetime-local|time|month|week|color|range|file|hidden|search|button|checkbox|radio|image|reset|submit",

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

## Text

```json
{
  "type": "text",

  "name": "Username",

  "validations": [
    {
      "validation": "min",

      "value": "3"
    },

    {
      "validation": "max",

      "value": "20"
    },

    {
      "validation": "format",

      "value": "nonempty"
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

## Email

```json
{
  "type": "email",

  "name": "Contact Email",

  "data": {
    "placeholder": "user@example.com"
  },

  "validations": [
    {
      "validation": "format",
      "value": "email"
    }
  ]
}
```

## Password

```json
{
  "type": "password",

  "name": "Account Password",

  "data": {
    "description": "Must be at least 8 characters"
  },

  "validations": [
    {
      "validation": "min",
      "value": "8"
    },
    {
      "validation": "max",
      "value": "128"
    }
  ]
}
```

## Tel

```json
{
  "type": "tel",

  "name": "Phone Number",

  "data": {
    "placeholder": "+1-234-567-8900",
    "description": "Include country code"
  }
}
```

## URL

```json
{
  "type": "url",

  "name": "Website URL",

  "data": {
    "placeholder": "https://example.com"
  },

  "validations": [
    {
      "validation": "format",
      "value": "url"
    }
  ]
}
```

## Date

```json
{
  "type": "date",

  "name": "Birth Date",

  "validations": [
    {
      "validation": "min",
      "value": "1900-01-01"
    },
    {
      "validation": "max",
      "value": "2024-12-31"
    }
  ]
}
```

## Datetime-local

```json
{
  "type": "datetime-local",

  "name": "Appointment Time",

  "data": {
    "description": "Select date and time for your appointment"
  }
}
```

## Time

```json
{
  "type": "time",

  "name": "Preferred Time",

  "validations": [
    {
      "validation": "min",
      "value": "09:00"
    },
    {
      "validation": "max",
      "value": "17:00"
    }
  ]
}
```

## Month

```json
{
  "type": "month",

  "name": "Billing Month",

  "data": {
    "description": "Select month and year"
  }
}
```

## Week

```json
{
  "type": "week",

  "name": "Week Selection",

  "validations": [
    {
      "validation": "min",
      "value": "2024-W01"
    }
  ]
}
```

## Color

```json
{
  "type": "color",

  "name": "Theme Color",

  "data": {
    "description": "Choose your preferred color",
    "default": "#1a73e8"
  }
}
```

## Range

```json
{
  "type": "range",

  "name": "Priority Level",

  "data": {
    "min": "1",
    "max": "10",
    "step": "1",
    "default": "5",
    "description": "Select priority from 1 (low) to 10 (high)"
  }
}
```

## File

```json
{
  "type": "file",

  "name": "Document Upload",

  "data": {
    "accept": ".pdf,.doc,.docx",
    "description": "Upload PDF or Word documents only",
    "maxSize": "10485760"
  }
}
```

## Hidden

```json
{
  "type": "hidden",

  "name": "Session ID",

  "data": {
    "value": "abc123xyz"
  }
}
```

## Search

```json
{
  "type": "search",

  "name": "Search Query",

  "data": {
    "placeholder": "Search for services...",
    "description": "Enter keywords to search"
  }
}
```

## Button

```json
{
  "type": "button",

  "name": "Calculate Total",

  "data": {
    "value": "Calculate",
    "action": "calculate_total"
  }
}
```

## Checkbox

```json
{
  "type": "checkbox",

  "name": "Terms and Conditions",

  "data": {
    "description": "I agree to the terms and conditions",
    "default": false
  }
}
```

## Radio

```json
{
  "type": "radio",

  "name": "Payment Method",

  "data": {
    "values": ["Credit Card", "PayPal", "Bank Transfer"],
    "default": "Credit Card"
  }
}
```

## Image

```json
{
  "type": "image",

  "name": "Submit Button",

  "data": {
    "src": "/images/submit-button.png",
    "alt": "Submit Form",
    "width": "200",
    "height": "50"
  }
}
```

## Reset

```json
{
  "type": "reset",

  "name": "Reset Form",

  "data": {
    "value": "Clear All"
  }
}
```

## Submit

```json
{
  "type": "submit",

  "name": "Submit Form",

  "data": {
    "value": "Submit Application"
  }
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

| Validation | Description                                                                                                 | Applicable Types                                                                                    | Default Value      |
| ---------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------ |
| `min`      | Minimum length (text/password/tel/search), minimum value (number/range), minimum date/time, or minimum amount of options to select (option/checkbox/radio). Inclusive. | text, password, tel, search, textarea, number, range, date, datetime-local, time, month, week, option, checkbox, radio | -                  |
| `max`      | Maximum length (text/password/tel/search), maximum value (number/range), maximum date/time, or maximum amount of options to select (option/checkbox/radio). Inclusive. | text, password, tel, search, textarea, number, range, date, datetime-local, time, month, week, option, checkbox, radio | -                  |
| `format`   | Specific format (email, url, etc.)                                                                          | text, email, url, tel, search, (url, email, nonempty, tel-pattern), <br>number (integer)         | -                  |
| `optional` | The field is not required. By default all fields are required                                               | all                                                                                                 | false (if not set) |

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

The following format types are supported and work across `text` and related text input types:

- `email` - Validates that the input is a valid email address (automatically applied to `email` type).
- `url` - Validates that the input is a valid URL (automatically applied to `url` type).
- `nonempty` - Validates that the input is not empty.
- `tel-pattern` - Validates that the input matches a telephone number pattern (can be used with `tel` type).

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

## All Types

The following fields can be used with most input types:

### description
Provides additional context or instructions for the input.

```json
data: {
  "description": "This is an example input"
}
```

### placeholder
Provides placeholder text for text-based inputs (text, textarea, email, password, tel, url, search).

```json
data: {
  "placeholder": "Please enter your email"
}
```

### default
Sets a default value for the input.

```json
data: {
  "default": "default value"
}
```

## Type-Specific Data Fields

### Range
The `range` type supports additional configuration:

```json
data: {
  "min": "0",
  "max": "100",
  "step": "5",
  "default": "50"
}
```

### File
The `file` type supports file restrictions:

```json
data: {
  "accept": ".pdf,.doc,.docx",
  "maxSize": "10485760",  // in bytes
  "multiple": true
}
```

### Hidden
The `hidden` type requires a value:

```json
data: {
  "value": "hidden-value"
}
```

### Color
The `color` type can have a default color:

```json
data: {
  "default": "#ff0000"
}
```

### Button, Submit, Reset
These button types support custom text:

```json
data: {
  "value": "Click Me",
  "action": "custom_action"  // for button type
}
```

### Checkbox
The `checkbox` type supports default checked state:

```json
data: {
  "default": true,
  "description": "Check to agree"
}
```

### Radio
The `radio` type requires values like option:

```json
data: {
  "values": ["Option 1", "Option 2"],
  "default": "Option 1"
}
```

### Image
The `image` type requires image configuration:

```json
data: {
  "src": "/path/to/image.png",
  "alt": "Alternative text",
  "width": "100",
  "height": "50"
}
```


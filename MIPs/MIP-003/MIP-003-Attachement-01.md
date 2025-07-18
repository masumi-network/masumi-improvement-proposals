# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This document describes the standard format used for input types. The standard follows default JSON schema that supports various data types and validation rules. Data types should be compatible with the [HTML input types](https://www.w3schools.com/tags/tag_input.asp).


By default all fields are required, you can make the optional however see the [Validation Types](#validation-types)

As a developer you don't have to care about the right display types, just define the schema, we handle the rest.

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

## Input Types Reference Table

| Type | Description | Common Validations | Data Fields |
|------|-------------|-------------------|-------------|
| **none** | Display-only text/information | None (ignored) | `description` |
| **text** | Single-line text input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` |
| **textarea** | Multi-line text input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` |
| **number** | Numeric input | `min`, `max`, `format: integer` | `placeholder`, `description`, `default` |
| **boolean** | True/false checkbox | `optional` | `description`, `default` |
| **email** | Email address input | `format: email` (auto), `min`, `max` | `placeholder`, `description`, `default` |
| **password** | Password input (hidden) | `min`, `max` | `placeholder`, `description` |
| **tel** | Telephone number | `format: tel-pattern`, `min`, `max` | `placeholder`, `description`, `default` |
| **url** | URL/website input | `format: url` (auto), `min`, `max` | `placeholder`, `description`, `default` |
| **date** | Date picker | `min`, `max` | `description`, `default` |
| **datetime-local** | Date and time picker | `min`, `max` | `description`, `default` |
| **time** | Time picker | `min`, `max` | `description`, `default` |
| **month** | Month/year picker | `min`, `max` | `description`, `default` |
| **week** | Week picker | `min`, `max` | `description`, `default` |
| **color** | Color picker | None | `default`, `description` |
| **range** | Slider control | `min`, `max` | `min`, `max`, `step`, `default`, `description` |
| **file** | File upload | None | `accept`, `maxSize`, `multiple`, `description` |
| **hidden** | Hidden field | None | `value` (required) |
| **search** | Search input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` |
| **button** | Clickable button | None | `value`, `action`, `description` |
| **checkbox** | Single checkbox | `optional` | `description`, `default` |
| **radio** | Radio button group | `min: 1`, `max: 1` | `values` (required), `default`, `description` |
| **image** | Image submit button | None | `src`, `alt`, `width`, `height` |
| **reset** | Reset form button | None | `value`, `description` |
| **submit** | Submit form button | None | `value`, `description` |
| **option** | Multi/single select | `min`, `max` | `values` (required), `description` |

## Input Type Examples

### Text Input Types

<details>
<summary><strong>text</strong> - Single-line text input</summary>

```json
{
  "type": "text",
  "name": "Username",
  "data": {
    "placeholder": "Enter username",
    "description": "3-20 characters"
  },
  "validations": [
    {"validation": "min", "value": "3"},
    {"validation": "max", "value": "20"},
    {"validation": "format", "value": "nonempty"}
  ]
}
```
</details>

<details>
<summary><strong>textarea</strong> - Multi-line text input</summary>

```json
{
  "type": "textarea",
  "name": "Comments",
  "data": {
    "placeholder": "Enter your comments...",
    "description": "Maximum 500 characters"
  },
  "validations": [
    {"validation": "max", "value": "500"}
  ]
}
```
</details>

<details>
<summary><strong>email</strong> - Email address input</summary>

```json
{
  "type": "email",
  "name": "Contact Email",
  "data": {
    "placeholder": "user@example.com"
  },
  "validations": [
    {"validation": "format", "value": "email"}
  ]
}
```
</details>

<details>
<summary><strong>password</strong> - Password input</summary>

```json
{
  "type": "password",
  "name": "Password",
  "data": {
    "description": "Minimum 8 characters"
  },
  "validations": [
    {"validation": "min", "value": "8"},
    {"validation": "max", "value": "128"}
  ]
}
```
</details>

<details>
<summary><strong>tel</strong> - Telephone number</summary>

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
</details>

<details>
<summary><strong>url</strong> - URL input</summary>

```json
{
  "type": "url",
  "name": "Website",
  "data": {
    "placeholder": "https://example.com"
  },
  "validations": [
    {"validation": "format", "value": "url"}
  ]
}
```
</details>

<details>
<summary><strong>search</strong> - Search input</summary>

```json
{
  "type": "search",
  "name": "Search Query",
  "data": {
    "placeholder": "Search for services...",
    "description": "Enter keywords"
  }
}
```
</details>

### Date/Time Input Types

<details>
<summary><strong>date</strong> - Date picker</summary>

```json
{
  "type": "date",
  "name": "Birth Date",
  "validations": [
    {"validation": "min", "value": "1900-01-01"},
    {"validation": "max", "value": "2024-12-31"}
  ]
}
```
</details>

<details>
<summary><strong>datetime-local</strong> - Date and time picker</summary>

```json
{
  "type": "datetime-local",
  "name": "Appointment Time",
  "data": {
    "description": "Select date and time"
  }
}
```
</details>

<details>
<summary><strong>time</strong> - Time picker</summary>

```json
{
  "type": "time",
  "name": "Start Time",
  "validations": [
    {"validation": "min", "value": "09:00"},
    {"validation": "max", "value": "17:00"}
  ]
}
```
</details>

<details>
<summary><strong>month</strong> - Month/year picker</summary>

```json
{
  "type": "month",
  "name": "Billing Month",
  "data": {
    "description": "Select month and year"
  }
}
```
</details>

<details>
<summary><strong>week</strong> - Week picker</summary>

```json
{
  "type": "week",
  "name": "Week Selection",
  "validations": [
    {"validation": "min", "value": "2024-W01"}
  ]
}
```
</details>

### Numeric Input Types

<details>
<summary><strong>number</strong> - Numeric input</summary>

```json
{
  "type": "number",
  "name": "Age",
  "data": {
    "description": "Must be 18 or older"
  },
  "validations": [
    {"validation": "min", "value": "18"},
    {"validation": "max", "value": "120"},
    {"validation": "format", "value": "integer"}
  ]
}
```
</details>

<details>
<summary><strong>range</strong> - Slider control</summary>

```json
{
  "type": "range",
  "name": "Priority Level",
  "data": {
    "min": "1",
    "max": "10",
    "step": "1",
    "default": "5",
    "description": "1 (low) to 10 (high)"
  }
}
```
</details>

### Selection Input Types

<details>
<summary><strong>boolean</strong> - Checkbox for true/false</summary>

```json
{
  "type": "boolean",
  "name": "Subscribe to Newsletter",
  "data": {
    "default": false
  }
}
```
</details>

<details>
<summary><strong>checkbox</strong> - Single checkbox</summary>

```json
{
  "type": "checkbox",
  "name": "Terms and Conditions",
  "data": {
    "description": "I agree to the terms",
    "default": false
  }
}
```
</details>

<details>
<summary><strong>radio</strong> - Radio button group</summary>

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
</details>

<details>
<summary><strong>option</strong> - Multi/single select dropdown</summary>

```json
{
  "type": "option",
  "name": "Countries",
  "data": {
    "values": ["United States", "United Kingdom", "Canada"],
    "description": "Select one or more countries"
  },
  "validations": [
    {"validation": "min", "value": "1"},
    {"validation": "max", "value": "3"}
  ]
}
```
</details>

### Special Input Types

<details>
<summary><strong>color</strong> - Color picker</summary>

```json
{
  "type": "color",
  "name": "Theme Color",
  "data": {
    "default": "#1a73e8",
    "description": "Choose your preferred color"
  }
}
```
</details>

<details>
<summary><strong>file</strong> - File upload</summary>

```json
{
  "type": "file",
  "name": "Document Upload",
  "data": {
    "accept": ".pdf,.doc,.docx",
    "maxSize": "10485760",
    "description": "PDF or Word documents only (max 10MB)"
  }
}
```
</details>

<details>
<summary><strong>hidden</strong> - Hidden field</summary>

```json
{
  "type": "hidden",
  "name": "Session ID",
  "data": {
    "value": "abc123xyz"
  }
}
```
</details>

### Button Types

<details>
<summary><strong>button</strong> - Clickable button</summary>

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
</details>

<details>
<summary><strong>submit</strong> - Submit form button</summary>

```json
{
  "type": "submit",
  "name": "Submit Form",
  "data": {
    "value": "Submit Application"
  }
}
```
</details>

<details>
<summary><strong>reset</strong> - Reset form button</summary>

```json
{
  "type": "reset",
  "name": "Reset Form",
  "data": {
    "value": "Clear All"
  }
}
```
</details>

<details>
<summary><strong>image</strong> - Image submit button</summary>

```json
{
  "type": "image",
  "name": "Submit Button",
  "data": {
    "src": "/images/submit.png",
    "alt": "Submit Form",
    "width": "200",
    "height": "50"
  }
}
```
</details>

### Display Types

<details>
<summary><strong>none</strong> - Display-only text</summary>

```json
{
  "type": "none",
  "name": "Instructions",
  "data": {
    "description": "Please fill out all required fields"
  }
}
```
</details>

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


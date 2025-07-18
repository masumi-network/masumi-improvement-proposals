# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This document describes the standard format used for input types. The standard follows default JSON schema that supports various data types and validation rules. 

Data types should be compatible with the [HTML input types](https://www.w3schools.com/tags/tag_input.asp) (except button types).


By default all fields are required, but you can make it optional. See [Validation Types](#validation-types).

# Format and fields

Below is an example with all possible options to define an input. (possible types are separated by '|' see [Supported Types](#supported-types) and [Validation Types](#validation-types) for more details)

```json
{
  "id": "example-input",

  "type": "text|textarea|number|boolean|option|none|email|password|tel|url|date|datetime-local|time|month|week|color|range|file|hidden|search|checkbox|radio",

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

| Type | Description | Common Validations | Data Fields | Example |
|------|-------------|-------------------|-------------|---------|
| **none** | Display-only text/information | None (ignored) | `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "none", "name": "Instructions", "data": {"description": "Please fill out all required fields"}}<br>```</pre></details> |
| **text** | Single-line text input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "text", "name": "Username", "data": {"placeholder": "Enter username", "description": "3-20 characters"}, "validations": [{"validation": "min", "value": "3"}, {"validation": "max", "value": "20"}]}<br>```</pre></details> |
| **textarea** | Multi-line text input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "textarea", "name": "Comments", "data": {"placeholder": "Enter your comments...", "description": "Maximum 500 characters"}, "validations": [{"validation": "max", "value": "500"}]}<br>```</pre></details> |
| **number** | Numeric input | `min`, `max`, `format: integer` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "number", "name": "Age", "data": {"description": "Must be 18 or older"}, "validations": [{"validation": "min", "value": "18"}, {"validation": "max", "value": "120"}, {"validation": "format", "value": "integer"}]}<br>```</pre></details> |
| **boolean** | True/false checkbox | `optional` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "boolean", "name": "Subscribe to Newsletter", "data": {"default": false}}<br>```</pre></details> |
| **email** | Email address input | `format: email` (auto), `min`, `max` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "email", "name": "Contact Email", "data": {"placeholder": "user@example.com"}, "validations": [{"validation": "format", "value": "email"}]}<br>```</pre></details> |
| **password** | Password input (hidden) | `min`, `max` | `placeholder`, `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "password", "name": "Password", "data": {"description": "Minimum 8 characters"}, "validations": [{"validation": "min", "value": "8"}, {"validation": "max", "value": "128"}]}<br>```</pre></details> |
| **tel** | Telephone number | `format: tel-pattern`, `min`, `max` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "tel", "name": "Phone Number", "data": {"placeholder": "+1-234-567-8900", "description": "Include country code"}}<br>```</pre></details> |
| **url** | URL/website input | `format: url` (auto), `min`, `max` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "url", "name": "Website", "data": {"placeholder": "https://example.com"}, "validations": [{"validation": "format", "value": "url"}]}<br>```</pre></details> |
| **date** | Date picker | `min`, `max` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "date", "name": "Birth Date", "validations": [{"validation": "min", "value": "1900-01-01"}, {"validation": "max", "value": "2024-12-31"}]}<br>```</pre></details> |
| **datetime-local** | Date and time picker | `min`, `max` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "datetime-local", "name": "Appointment Time", "data": {"description": "Select date and time"}}<br>```</pre></details> |
| **time** | Time picker | `min`, `max` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "time", "name": "Start Time", "validations": [{"validation": "min", "value": "09:00"}, {"validation": "max", "value": "17:00"}]}<br>```</pre></details> |
| **month** | Month/year picker | `min`, `max` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "month", "name": "Billing Month", "data": {"description": "Select month and year"}}<br>```</pre></details> |
| **week** | Week picker | `min`, `max` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "week", "name": "Week Selection", "validations": [{"validation": "min", "value": "2024-W01"}]}<br>```</pre></details> |
| **color** | Color picker | None | `default`, `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "color", "name": "Theme Color", "data": {"default": "#1a73e8", "description": "Choose your preferred color"}}<br>```</pre></details> |
| **range** | Slider control | `min`, `max` | `min`, `max`, `step`, `default`, `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "range", "name": "Priority Level", "data": {"min": "1", "max": "10", "step": "1", "default": "5", "description": "1 (low) to 10 (high)"}}<br>```</pre></details> |
| **file** | File upload | None | `accept`, `maxSize`, `multiple`, `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "file", "name": "Document Upload", "data": {"accept": ".pdf,.doc,.docx", "maxSize": "10485760", "description": "PDF or Word documents only (max 10MB)"}}<br>```</pre></details> |
| **hidden** | Hidden field | None | `value` (required) | <details><summary>View Example</summary><pre>```json<br>{"type": "hidden", "name": "Session ID", "data": {"value": "abc123xyz"}}<br>```</pre></details> |
| **search** | Search input | `min`, `max`, `format: nonempty` | `placeholder`, `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "search", "name": "Search Query", "data": {"placeholder": "Search for services...", "description": "Enter keywords"}}<br>```</pre></details> |
| **checkbox** | Single checkbox | `optional` | `description`, `default` | <details><summary>View Example</summary><pre>```json<br>{"type": "checkbox", "name": "Terms and Conditions", "data": {"description": "I agree to the terms", "default": false}}<br>```</pre></details> |
| **radio** | Radio button group | `min: 1`, `max: 1` | `values` (required), `default`, `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "radio", "name": "Payment Method", "data": {"values": ["Credit Card", "PayPal", "Bank Transfer"], "default": "Credit Card"}}<br>```</pre></details> |
| **option** | Multi/single select | `min`, `max` | `values` (required), `description` | <details><summary>View Example</summary><pre>```json<br>{"type": "option", "name": "Countries", "data": {"values": ["United States", "United Kingdom", "Canada"], "description": "Select one or more countries"}, "validations": [{"validation": "min", "value": "1"}, {"validation": "max", "value": "3"}]}<br>```</pre></details> |


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

# Additional Notes

## Format Validation

Input types automatically handle their own format validation:
- `email` type validates email format
- `url` type validates URL format  
- `tel` type accepts telephone numbers
- `number` type accepts numeric values

For additional validation, use the `format` validation with values like:
- `nonempty` - Ensures field is not empty
- `integer` - Ensures number is an integer

## Data Field Configuration

All input types support common data fields like `description`, `placeholder`, and `default`. Type-specific configurations are shown in the examples above and include:

- **option/radio**: Require `values` array
- **range**: Supports `min`, `max`, `step` 
- **file**: Supports `accept`, `maxSize`, `multiple`
- **hidden**: Requires `value`


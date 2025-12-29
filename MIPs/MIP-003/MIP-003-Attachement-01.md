# MIP-003 Attachement 01: Input Validation Schema Format

## Overview

This document describes the standard format used for input types. The standard follows default JSON schema that supports various data types and validation rules. 

Data types should be compatible with the [HTML input types](https://www.w3schools.com/tags/tag_input.asp) (except button types).


By default all fields are required, but you can make it optional. See [Validation Types](#validation-types).

For more info on types see [Supported Types](#supported-types) and [Validation Types](#validation-types).


## Field Descriptions

| Field | Required | Description |
|-------|----------|-------------|
| <code>id</code> | Yes | Used to identify the input. Links the input to the correct field in the data and should be unique within the form. |
| <code>type</code> | Yes | Defines the type of the input. Determines the correct rendering and validation for the input. See [Supported Types](#supported-types) for more details. |
| <code>name</code> | Yes | Used to display the name of the input to the user. |
| <code>data</code> | No | Provides additional data specific to the input type. Used to render the input accordingly. See [Data Field](#data-field) for more details. |
| <code>validations</code> | No | Provides validation rules specific to the input type. Used to validate the input accordingly. See [Validation Types](#validation-types) for more details. |

## Input Validation Schema Example
Below is an example of an input definition, with all possible types separated by  "|". 

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

# Supported Types

## Supported Input Types

| Type | Description | Common Validations | Data Fields | Example |
|------|-------------|-------------------|-------------|---------|
| **none** | Display-only text/information | None (ignored) | <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "none",<br>&nbsp;&nbsp;"name": "Instructions",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Please fill out all required fields"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **text** | Single-line text input | <code>min</code>, <code>max</code>, <code>format: nonempty</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "text",<br>&nbsp;&nbsp;"name": "Username",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "Enter username",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "3-20 characters"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "3"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "20"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **textarea** | Multi-line text input | <code>min</code>, <code>max</code>, <code>format: nonempty</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "textarea",<br>&nbsp;&nbsp;"name": "Comments",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "Enter your comments...",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Maximum 500 characters"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "500"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **number** | Numeric input | <code>min</code>, <code>max</code>, <code>format: integer</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "number",<br>&nbsp;&nbsp;"name": "Age",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Must be 18 or older"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "18"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "120"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "format", "value": "integer"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **boolean** | True/false checkbox | <code>optional</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "boolean",<br>&nbsp;&nbsp;"name": "Subscribe to Newsletter",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"default": false<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **email** | Email address input | <code>format: email</code> (auto), <code>min</code>, <code>max</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "email",<br>&nbsp;&nbsp;"name": "Contact Email",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "user@example.com"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "format", "value": "email"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **password** | Password input (hidden) | <code>min</code>, <code>max</code> | <code>placeholder</code>, <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "password",<br>&nbsp;&nbsp;"name": "Password",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Minimum 8 characters"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "8"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "128"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **tel** | Telephone number | <code>format: tel-pattern</code>, <code>min</code>, <code>max</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "tel",<br>&nbsp;&nbsp;"name": "Phone Number",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "+1-234-567-8900",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Include country code"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **url** | URL/website input | <code>format: url</code> (auto), <code>min</code>, <code>max</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "url",<br>&nbsp;&nbsp;"name": "Website",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "https://example.com"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "format", "value": "url"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **date** | Date picker | <code>min</code>, <code>max</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "date",<br>&nbsp;&nbsp;"name": "Birth Date",<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "1900-01-01"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "2024-12-31"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **datetime-local** | Date and time picker | <code>min</code>, <code>max</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "datetime-local",<br>&nbsp;&nbsp;"name": "Appointment Time",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Select date and time"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **time** | Time picker | <code>min</code>, <code>max</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "time",<br>&nbsp;&nbsp;"name": "Start Time",<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "09:00"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "17:00"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **month** | Month/year picker | <code>min</code>, <code>max</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "month",<br>&nbsp;&nbsp;"name": "Billing Month",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Select month and year"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **week** | Week picker | <code>min</code>, <code>max</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "week",<br>&nbsp;&nbsp;"name": "Week Selection",<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "2024-W01"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **color** | Color picker | None | <code>default</code>, <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "color",<br>&nbsp;&nbsp;"name": "Theme Color",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"default": "#1a73e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Choose your preferred color"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **range** | Slider control | <code>min</code>, <code>max</code> | <code>min</code>, <code>max</code>, <code>step</code>, <code>default</code>, <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "range",<br>&nbsp;&nbsp;"name": "Priority Level",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"min": "1",<br>&nbsp;&nbsp;&nbsp;&nbsp;"max": "10",<br>&nbsp;&nbsp;&nbsp;&nbsp;"step": "1",<br>&nbsp;&nbsp;&nbsp;&nbsp;"default": "5",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "1 (low) to 10 (high)"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **file** | File upload | <code>accept</code>, <code>min</code>, <code>max</code> | <code>description</code>, <code>outputFormat</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"id": "file-upload",<br>&nbsp;&nbsp;"type": "file",<br>&nbsp;&nbsp;"name": "Document Upload",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "PDF or Word documents only (max 4.5MB)",<br>&nbsp;&nbsp;&nbsp;&nbsp;"outputFormat": "url"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "accept", "value": "image/*,.pdf,.doc,.docx"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "1"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "1"}<br>&nbsp;&nbsp;]<br>}</pre></details> |
| **hidden** | Hidden field | None | <code>value</code> (required) | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "hidden",<br>&nbsp;&nbsp;"name": "Session ID",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"value": "abc123xyz"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **search** | Search input | <code>min</code>, <code>max</code>, <code>format: nonempty</code> | <code>placeholder</code>, <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "search",<br>&nbsp;&nbsp;"name": "Search Query",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"placeholder": "Search for services...",<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Enter keywords"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **checkbox** | Single checkbox | <code>optional</code> | <code>description</code>, <code>default</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "checkbox",<br>&nbsp;&nbsp;"name": "Terms and Conditions",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "I agree to the terms",<br>&nbsp;&nbsp;&nbsp;&nbsp;"default": false<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **radio** | Radio button group | <code>min: 1</code>, <code>max: 1</code> | <code>values</code> (required), <code>default</code>, <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "radio",<br>&nbsp;&nbsp;"name": "Payment Method",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"values": ["Credit Card", "PayPal", "Bank Transfer"],<br>&nbsp;&nbsp;&nbsp;&nbsp;"default": "Credit Card"<br>&nbsp;&nbsp;}<br>}</pre></details> |
| **option** | Multi/single select | <code>min</code>, <code>max</code> | <code>values</code> (required), <code>description</code> | <details><summary>View Example</summary><pre>{<br>&nbsp;&nbsp;"type": "option",<br>&nbsp;&nbsp;"name": "Countries",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"values": ["United States", "United Kingdom", "Canada"],<br>&nbsp;&nbsp;&nbsp;&nbsp;"description": "Select one or more countries"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"validations": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "min", "value": "1"},<br>&nbsp;&nbsp;&nbsp;&nbsp;{"validation": "max", "value": "3"}<br>&nbsp;&nbsp;]<br>}</pre></details> |


## Validation Types

| Validation | Description                                                                                                 | Applicable Types                                                                                    | Default Value      |
| ---------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------ |
| `min`      | Minimum length (text/password/tel/search), minimum value (number/range), minimum date/time, or minimum amount of options to select (option/checkbox/radio). Inclusive. | text, password, tel, search, textarea, number, range, date, datetime-local, time, month, week, option, checkbox, radio | -                  |
| `max`      | Maximum length (text/password/tel/search), maximum value (number/range), maximum date/time, or maximum amount of options to select (option/checkbox/radio). Inclusive. | text, password, tel, search, textarea, number, range, date, datetime-local, time, month, week, option, checkbox, radio | -                  |
| `format`   | Specific format (email, url, etc.)                                                                          | text, email, url, tel, search, (url, email, nonempty, tel-pattern), <br>number (integer)         | -                  |
| `accept` | The accept attribute takes as its value a comma-separated list of one or more file types, or unique file type specifiers, describing which file types to allow. [HTML attribute: accept](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/accept) | file | - |
| `optional` | The field is not required. By default all fields are required                                               | all                                                                                                 | false (if not set) |

By default there are no optional validations, all fields are required.

If you do not set a validation, the field will not be limited. 

If you set validations multiple times, all instances of the validation will be applied, validations follow a logical AND.

For example:

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

(Please ensure that you do not set impossible validations like `format: email` and a `max: 2` as the user will not be able to select any value. Ensure that the value you receive in your agent is validated to your needs and handled as untrusted data. This schema is a suggestion to frontend on how to display inputs)

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
- **file**: Requires `outputFormat`
- **hidden**: Requires `value`

## File Handling Options

File inputs currently support only one `outputFormat`: `url`. This means the buyer must upload the file and provide a URL as input. Another example is `base64`, which is not currently supported. If it were supported, the file would be encoded in base64 format and submitted to the agent.

**Example:**
```json
{ 
  "id": "file-upload",
  "type": "file",
  "name": "Document Upload",
  "data": {
    "description": "PDF or Word documents only",
    "outputFormat": "url"
  },
  "validations": [
    {"validation": "accept", "value": "image/*,.pdf,.doc,.docx"},
    {"validation": "min", "value": "1"},
    {"validation": "max", "value": "1"}
  ]
}
```

**Result:** The AI agent receives the file as a URL string that can be decoded and processed directly.

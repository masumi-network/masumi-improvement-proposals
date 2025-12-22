# MIP-003 Attachement 02: Pre-Filled Fields

## Overview

This document outlines certian input field IDs that are supposed to be treated like static variables & do not get exposed to the users.
The reason behind this is that there should be fields that can be automatically filled by the platform itself, such as e.g. unique the users name.
These fields should not be filled by the user but instead by the platform for better user experience. 

## Field Descriptions

| Input Field ID | Description |
|-------|-------------|
| <code>user-name</code> | Used to identify the user. |
| <code>user-email</code> | Used to identify user. Can be used as a way for the agent to contact the user directly. |
| <code>user-organisation</code> | Used to identify the users organisation. |

**Best practice:**
The user should be made aware by the platform that these information are given to the agent.
These fields should not be used to give access to any sensitive data, since they can easily be spoofed. 

## Example
Below is an example of a pre-filled field. In this case with the user-name.

**Example:**
```json
{
  "type": "text",
  "name": "Unique User Identifier",
  "id": "user-id"
}
```

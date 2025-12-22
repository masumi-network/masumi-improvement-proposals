# MIP-003 Attachement 02: Pre-Filled Fields

## Overview

This document outlines certian input field IDs that are supposed to be treated like static variables & do not get exposed to the users.
The reason behind this is that there should be fields that can be automatically filled by the platform itself, such as e.g. unique user identifiers.
These fields should not be filled by the user but instead by the platform for user experience sake. 

## Field Descriptions

| Input Field ID | Description |
|-------|-------------|
| <code>user-id</code> | Used to identify user. Can be used by the agent to build up memory about the users past requests. |
| <code>user-email</code> | Used to identify user. Can be used as a way for the agent to contact the user directly. |

**Best practice:**
The user should be made aware by the platform that these information are given to the agent. 

## Example
Below is an example of a pre-filled field. In this case with the user-id, which would be a unique identifier for each user.

**Example:**
```json
{
  "type": "text",
  "name": "Unique User Identifier",
  "id": "user-id"
}
```

# MIP-003 Attachement 02: Pre-Filled User Information Fields

## Overview

This document outlines specific input field IDs designated for user variables. The purpose of this is to ensure that certain fields can be automatically populated by platforms, such as the user's unique name. These fields should be filled by the platform rather than the user to enhance the overall user experience.

## Field Descriptions

We define the prefix `user` as a predefined indicator for platforms to automatically fill these fields with available user information.

### Examples

| Input Field ID | Description |
|-------|-------------|
| <code>user-name</code> | Used for a personal response by the agent. |
| <code>user-email</code> | This can serve as a way for the agent to contact the user directly. |
| <code>user-organisation</code> | Used to identify the user's organization.|

**Best practice:**
The user should be made aware by the platform that these information are given to the agent.
These fields should not be used to give access to any sensitive data, since they can easily be spoofed. 

## Example
Below is an example of a pre-filled field. In this case with the user-name.

**Example:**
```json
{
  "type": "text",
  "name": "User Name",
  "id": "user-name"
}
```

# MIP-00X: On-Chain Metadata Standard for A2A Agent Integration

## Author
Patrick Tobler

## Title
MIP-00X: On-Chain Metadata Standard for A2A Agent Integration

## Abstract

This proposal defines a standardized **on-chain metadata schema** for agentic services that integrate with the **Agent2Agent (A2A) protocol** within the Masumi ecosystem.

The schema anchors an agent’s **off-chain A2A Agent Card** on-chain, enabling decentralized discovery, interoperability, and verification, while keeping rich capability metadata off-chain.

## Problem Statement

Masumi should be interoperable with the A2A protocol. However the A2A protocol relies on **Agent Cards** for discovery and interoperability. However:

- Agent Cards are off-chain and mutable
- There is no canonical on-chain anchor linking an Agent Card to a verified identity
- Clients need a trusted way to discover:
  - where the Agent Card lives
  - which A2A versions are supported
  - which extensions (e.g. x402) are required

Without an on-chain standard, A2A discovery lacks trust guarantees.

## Solution

Introduce a **new, minimal On-Chain Metadata Standard** dedicated exclusively to **A2A agent integration**, combined with a clearly defined **Off-Chain Agent Card Metadata Schema**.

This creates a clean separation:

- **On-chain metadata**: identity, discovery, protocol requirements  
- **Off-chain Agent Card**: capabilities, skills, interfaces, interaction details  

## Normative References

- **Agent2Agent (A2A) Protocol Specification (latest)**  
  https://a2a-protocol.org/latest/specification/

- **A2A Agent Card canonicalization & signing**  
  https://a2a-protocol.org/latest/specification/#841-canonicalization-requirements

- **A2A specification source (Markdown)**  
  https://raw.githubusercontent.com/a2aproject/A2A/main/docs/specification.md

- **Official x402 A2A Extension URI**  
  https://github.com/google-a2a/a2a-x402/v0.1

---

## Part 1 — On-Chain Metadata Schema (A2A Integration)

The proposed on-chain metadata schema is as follows:

```json
{
  "name": ["string"],                        // (Required) Human-readable name of the agent
  "description": ["string"],                 // (Optional) Short description of what the agent does 
  "api_url": ["string"],                     // (Required) Base URL of the agent’s A2A API endpoint

  "agent_card_url": ["https://{server_domain}/.well-known/agent-card.json"], // (Required) Replace {server_domain} with the actual domain
  "a2a_protocol_versions": ["string"],       // (Required) e.g. 1.0

  "tags": ["tag1", "tag2"],                  // (Optional)
  "image": ["string"],                       // (Optional) but encouraged

  "metadata_version": 2                      // (Required)
}
```

***Note every string type on-chain is either represented by a string or array of strings, as Cardano limits single strings to 63 chars. Example: `["first 63 chars of the string","remaining string2"]`***


## Part 2 — Off-Chain Metadata Standard (A2A Agent Card)

This section defines the **Off-Chain Agent Card Metadata Schema** for agents registered via the Masumi A2A on-chain metadata standard.

The Agent Card is the primary discovery and interoperability document used by **A2A clients**.  
It describes **how to interact with the agent**, **what the agent can do**, and **which rules or extensions apply**.

Agents implementing this standard MUST expose an Agent Card that conforms to the schema below.

The Agent Card SHOULD be hosted at the URL specified in the on-chain field `agent_card_url`.
Default location: https://{server_domain}/.well-known/agent-card.json


Off-Chain Agent Card Metadata Schema

```json
{
  "protocolVersions": ["string"],            // (Required) Supported A2A protocol versions. Example: ["1.0"]
  "name": "string",                          // (Required) Human-readable agent name. SHOULD match the on-chain name to avoid ambiguity.
  "description": "string",                   // (Required) Description of what the agent does. Used for discovery by users and other agents.
  "version": "string",                       // (Required) Agent implementation version. SHOULD change when behavior, skills, or capabilities change.
  "supportedInterfaces": [                   // (Required) At least one
    {
      "url": "string",                       // (Required, HTTPS) Base URL of the agent’s A2A endpoint. All A2A operations (SendMessage, GetTask, etc.) are sent here.
      "protocolBinding": "HTTP+JSON | JSONRPC | GRPC", // (Required) Transport and binding used by this interface. HTTP+JSON is RECOMMENDED as a baseline.
      "protocolVersion": "string" // (Required)  A2A protocol version supported by this interface. MUST match one of the values in `protocolVersions`.
    }
  ],
  "provider": {                              // (Optional)
    "organization": "string",                // (Optional) Name of the organization or entity operating the agent.
    "url": "string"                          // (Optional) Public website of the provider.
  },
  "documentationUrl": "string",              // (Optional) Link to human-readable documentation or usage instructions.
  "iconUrl": "string",                       // (Optional) URL to an icon or logo representing the agent. Used in UIs, marketplaces, and dashboards.
  "capabilities": {                          // (Required)
    "streaming": true,                       // (Optional) Indicates whether the agent supports streaming task updates.
    "pushNotifications": false,              // (Optional) Indicates whether the agent supports push notifications. (e.g. webhooks) for long-running tasks.
    "extensions": [                          // (Optional) The x402 Extension is required when wanting to accept payments for the agent. Other extensions can also be added.
      {
        "uri": "https://github.com/google-a2a/a2a-x402/v0.1", // (Optional) Canonical URI of the x402 A2A extension.
        "description": "Supports payments using the x402 protocol for on-chain settlement.", // (Optional) Human-readable description of the extension.
        "required": false                    // (Optional) If true, clients MUST support x402 to interact with this agent. If false, x402 is OPTIONAL and the agent MAY offer free interactions.
      }
    ]
  },
  "defaultInputModes": ["string"],           // (Required) MIME types accepted by default. Example: ["application/json", "text/plain"]
  "defaultOutputModes": ["string"],          // (Required) MIME types produced by default. Example: ["application/json", "application/pdf"]
  "skills": [                                // (Required) At least one
    {
      "id": "string",                        // (Required) Stable identifier for the skill. Used for routing, orchestration, and selection.
      "name": "string",                      // (Required) Human-readable name of the skill.
      "description": "string",               // (Required)  Description of what this skill does.
      "tags": ["string"],                    // (Required) Tags describing the skill’s domain or purpose.
      "examples": ["string"],                // (Optional) Example prompts or use cases for this skill.
      "inputModes": ["string"],              // (Required) MIME types accepted by this specific skill.
      "outputModes": ["string"]              // (Required) MIME types produced by this specific skill.
    }
  ]
}
```

## Rationale
By splitting responsibilities clearly between:
- On-chain metadata (identity, discovery, requirements)
- Off-chain Agent Cards (capabilities, skills, interaction rules)

Masumi enables decentralized trust without freezing innovation on-chain.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the Masumi Improvement Proposals (MIPs) repository - a governance framework for the Masumi Network ecosystem. The repository contains formal proposals for improvements to the Masumi decentralized AI services marketplace.

## Project Structure

All proposals are organized in the `/MIPs/` directory, with each MIP having its own subdirectory:

```
MIPs/
├── MIP-001/  # Governance process foundation (Accepted)
├── MIP-002/  # On-chain metadata standard (Draft)
├── MIP-003/  # API standard for agentic services (Draft)
└── MIP-004/  # SHA-256 hashing standard (Draft)
```

## Working with MIPs

### MIP Format
Each MIP follows a standard format with:
- Header metadata (MIP number, title, author, status, created date)
- Abstract
- Motivation
- Specification (the core technical details)
- References

### MIP Statuses
- **Draft**: Initial proposal state
- **Review**: Under community review
- **Accepted**: Approved and implemented
- **Rejected**: Not accepted
- **Withdrawn**: Removed by author

### Creating or Editing MIPs

When working on MIPs:
1. Each MIP gets its own directory under `/MIPs/`
2. The main proposal document should be named `MIP-XXX.md`
3. Supporting documents use the format `MIP-XXX-Attachment-NN.md`
4. Update the main README.md with the new MIP's status

## Key Technical Context

The Masumi Network is building a decentralized marketplace for AI-driven services with:
- **On-chain governance**: Proposals can be voted on-chain
- **Payment rails**: Integrated blockchain payments for AI services
- **Standardized APIs**: RESTful endpoints for service discovery and interaction
- **Data integrity**: SHA-256 hashing for verifying data authenticity

## Current Development

The repository is currently on branch `MIP-003-Update`, indicating active work on the API standards proposal. This MIP defines:
- Service discovery endpoints
- Job submission and tracking
- Payment verification
- Input validation schemas

## Important Notes

- This is a documentation-only repository - no build/test commands needed
- All content is in Markdown format
- Changes should maintain consistency with existing MIP formatting
- The README.md serves as the index of all MIPs and must be kept updated
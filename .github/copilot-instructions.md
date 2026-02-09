# Copilot Instructions for llm-design

## Project Overview

This repository defines AI agent specifications for domain-specific assistants. Currently, it contains the specification for an **Aruba ISP Network Engineer Assistant** — a specialized AI persona designed to support network engineering and operational tasks within Aruba's ISP (Internet Service Provider) division.

## Core Architecture

### Agent Specification Format
- **File**: [agent.md](../../agent.md)
- **Format**: YAML configuration defining agent identity, capabilities, and operating principles
- **Structure**: Top-level `agent:` object with sections: `name`, `language`, `role`, `context`, `team_objectives`, `capabilities`, `operating_principles`, `response_style`, `nomenclature`, `cost_model_template`

### Key Project Sections

1. **Role & Context** — Defines who the agent serves (Network Engineers), their organization (Aruba), and team mission (ISP backbone administration)
2. **Services Portfolio** — Three core ISP services: L2 DCI (EVPN over MPLS pseudowire), L3VPN (EVPN over MPLS), and BGP Transit Access
3. **Capabilities** — Lists specific technical domains: BGP/OSPF/MPLS routing, peering, transit management, cost estimation, technical documentation
4. **Operating Principles** — Safety-first guidelines: always gather objectives/constraints/risks before production changes, prefer simple/operableolutions, highlight dependencies, include cost analysis in proposals
5. **Cost Model Template** — Standardized structure for CapEx/OpEx/Labor estimates in business cases

## Important Conventions

### Language & Terminology
- **Primary language**: Italian (it-IT)
- **Context**: Aruba infrastructure terminology (e.g., datacenters IT1-IT4, colocations IT101-IT103 with specific geographic locations)
- **Nomenclature is intentional**: Use Aruba's standard naming to maintain consistency across documentation and operational communication

### Safety & Operational Mindset
- Never propose production changes without explicitly stating: objectives, constraints, impacts, and risk assessment
- Always include rollback procedures and maintenance windows for destructive operations
- Highlight technical dependencies (data centers, links, hardware, contracts, licenses)
- When data is incomplete, propose a data-gathering approach plus a provisional solution

### Response Style Patterns
- Use **checklists** for operational procedures
- Use **tables** for cost comparisons and capacity analysis
- Use **numbered steps** for troubleshooting sequences
- State **explicit assumptions** when critical data is missing

## Domain Patterns

### Network Engineering Domains Covered
- Layer 2/3 troubleshooting and backbone management
- BGP/OSPF/ISIS/MPLS routing protocols
- EVPN/VXLAN overlay technologies
- Peering and transit management (RPKI, community filters)
- Service design and cost estimation
- Technical documentation (HLD/LLD, MOP, runbooks, post-mortems)

### Cost Model Application
When estimating infrastructure changes or new service proposals, use the three-category breakdown:
- **CapEx**: Hardware, installation, cabling
- **OpEx**: Licenses, maintenance, colocation, circuits
- **Labor**: Design, implementation, testing, documentation, recurring operations

## Development & Maintenance

When updating `agent.md`:
- Preserve the YAML structure and indentation
- Keep Italian descriptions clear and technically precise
- Update nomenclature if datacenters/POPs change
- Revise operating_principles if new safety rules emerge
- Extend capabilities section only for validated, recurring support patterns

---

**Last Updated**: February 2026 | **Language**: Italian | **Focus**: ISP/Network Engineering Operations

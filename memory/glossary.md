# Glossary — Field Nation Internal Terms

## Platform Concepts
- **WO** — Work Order. The core unit of work on the FN platform. Buyers post WOs, providers fulfill them.
- **Provider** — Technicians who do work on the Field Nation platform (the supply side of the marketplace).
- **Buyer** — People or companies who post work on the platform for providers to do (the demand side).
- **Flightboard** — The main UI view for managing work orders (being modernized as part of UI Modernization).
- **PUDO** — Pick-Up/Drop-Off. Unisys-specific term for locations where parts are stocked for providers to pick up and drop off. PUDOs are assigned at the part level, not the WO level.
- **Service Territories** — Geographic regions for managing provider coverage and work assignment.
- **LTE** — Long Term Engagement. Buyers FN wants to retain on the platform long-term.

## Teams & Orgs
- **BD** — Bangladesh. Where a large portion of the engineering team is located (specifically Dhaka).
- **NetDev** — Internal FN team that vets and recruits providers into a curated list for enterprise buyers.

## Technical
- **FN-Core** — The PHP monolith. The main backend codebase (Field Nation also has microservices).
- **fn_react_native** — The mobile app repository (React Native).
- **core-ui** — Frontend monorepo for UI modernization.
- **AGENTS.md** — AI coding instructions file for repos (works with Copilot and Claude Code).
- **CLAUDE.md** — Claude-specific configuration file in repos for AI-assisted development.

## Tools & Systems
- **DARRYL** — Internal AI tool developed by Travis Emslander (CTO) for natural language data queries.
- **DX** — Developer experience platform used for engineering metrics and productivity measurement.
- **Copilot** — GitHub Copilot, the AI coding assistant most FN engineers use.
- **Claude Code** — Anthropic's CLI-based AI coding tool. Chuck's primary tool for advanced AI-assisted development.

## Programs & Initiatives
- **UI Modernization** — Frontend rebuild starting with the flightboard.
- **AI PM Playbook** — Chuck's AI-powered PM workflow framework, now a Cowork plugin.
- **GTM** — Go to Market. The process/meetings for rolling out new features to customers.

## Events
- **Field Week** — Happens twice a year (US and BD at different times). Everyone comes on-site for networking, presentations, collaboration. Chuck built the FN-Core orchestration agents during US Field Week.

## People (Quick Reference)
- **Travis Emslander** — CTO. Built DARRYL. Focused on AI strategy but more cost-cutting oriented per Chuck and Cory's view.
- **Shane Kinder** — EVP Product. Same level as Cory.
- **Unisys** — Major enterprise client with complex integration requirements (ServiceNow, PUDOs, check-in workflows).

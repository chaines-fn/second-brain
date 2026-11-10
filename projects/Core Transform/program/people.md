# Key People — Core Transform (Pearl)

## Core Team

### Spencer Moulton
- **Role:** Principal Engineer — Core team leader
- **Function:** Owns driving design and implementation. Framework evaluation and tech survey lead — evaluating agentic frameworks (PydanticAI, LangGraph, Strands), runtime options (AgentCore vs EKS), and durable execution tech.
- **Slack:** <@U0ANJRRLEV6>
- **Timezone:** Mountain (just north of Salt Lake City, UT)
- **Note:** Joined ~March 30, 2026. Strong advocate for PydanticAI and `uv` for Python. At a principal level — Cory expects he'll need less architectural support than a more junior engineer. Coordinating with Chuck, Safin, Cory, and Travis on vision alignment.

### Linda Lo
- **Role:** Product Lead — transitioning onto Core Transform
- **Function:** Product strategy, use case definition, co-creator program design. Already sent a 15-page doc of use cases to Spencer before the kickoff.
- **Email:** linda.lo@fieldnation.com
- **Slack:** <@U08K27W18F8>
- **Note:** Transitioning from current team (previously on Manage at Scale initiative). Very sharp on organizational dynamics — flagged cross-team coordination concerns, legal/compliance implications, and the "data platform 2.0 spiral" risk at kickoff. Proposed the late May/early June in-person sign-off milestone.

### Brent Grill
- **Role:** Design Lead — transitioning onto Core Transform
- **Function:** Product design, buyer experience, co-creator engagement
- **Email:** brent.grill@fieldnation.com
- **Note:** Transitioning from current team (also previously on Manage at Scale). Raised early adopter/co-creator question at kickoff. Thinking about integrated buyer experience and agent interoperability.

### Chuck Haines
- **Role:** Sr. Principal Software Engineer
- **Function:** Architecture Lead — not 100% dedicated (also working other AI projects). Provides cross-cutting architectural support, helps tie work together across the org. Jointly responsible with Spencer for articulating near/mid/long-term technical decisions.
- **Slack:** <@U067T9NK3MF>
- **Email:** chuck.haines@fieldnation.com

### Maria Saxton
- **Role:** Engineering Manager (created the #pearl-eng channel)
- **Function:** Leading the Pearl engineering team, Q2 planning, team operations
- **Slack:** <@U085SGMR7B7>
- **Email:** maria.saxton@fieldnation.com
- **Note:** Was OOO for the kickoff meeting (family funeral, early April). Driving Q2 planning and potential onsite gathering.

### Kimberly Zito
- **Role:** Engineer
- **Function:** Infrastructure, DevOps, and platform concerns — buildpacks, sandbox IAM policies, operational principles for AI apps
- **Slack:** <@U06QDK955AM>
- **Note:** Standing OOO midday Wednesdays. Thoughtful about operational principles (12-to-16-factor app thinking for AI). Maintains the Pearl Sandbox state doc.

### O Max Anderson
- **Role:** Engineer
- **Function:** Hands-on development and experimentation in the Pearl sandbox
- **Slack:** <@UCGUXALFR>
- **Note:** Active in sandbox experimentation, ran into Bedrock IAM permission issues early on.

## Leadership / Sponsors

### Shane Kinder
- **Role:** VP/SVP — Initiative sponsor
- **Function:** Framed the initiative vision and strategy with Travis. Driving board/SLT alignment. Engaged partner, not day-to-day driver.
- **Email:** shane.kinder@fieldnation.com
- **Note:** Created the kickoff slide deck. Pre-wired SLT to support moving fast and avoiding edge-case-driven delays. Will participate in regular syncs.

### Travis Emslander
- **Role:** Engineering leadership — engaged partner
- **Function:** Technical strategy partner, sounding board. Co-framed the initiative with Shane. Strong opinions on avoiding the "Agent Force trap" (don't make customers build their own agents).
- **Email:** travis.emslander@fieldnation.com
- **Note:** Involved in AWS AgentCore workshop (2026-04-08). Working on "Daryl thing" which uses AgentCore memory/session storage.

### Cory Muir
- **Role:** Engineering leadership (Spencer's management chain)
- **Function:** Engaged partner, supporting Spencer and the Pearl team. Advocated for "magic marker fidelity" concept vision and one-way vs two-way door decision framework.
- **Email:** cory.muir@fieldnation.com
- **Note:** Looking at logistics for Minneapolis in-person workshop. Drew the analogy to the data platform project as a cautionary tale.

### Mynul Khan
- **Role:** CEO
- **Function:** Wants to be brought along — sees Core Transform as foundational to the business. Will be an "accelerant," not a blocker. Expects regular updates.
- **Note:** Team will set up a sync cadence that includes him. Shane describes his involvement as supportive, not directive. Talks directly with customers who are asking for automation — understands the tension that full automation creates for dispatchers.

## Extended Team / Key Collaborators

### Mike Mueller
- **Role:** Engineering leadership — infrastructure/platform expertise
- **Function:** Key stakeholder for EKS, API gateway (Gravitee), and infrastructure decisions. On-site at FN office for AWS engagements. Part of the Chuck/Spencer/Mike alignment group on AgentCore strategy. Strong advocate for managed/hosted solutions (e.g., Temporal Cloud) to minimize self-hosted maintenance burden given FN's limited SRE/platform team.
- **Slack:** <@U0713V2HBKN>
- **Email:** mike.mueller@fieldnation.com
- **Timezone:** Central
- **Note:** His integration team is setting up Gravitee API Gateway to replace AES (Ambassador Edge Stack) for Integration v3. Deep knowledge of FN's existing auth, API gateway, and infrastructure patterns.

### Ian Loverink
- **Role:** Core-ops team lead — owns auth/identity implementation
- **Function:** Best contact for auth questions. His team would implement any auth changes needed for agentic workloads. Tagged by Spencer on AgentCore Identity feasibility assessment in the AWS call talking points doc.
- **Slack:** <@UGA852PB7>
- **Note:** Added to #aws_bedrock_training channel on 2026-04-14 for the AWS call. Key person for evaluating whether AgentCore Identity can work with FN's non-standard auth (Cognito + custom layer).

## Other Stakeholders

### Safin
- **Function:** Engineering org alignment — Spencer has been coordinating with him on shared vision. Has been doing POC work (NetDev) that needs to be reconciled with Pearl's reference architecture. Meeting scheduled 2026-04-15.

### Ambina & John
- **Function:** Working through transition logistics for Linda and Brent moving onto Core Transform. Setting up meetings to discuss team transition details.

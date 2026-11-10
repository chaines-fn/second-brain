> **Note 2026-10-14:** Q4 KRs and milestones were revised (KR1 cycle time, KR2 quality, KR3 company knowledge layer; milestones M1, M2, M3). KR and milestone references below use the earlier numbering. See program/OKRs/q4-2026-okrs.md.

# Fai: spike ticket drafts, gate 1

*Draft for review. Nothing is created in Linear yet. Ticket IDs are assigned when the tickets are created.*

## Proposed project

- **Name:** Fai: Factory Q4 2026
- **Team:** Moissanite (MOI). Fai is separate from Engineering Velocity via Repo Enablement (MOI-485), the H2 O1 project. Link the two as related.
- **Lead:** Chuck Haines
- **Target date:** Dec 18, 2026 (confirm). Q4 readout.
- **Summary:** Q4 2026 OKR for Fai, the factory. A thin slice that tests delivery speed, completed issue volume, and quality.
- **Description:** Link program/OKRs/q4-2026-okrs.md as the source of truth. Restate the objective and KR1 to KR3. Out of scope: command center UI, upstream shaping and TDRs, additional OSs, a second repo target, and long-term platform ownership.
- **Milestones:** Measurement and scope; Walking skeleton; Agent creation and companyOS; First Factory-completed issue; Factory pilot; Q4 outcomes review. Gate 1 tickets sit under Walking skeleton because the skeleton depends on the platform decision.

## Spike tickets

Labels for all: `Spike`. Milestone for all: Walking skeleton.

### Harness candidates

**Spike: pi as coding harness in a sandbox**
- Priority: High.
- Ask: Decide whether pi can run a coding task end to end in a sandbox from the Runtime or EC2 spikes.
- Why: pi is a candidate in the Q4 OKR. Its MCP support is unconfirmed, and runs need MCP for tool access.
- Test: Run one coding agent with marketplace skills, using MCP, git, tests, and Bedrock through the AI gateway. Record a trace and a cost per run.
- Questions:
  - Does pi support MCP servers in a run? Cite the source.
  - Does it pass each part of the test?
  - Does it fit the sandbox limits?
- Done when: Results are recorded per part of the test, MCP status is cited, and the recommendation is keep or drop.

**Spike: OpenCode as coding harness in a sandbox**
- Priority: High.
- Ask: Decide whether OpenCode can run the same coding task end to end in a sandbox from the Runtime or EC2 spikes.
- Why: OpenCode is a candidate in the Q4 OKR. It has not been evaluated yet.
- Test: Same as the pi spike.
- Done when: Results are recorded per part of the test, the sandbox fit is noted, and the recommendation is keep or drop.

**Spike: Strands harness as coding harness in a sandbox**
- Priority: High.
- Ask: Decide whether the open-source Strands harness can run the same coding task end to end in a sandbox from the Runtime or EC2 spikes.
- Why: The architecture draft names Strands as the leading candidate, because the managed AgentCore harness is built on it. It was released recently, so its cost and accuracy numbers come from its authors.
- Test: Same as the pi spike.
- Questions:
  - Does it accept a custom OpenAI-compatible base URL?
  - Does session resume work across AgentCore sessions?
  - How does it behave on a large repo under the 8 GB limit?
  - Do its config files share code or config with the managed AgentCore harness?
- Done when: Results are recorded per part of the test, the four questions are answered with evidence, and the recommendation is keep or drop.

**Spike: AgentCore managed harness fit and cost attribution**
- Priority: High.
- Ask: Decide whether the managed AgentCore harness can host config agents with cost attributed per agent and per run.
- Why: The Q4 OKR requires cost per agent and per run for 100% of runs. The architecture draft uses the managed harness for config agents such as meeting synthesis. This is the AgentCore harness, not Harness CI.
- Questions:
  - Can cost be attributed per agent and per run? Test the run_id header through the gateway apiBase path.
  - Does a named harness endpoint invoke correctly? The endpoint qualifier is unconfirmed.
  - How are skills and tools configured on the harness? Coordinate with the skills spike.
- Done when: Cost attribution is shown with evidence, a config agent (meeting synthesis) deploys to a STAGING endpoint or the blocker is written down, and the findings are recorded for the platform decision.

### Sandbox

**Spike: Test AgentCore Runtime as the sandbox for agent runs**
- Priority: High.
- Ask: Find out what AgentCore Runtime provides for isolation, sizing, and network, and which of our runs it cannot serve.
- Why: AgentCore Runtime isolates each session in its own microVM, so it could serve as the sandbox without us building isolation. It caps a session at 2 vCPU, 8 GB memory, and a 2 GB image, with a maximum lifetime of 14400 seconds. We need to know which runs fit those caps and which need another host.
- Test: Start one run of the fn-core sandbox image and one run against the non-fn-core repo inside Runtime sessions. Bring the repo in through an S3 Files snapshot or a git clone.
- Questions:
  - What isolation does each session get across the filesystem, processes, and network?
  - Does the fn-core sandbox image fit the 2 GB image limit and the 8 GB memory limit? Does the non-fn-core repo fit?
  - Can VPC mode reach GitHub through an egress path security accepts?
  - How long do session startup and teardown take? How long does an idle session last before it times out?
  - What does one run cost in sandbox time?
- Done when: Each question has a measured or cited answer. The spike lists the runs Runtime handles, the runs it cannot handle, and the limit that blocks each one.

**Spike: Test EC2 as the sandbox for agent runs**
- Priority: High.
- Ask: Find out how a per-run EC2 sandbox starts, runs, and ends, and what we operate ourselves when we use it.
- Why: EC2 has none of the Runtime caps, so it could run the fn-core image and larger repos. The cost is that we build and operate isolation, startup, and teardown. We need to know which runs justify that work.
- Test: Run the fn-core sandbox image and one run against the non-fn-core repo, each in a per-run instance or container. Bring the repo in through an S3 snapshot or a git clone.
- Questions:
  - What does a per-run instance or container look like: image, instance type, and launch path?
  - How long do startup and teardown take? What cleans up after a failed run?
  - What isolation does each run get, and what do we have to add ourselves?
  - Which repo entry path needs network egress, and which works from inside the VPC alone?
  - What does one run cost in instance time?
- Done when: Each question has a measured or cited answer. The spike lists the runs EC2 handles, the isolation and operations work it adds, and the cases where it fits better than Runtime.

### S3 and skills

**Spike: S3 layout and read-only snapshot access for runs**
- Priority: High.
- Ask: Define the bucket and prefix layout for repo snapshots, and the read-only access each sandbox and harness needs.
- Why: Runs read repo snapshots from S3. The architecture draft mounts an S3 Files access point read-only into the sandbox. Each run should read only what it needs, so the IAM roles matter as much as the layout.
- Questions:
  - What bucket and prefix layout holds repo snapshots? Does it share a bucket with skills?
  - Does the S3 Files access point mount read-only into an AgentCore Runtime session? Does it mount into an EC2 instance the same way?
  - Which IAM execution role does each sandbox and harness use? What is the minimum read access for each?
  - Does each harness get its own workload identity automatically?
  - How does a snapshot refresh without changing a run that is already in progress?
- Done when: The layout is written down. A read-only mount is tested on both hosts with one repo snapshot. The IAM policy for each role is written down and tested to deny writes. The workload identity question has an answer with evidence.

**Spike: How skills reach agent runs**
- Priority: High.
- Ask: Decide how ai-toolbox marketplace skills get into a run, and confirm the same skills load in a managed harness and in a sandbox.
- Why: Skills live in ai-toolbox and the FN marketplace, not in the OS repos. The architecture draft loads harness skills from S3 under versioned prefixes. Runs do not accept skill overrides, because only the deployed definition sets skills. Skill version pinning is deferred.
- Test: Load one marketplace skill into a managed harness run and into a sandbox run.
- Questions:
  - How do ai-toolbox skills get published to S3: a sync job, a build from git, or both? What triggers the sync?
  - Do the managed harness and the sandbox load skills from the same path?
  - Can a run override its skills at invoke time? Confirm it cannot with a test call.
- Done when: One marketplace skill loads in both runtimes from the same S3 path, with no fetch from GitHub at run time. The sync trigger is written down, and the override behavior is confirmed.

### Cost and security

**Spike: Measure cost per run by run_id**
- Priority: High.
- Ask: Capture the cost of each run, keyed by run_id, and reconcile it to Bedrock usage.
- Why: KR4 of the Q4 OKR needs cost per agent, per run, and per accepted change for 100% of runs. The variance tolerance is not yet set. The Bedrock IAM role (MOI-149) blocks testing.
- Questions:
  - Can token cost be captured per run through the AI gateway, keyed by run_id, and reconciled to Bedrock usage?
  - What compute cost (sandbox time) sits outside token cost, and can it be captured?
  - What variance tolerance is acceptable? Agree it with Cory.
- Done when: A test run produces a cost record keyed by run_id, reconciliation against Bedrock or CloudWatch is within the agreed tolerance, and gaps are recorded for the platform decision.

## Parked for later (no tickets yet)

- Eligible issue cohort and human baseline for KR1 to KR3. The Measurement and scope milestone has no ticket until this is created.
- Eval engine choice for the agent definition gate: Strands Evals, Harness Evals, or DeepEval.
- Temporal worker placement on AgentCore or on ECS or EKS.

## Open questions

1. Confirm the Oct 21 platform decision date.
2. Workato and GCP: the OKR lists both as part of the platform decision. They are decisions rather than spikes, so they may need a decision ticket.
3. Bedrock IAM role (MOI-149): it blocks the pipeline and the Bedrock-dependent spikes (the harness spikes and cost capture). When will it be granted?
4. Team and lead: confirm Moissanite as the team and Chuck as project lead.
5. Milestone placement: gate 1 sits under Walking skeleton. Confirm this matches the OKR exit criteria.

# CorpClaw: A Personal AI Agent for Google Employees

## The Problem

Google employees juggle a dozen internal tools daily. Buganizer for bugs. MOMA for people. Corp Gmail for email. Calendar for meetings. go/ links for everything else. Each tool lives in its own tab, its own workflow, its own mental context switch. No single interface ties them together.

## The Proof of Concept

I run OpenClaw on a Raspberry Pi at home. It reads my personal email, checks my calendar, searches the web, controls my devices — all through a single Telegram chat. It works because it treats every tool as a skill: a small plugin that wraps an API. The agent decides which skill to invoke based on what I ask.

The architecture is simple. A message bus receives input. An LLM routes intent. Skills execute actions. Results flow back. This pattern ports to any tool ecosystem — including Google's.

## What It Would Do

A corp-internal agent — call it CorpClaw — would integrate the tools Googlers already use:

- **Buganizer:** "What P0s are assigned to my team?" "File a bug for the latency regression in prod."
- **Calendar:** "Block 2 hours tomorrow for deep work." "What's my next meeting?"
- **Corp Gmail:** "Summarize unread threads from my TL." "Draft a reply to the perf committee."
- **MOMA:** "Who's the TL for Cloud Spanner SRE?" "Show me the org chart for Infra."
- **go/ links:** "Open go/oncall." Natural language resolution of internal resources.
- **Code Search / Critique:** "Find recent CLs touching this config." "What's the review status on my open CLs?"

Each integration is a skill. Ship one at a time. Start with Calendar and Buganizer — highest frequency, clearest value.

## Deployment

Three options. Each carries different tradeoffs.

**Corp laptop (gLinux/gMac).** Runs locally. Data stays on-device. Uses corp auth natively. Simplest compliance story. Limited by laptop uptime.

**Corp cloud (GCE/Borg).** Always on. Scales. Requires LOAS or workload identity for auth. Must stay within corp network boundary. Best for a productionized service.

**Personal device.** Where I run OpenClaw today. Cannot touch corp data. Full stop. Useful only for non-corp tasks or as an architecture reference.

The hard constraint: **corp data cannot leave corp.** Any deployment that processes internal data must run on corp infrastructure, behind corp auth, subject to corp data governance. No exceptions.

## Two Paths Forward

**20% project.** Build it inside Google. Use internal LLMs (Gemini via Vertex). Tap existing APIs. Ship to a small group in Infra. Prove value. Expand. No IP complications. No compliance headaches. The fastest path to impact.

**External product (Ridgeback Solutions LLC).** Build a generic "enterprise agent framework" that companies self-host. Google becomes the first case study — but only Google deploys the Google-specific skills, on Google infrastructure. Ridgeback owns the framework, not the integrations. Harder. Slower. Bigger upside if it works.

These paths aren't mutually exclusive. Start inside. Learn what works. Extract the generic parts later.

## Next Step

Build one skill. Calendar is the obvious choice — it has a clean API, high daily usage, and low data sensitivity. Ship it. Use it. Let the results argue for everything else.

---

*Draft — February 2026*

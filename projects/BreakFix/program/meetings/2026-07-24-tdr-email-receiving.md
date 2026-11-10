# Technical Design Review - Email Receiving - Jul 24, 2026

**Organizer:** Mike Mueller · 7:00 AM CDT
**Presenter:** Andrew Kandels
**Speakers:** Andrew Kandels, Travis Emslander, Mike Mueller, Joshua Kapellen, Safin Ahmed, Almansur Siddiqui, Murad Rahat
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1UGHFI4_UUKZctMgaW8qn0N4ScrV_VzGyXdK8lMYpLE0/edit)

> **Not owned by a Break/Fix team**, but NetDev recruiting (SLATE) is one of the two named driving use cases (the other is the AV/complex-projects LaborFlex/NPE work). Safin Ahmed participated on the B/F side. This is the TDR behind the email-send-service work SLATE is absorbing - see `program/meetings/2026-07-24-netdev-tooling-connect.md`.

## Problem

NetDev's Gmail-based reply-tracking gets throttled after "a few thousand messages," which pauses email in **both** directions for roughly a day. The monolith already supports email-reply-to-work-order via SES → SNS/SQS → `email_receiver` Lambda → `inbox` table (~1M messages/day, 4-day retention, **no DLQ**; a 2–3k message backlog is ~1–2 min delay).

## Decisions

| Decision | Detail |
|----------|--------|
| **Option 3 (extend the monolith) rejected outright** | Adds responsibility/bloat. Aligned. |
| First pass will **not** refactor monolith email receiving | Opt-in by allow-listed sender address for new services only. Monolith refactor deferred to a later phase. |
| **No new data persistence** - publish to Kafka, let each consuming service own correlation and retention | Reversal within the meeting: Andrew opened recommending Option 1 (extend Email Sender, correlate replies against MongoDB message IDs). Travis pushed back hard - "I don't think we need data persistence here at all" - citing a prior MongoDB outage where the only mitigation was shutting off Email Sender logging; adding *functional* persistence would remove that lever. Andrew conceded. |
| **Pattern established:** synchronous message-ID handback | The sending service gets a message ID back synchronously from Email Sender *before* the send, stores it locally for whatever TTL it wants, and correlates on its own. Downstream services own their own audit logging rather than a central DB (Travis, answering Joshua's compliance question). |

## Still Open

- **Kafka topic strategy.** Firehose-plus-cheap-header-filter vs. topic per sender/feature. Mike sided with Travis toward per-sender/per-feature topics: "there's going to be a lot of messages on these topics that only one consumer cares about." Andrew's counter: name the topic at the *feature* level, sender address as configuration, allow list extended from `sender` to `sender + topic`. Topics must be pre-registered - no dynamic topics.
- **TTL / storage path.** Three options still live: existing Email Sender + Mongo path, migrate to DocumentDB (Safin's preferred long-term), or no new persistence at all (Travis's and now Andrew's position).

## Risks Raised

- **No DLQ on the receive side today** - Andrew proposes adding one.
- Header rewriting by forwards/milters/O365 breaking correlation (Joshua).
- Distinguishing genuine replies from CC/BCC recipients (Almansur) - punted to the consuming service.
- Joshua proposed a "treat the service as a mail client" bulkhead principle so raw-email exception handling doesn't leak into every consumer. Noted, not decided.

## Outcome

**Not approved.** Andrew to update the doc and repost to the Engineering TDR channel. Side benefit called out: moving off the Mongo/`inbox` path removes significant DB load.

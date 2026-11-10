# Time-on-Page/Tab Tracking Tools Beyond Amplitude & Datadog

**Research question:** Beyond Amplitude and Datadog RUM (already evaluated for Mike Mueller's request), what other tools could support foreground/idle-aware duration tracking on specific pages, tabs, and rows (e.g., Provider Match page, FlightBoard tabs, WO row hover), with the data landing in the Data Platform for reporting?
**Date:** 2026-08-27
**Scope:** Covers the product analytics, digital experience analytics (DXA), web analytics, and warehouse-native CDP tool landscape. Excludes formal pricing/procurement analysis, security/compliance review, and internal build-effort estimation — those would need a follow-up pass if any of these move forward.

---

## Summary

Six tools are worth putting on the table beyond Amplitude and Datadog: Google Analytics 4, Pendo, PostHog, Contentsquare, FullStory, and the warehouse-native CDP category (Snowplow/RudderStack). None of them do exactly what Mike described out of the box — a true start/stop timer keyed to foreground/background, tab switch, and idle threshold, at row-level granularity, landing natively in the Data Platform. But two are meaningfully closer than what Andrew and I already looked at.

GA4's engagement-time metric is the closest semantic match to Mike's spec: it's foreground-only by design, pausing when a tab is hidden and resuming when it's active again. That's the exact behavior in his write-up. The catch is GA4 is built for marketing/web analytics, not authenticated in-app B2B usage, so adopting it here would be an odd fit organizationally even though the underlying mechanism is right.

Pendo is the more realistic option. It's a B2B product analytics tool (used for feature/page adoption tracking in software like ours), it already excludes idle and backgrounded time from its "time on page" calculation using 1-minute active windows, and it has a native nightly Data Sync to Snowflake and other warehouses. It's still page-level, not row-hover level, without custom Track Events, but the foundation is closer to what Mike's asking for than Amplitude's approach.

Contentsquare and FullStory are digital experience analytics tools built specifically for hover/dwell/zone-level tracking — genuinely the closest match for "how long did someone hover over this WO row." But they're oriented toward UX/heatmap output for marketing and product design teams, not toward raw event export into a data warehouse for engineering-style reporting. Getting granular data out to Tableau isn't their primary use case.

PostHog is the strongest "get it into the warehouse" story: native batch export to Snowflake, BigQuery, Redshift, S3, and it's open source (self-hostable), which fits FN's AWS-first posture. Its time-on-page approach is the same gap-between-events calculation as Amplitude on web, though — no foreground-awareness out of the box.

The warehouse-native CDPs (Snowplow, RudderStack) don't solve the tracking problem at all — they solve the "getting custom events into the Data Platform" problem. You'd still build the exact start/stop logic Mike described yourself (visibilitychange, idle timer, route change listeners), same lift as Datadog's Duration Vital API, but the resulting events land directly in the warehouse with schema governance instead of living inside a vendor's UI first.

---

## Findings

### Comparison at a glance

| Tool | Foreground-aware timing? | Row/element-level tracking? | Native warehouse export? | Best fit for this ask |
| --- | --- | --- | --- | --- |
| GA4 | Yes — pauses on hidden tab, resumes on focus | No (page-level; would need custom params) | Not native to a warehouse; BigQuery export exists but GA4 is schema-locked to its own event model | Closest timing semantics, wrong tool category for internal B2B app |
| Pendo | Partial — excludes idle/background via 1-minute active windows | No (page/feature-level; row-level needs custom Track Events) | Yes — nightly Data Sync to Snowflake/S3/GCS/Azure | Best realistic fit if buying a product analytics tool |
| PostHog | No (web) — gap-between-events like Amplitude | No — needs custom events | Yes — native batch export to Snowflake/BigQuery/Redshift/S3; open source/self-hostable | Best warehouse story if starting fresh, weak timing model |
| Contentsquare | Yes, implicitly (dwell/attention time is derived from real engagement, not raw wall-clock) | Yes — zone/element-level hover and dwell metrics are its specialty | Not clearly built for raw export to a warehouse; enterprise-managed | Best match for "hover on a row," weakest for Data Platform reporting |
| FullStory | Similar to Contentsquare (session-replay-derived) | Yes, via replay/session analysis | Not clearly built for raw export to a warehouse | Same tradeoff as Contentsquare |
| Snowplow / RudderStack | N/A — you build the logic | N/A — you build the logic | Yes — this is their core purpose (warehouse-native CDP) | Best "get our own custom events into the Data Platform" path, doesn't reduce instrumentation work |

### Google Analytics 4 — closest timing semantics, wrong category

GA4 tracks `engagement_time_msec`, which represents time the page was actually in the foreground of the user's device. Hidden-tab time, minimized-window time, and time when another application has focus don't count — the timer pauses and resumes exactly the way Mike's spec describes for "session end" and "session start." This is a genuinely different model from Amplitude's Time Spent Analysis (gap-between-events) and from Datadog's `view.time_spent` (counts background time).

The problem is fit, not capability. GA4 is built for marketing/web analytics on public-facing sites, not authenticated in-app product usage tracking. Bringing it in as a new vendor for this narrow use case, alongside Amplitude and Datadog RUM which already exist here, would add a fourth telemetry system rather than extending one we already have. Worth knowing the mechanism exists and works, less clear it's worth adopting.

### Pendo — most realistic out-of-box option

Pendo calculates time on page in 1-minute active windows: a minute only counts if the visitor generated a browser event (click, focus, etc.) during it, and time stops accumulating the moment a visitor goes idle or backgrounds the tab, resuming when they're active again. That's much closer to Mike's foreground/idle model than Amplitude's approach, though the granularity is coarser (whole minutes, not precise start/stop timestamps) and it's not natively row-level — a specific WO row would need a custom Track Event fired on hover-start/hover-end, same instrumentation work as anywhere else.

Pendo has a native Data Sync feature that syncs event, visitor, and account tables into Snowflake (or S3/GCS/Azure Blob) on a nightly cadence — no third-party ETL tool required, which is a real advantage over Amplitude's Enterprise-gated Snowflake export.

### PostHog — best warehouse story, same timing gap as Amplitude

PostHog documents time-on-page the same way Amplitude does: computed from the gap between consecutive event timestamps, with a configurable inactivity cutoff (30 minutes by default) rather than true foreground detection. Foreground/background lifecycle tracking is documented specifically for its mobile SDKs, not confirmed for web.

Where PostHog stands out is data portability: native batch exports to Snowflake, BigQuery, Redshift, S3, and Postgres are built in, and the product itself is open source and self-hostable. If FN ever wanted to run this kind of telemetry without depending on a vendor's export tier, PostHog removes that dependency. It doesn't solve the foreground-awareness gap on its own.

### Contentsquare & FullStory — best row/hover fit, weakest reporting fit

Both are digital experience analytics platforms built around session replay and heatmaps. Contentsquare's "engagement zones" specifically combine click, hover, move, and scroll data into per-element dwell-time and attention metrics — this is the one category of tool built to answer "how long did someone hover over this specific row" as a first-class feature rather than a workaround. FullStory works similarly through session replay analysis.

Neither tool's public documentation shows a straightforward path to raw event export into a data warehouse for BI reporting the way Pendo, PostHog, Amplitude, or the CDPs do. These are typically sold to and used by UX/design/marketing teams for qualitative and visual analysis (heatmaps, replays), not as a data source feeding Tableau. That doesn't rule them out, but it means the "into the Data Platform" half of Mike's ask is the harder part with these, not the tracking itself.

### Snowplow / RudderStack — solves distribution, not tracking

These are warehouse-native customer data platforms: their entire model is capturing custom events (from your own instrumentation) and delivering them directly into Snowflake/BigQuery/etc. with schema management, rather than housing an analytics UI you have to export from. If FN's team builds the exact start/stop logic Mike described (the same `visibilitychange`/idle-timer/route-change work needed for Datadog's Duration Vital approach), a tool like this would take that event and land it in the Data Platform directly, no export step, no per-vendor plan tier gating the warehouse sync.

This doesn't reduce the instrumentation effort at all — it's a distribution layer, not a tracking layer. Worth naming because it directly addresses the "rotten apple" problem Andrew called out, just from the pipe rather than the source.

---

## Key uncertainties

- I haven't confirmed whether FN already has a Segment, RudderStack, or Snowplow-style pipeline in place anywhere in the stack — the UI Modernization and Core Transform notes reference Kafka events feeding OpenSearch/the Data Platform from FN-Core, but that's backend event infrastructure, not frontend telemetry. Worth checking with the Data Platform team before assuming a build-it-ourselves path starts from zero.
- Contentsquare and FullStory's warehouse/export capabilities may be understated here — enterprise contracts sometimes include custom data feeds not documented publicly. Worth a direct conversation with either vendor if this path gets serious consideration.
- Pricing and plan-tier gating (especially for warehouse export features) changes often across all of these vendors — the Amplitude Snowflake-requires-Enterprise finding from the earlier research and Pendo/PostHog's export tiers should be re-verified against current contract terms before anyone commits to a direction.
- None of this addresses whether any of these vendors are already under an FN contract (e.g., if Product already pays for Pendo or a DXA tool elsewhere) — that would change the calculus significantly toward "extend what we have" rather than eval-from-scratch.

---

## Sources

- [Time spent analysis (Amplitude)](https://amplitude.com/docs/analytics/charts/data-tables/time-spent-analysis)
- [Engagement Time in GA4: How engagement_time_msec Works](https://accs-net.com/glossary/engagement-time/)
- [Does Google Analytics 4 Have Time on Page Metric? (Graphed)](https://www.graphed.com/blog/does-google-analytics-4-have-time-on-page-metric)
- [Time on page and app calculations (Pendo Help Center)](https://support.pendo.io/hc/en-us/articles/360032203171-Time-on-page-and-app-calculations)
- [Overview of Pendo Data Sync (Pendo Help Center)](https://support.pendo.io/hc/en-us/articles/18214274061595-Overview-of-Pendo-Data-Sync)
- [How to calculate time on page (PostHog)](https://posthog.com/tutorials/time-on-page)
- [Session Duration in PostHog (Webeyez)](https://webeyez.com/insights/guides/session-duration-posthog)
- [Batch exports (PostHog Docs)](https://posthog.com/docs/cdp/batch-exports)
- [Is PostHog warehouse native?](https://posthog.com/data-stack/warehouse-native)
- [How to Use Engagement Zone Heatmaps (Contentsquare)](https://contentsquare.com/guides/heatmaps/engagement-zones/)
- [Contentsquare Zoning Analysis Guide (Webeyez)](https://webeyez.com/insights/guides/contentsquare-zoning-analysis-guide)
- [RudderStack vs Snowplow comparison](https://www.rudderstack.com/blog/rudderStack-snowplow-comparison/)
- [Snowplow vs. RudderStack (Snowplow)](https://snowplow.io/comparisons/snowplow-vs-rudderstack)
- Datadog RUM Duration Vital API and `view.time_spent` findings from the prior Amplitude vs. Datadog research in this thread (not re-cited in full here; see Slack thread and chat history for details)

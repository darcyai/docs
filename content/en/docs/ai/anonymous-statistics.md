---
title: "Anonymous Statistics"
weight: 800
---

By default, Darcy collects anonymous usage information from Darcy AI Engine.

We are strongly committed to your [data privacy FIXME](/link/to/privacy/policy).

We use the statistics gathered from this information for two purposes:

1. **Quality assurance**, to help us understand if AI Engine behaves as expected,
and to help us classify issues in various environments.

2. **Usage statistics**, to help us interpret how people use AI Engine in real-world environments,
and to guide planning of future development and optimization.

You can opt out from sending anonymous statistics via the [opt-out mechanism](#opt-out).

## Data Collection

Each time a Darcy AI Engine pipeline begins, information is gathered about the runtime
state and environment of AI Engine, and that information is sent to the [Segment](https://segment.com)
platform (via an embedded Segment [client](https://segment.com/docs/connections/sources/catalog/libraries/server/python/)) and ultimately routed to Darcy's data warehouse.
Additionally, events are sent when a pipeline completes, an error condition is encountered, or
if a certain amount of time has elapsed since the last event (a _heartbeat_).

The information collected is:

- Generated anonymous machine ID
- AI Engine version
- Python runtime version
- System environment (OS, CPUs)
- Containerization technology
- AI acceleration technology
- Timezone
- Basic details of the pipeline config
  - Generated anonymous pipeline ID
  - Quantity and names of input and output streams
- Details of error messages
- FIXME: complete this list

This mechanism imposes minimal runtime overhead. Data collection events are batched and
sent asynchronously. Any failure in statistics collection will not affect the state of the
AI Engine process.

The anonymous statistics are stored in a US region. No Personally Identifiable Information (PII) is gathered.

## Opt-Out

You can opt out from sending anonymous statistics via the `DARCYAI_DISABLE_TELEMETRY` environment
variable. You can set this variable with the following command: `export DARCYAI_DISABLE_TELEMETRY=1`.

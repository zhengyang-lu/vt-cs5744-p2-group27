---
title: "Phase 2: Cloud to External Services Integration"
layout: post
---

In this phase of testing, we will be focusing on the interactions between the core of the Smart Food Inventory, and its various external services. This will largely be testing outgoing traffic from the application's core. Within this, it will assess the interactions with the Recipe API, calls to the Push Notification service, and information from the Telemetry service. 

These tests verify the logic between coming from the core service required by the following set of requirements:
* FR-8  : The system must notify users when an item is nearing its expiration date. 
* FR-11 : The system shall suggest basic recipe ideas using ingredients that are nearing expiration, leveraging a lightweight API or predefined recipe dataset (Advanced personalization and nutrition filters may be deferred to future releases). 
* FR-14 : The system shall provide summary insights on usage and food waste trends. 

These tests also verify non-functional components of the app required by the following set of requirements:
* NFR-1b : The system shall enqueue expiry alerts within < 2 s after an item write and deliver push notifications within < 60 s of the scheduled due time (platform permitting). 
* NFR-2 : The system shall encrypt user data in transit and at rest. 
* NFR-9 : The system shall implement telemetry and centralized logging for auditability, monitoring, and performance tracking. 

## Phase 2 Test Case Skeletons

| Test Case ID | Test Case Objective                                                       | Test Description                                                   | Expected Results                                                                                                                                                                   |
| ------------ | ------------------------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| P2-1a        | Verify successful recipe lookup via external Recipe API.                  | Core sends ingredient list to Recipe API.                          | API receives properly formatted request and sends suggestions for the core to display.                                                                                             |
| P2-1b        | Validate Core behavior when Recipe API returns malformed or errored data. | Recipe API error on malformed data from Core.                      | Core logs error via telemetry and avoids error on the front-end.                                                                                                                   |
| P2-1c        | Ensure Core handles Recipe API network failures gracefully.               | Recipe API times out or is otherwise unreachable.                  | Core handles time out properly, logs the event, and executes fallback functions.                                                                                                   |
| P2-1d        | Confirm Core enforces throughput limits for Recipe API calls.             | Core handles multiple calls to Recipe API.                         | Core enforces throughput restriction: only one API call is active at a time; excess requests are queued, processed sequentially, and all outputs are logged with correct ordering. |
| P2-2a        | Validate the Push Notification Service receives correct alert payloads.   | Core sends expiration alert payload to Push Notification Service.  | Push service receives properly formatted message; user receives alert successfully                                                                                                 |
| P2-2b        | Ensure Core handles push notification errors correctly.                   | Push Notification Service returns an error.                        | Core logs the error, retries per policy, and avoids duplicate notifications.                                                                                                       |
| P2-2c        | Verify high-volume notification dispatch behavior.                        | Multiple alerts (≥50) triggered simultaneously.                    | Core batches or queues notifications correctly; no dropped or duplicated alerts.                                                                                                   |
| P2-2d        | Ensure Core handles Push Notification Service unavailability.             | Push Service is unreachable, returns timeout, or connection fails. | Core logs failure, schedules retry if applicable, and prevents alert loss or repeated notifications.                                                                               |
| P2-3a        | Ensure Core sends structured logs to Seq / OpenTelemetry collector.       | Core emits log events (info, warning, error).                      | Logs appear in Seq/OTel with correct schema, timestamp, correlation IDs, and severity.                                                                                             |
| P2-3b        | Validate Core emits distributed traces for external-service calls.        | Trigger recipe lookup, push notification, or sync event.           | Trace spans are captured, linked by correlation ID, and exported properly to OTel collector.                                                                                       |
| P2-3c        | Ensure Core sends telemetry metrics to OTel collector.                    | Core generates metrics (latency, failure counts, throughput).      | Metrics are successfully exported, visible in downstream dashboards, and recorded with correct labels/dimensions.                                                                  |
| P2-3d        | Verify Core behavior when telemetry collector is unavailable.             | Telemetry endpoint down, unreachable, or returns errors.           | Core buffers according to configuration (or drops gracefully), avoids blocking critical functions, and records internal fallback events.                                           |
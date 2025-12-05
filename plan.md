---
title: Test Plan
layout: post
---
Testing the Smart Food Inventory System will be performed in three phases utilizing a bottom-up or “core-first” approach. This avenue is chosen because the Cloud/Core modules contain the highest complexity (OCR, parsing, inventory rules, notifications, storage sync). These are difficult to stub realistically, and the system cannot be meaningfully exercised without them. Furthermore, the External Services such as the Recipe API and Push Service rely on internal triggers to function properly.

Taking this into account, the first phase will center on testing the Cloud/Core modules as they form the foundation of the SMIS system. The five modules represent the internal, non-UI-facing core business logic and are tested in isolation to ensure their operations are correct; the system is proven to produce correct events/payloads and persists in a reliable manner.

The second phase will direct testing efforts towards integrating the Cloud/Core services to the External Services which are third-party dependencies. This is due to the fact that the External Services depend on correct internal events such as inventory changes and expiry triggers. In this phase, the authentication flow, Recipe API integration, Push Service’s receival of correctly, formatted payloads, and telemetry log flow are tested. This way we can confirm that the Cloud/Core module can interact with all external providers before they are exposed through the UI.

The third phase will encompass testing the end-to-end integration. The Client/UI integration will be tested with the fully functional system, covering all end-to-end flows. This is due to the fact that the UI relies on all underlying services (Cloud/Core and External Services) to be fully functional. Testing the full chain is justified at this point since true integration or UI errors will be dealt with as opposed to fundamental errors in the main business logic.

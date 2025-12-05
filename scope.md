---
title: Test Scope
layout: post
---

Smart Food Inventory is a system that aids in keeping inventory of food items in order to reduce waste.

The system encompasses

1. Client/UI
2. Cloud/Core
   1. Data Processing & Recognition Module
   2. Inventory Management Module
   3. Notification & Recipe Engine
   4. Telemetry & Logging Module
   5. Data Storage Layer
3. External Services
   1. Authentication
   2. Recipe API
   3. Push Service
   4. Seq/OpenTelemetry Collector

We will discuss testing strategies touching on the primary backend, represented by the five Cloud/Core module services. These modules form the foundation of the SMIS system and contain the main application logic and are tested first in isolation to ensure the stability and functionality of each individual module.

Next, testing strategies for the integration of the Cloud/Core services to the External Services will be discussed. We will test the Cloud/Core modules’ ability to communicate with its four major external dependencies.

Testing will be performed on the client/UI integration with the fully functional system, where the frontend interaction is tested with the stable backend. Lastly, we will cover validation checks for the Client, Cloud Services, and External Services as meeting the functional and non-functional requirements.
---
title: "Phase 1: Core/Cloud Module Testing"
layout: post
---

In the first phase, we are testing the core functionality of the application's backend logic. This involves isolating the four Cloud/Core modules identified in the architecture (Data Processing, Inventory Management, Telemetry, and Data Storage, minus Notifications) to ensure they process data correctly before any user interface or external service integration occurs.

This phase validates the internal algorithms for OCR parsing, barcode decoding, inventory deduplication, expiration estimation, and data synchronization. External dependencies (like the Recipe API or Push Notification Service) are mocked to ensure we are strictly testing the stability of the application's business logic.

These tests verify critical internal logic needed by the following requirements:

* FR-1: The system shall allow users to scan grocery receipts to add multiple items to their inventory.
* FR-2: The system shall allow users to scan individual barcodes to add single items.
* FR-3: The system must allow manual entry of items as a fallback option.
* FR-4: The system shall support full lifecycle management of inventory items, including manual editing, updating, and deletion.
* FR-5: The system must store product name, category, purchase date, quantity, expiration date, and location.
* FR-6: The system must estimate expiration dates when data is unavailable.
* FR-7: The system must categorize items (e.g., produce, dairy, meat) for organization and shelf-life estimation.
* NFR-2: The system shall encrypt user data in transit and at rest.
* NFR-8: The system shall ensure referential and transactional integrity across all persisted data.
* NFR-9: The system shall implement telemetry and centralized logging for auditability, monitoring, and performance tracking.

The tests will interact with the following modules:

* DE-2: Data Processing and Recognition Module
* DE-3: Inventory Management Module
* DE-5: Telemetry and Logging Module
* DE-6: Data Storage Layer

## Phase 1 Test Case Skeletons

| Test Case ID | Test Case Objective                                     | Test Description                                                                                                   | Expected Results                                                                                                                             |
| ------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| P1-1         | Validate OCR parsing logic on raw receipt images (FR-1) | Inject sample receipt image containing known items into DE-2                                                       | The module correctly identifies text blocks, extracts item names, prices, and dates, and outputs a normalized JSON object.                   |
| P1-2         | Validate barcode decoding algorithm (FR-2)              | Pass raw barcode strings (UPC-A, EAN-13, Code128) to DE-2                                                          | The decoder correctly interprets the standard and returns the corresponding numeric identifier without checksum errors.                      |
| P1-3         | Validate manual entry data normalization (FR-3)         | Inject manually created item object with whitespace, emojis, or any other UTF-8 characters into DE-3               | The system accepts the manual entry, trims whitespace, and persists the record successfully.                                                 |
| P1-4         | Validate Inventory CRUD operations (FR-4, FR-5)         | Perform Create, Read, Update, and Delete actions on DE-3                                                           | Data persists correctly after creation/update; items are marked as "deleted" or removed physically depending on policy; state is consistent. |
| P1-5         | Validate expiration date estimation logic (FR-6)        | Input an item into DE-2 with no explicit expiration date but a known category (e.g. Dairy)                         | The system assigns an estimated expiration date based on the predefined shelf-life rules (e.g., +7 days for Dairy).                          |
| P1-6         | Validate automatic item categorization (FR-7)           | Input items with known keywords (e.g., "Apple", "Chicken", "Cheddar") without a category into DE-2                 | DE-2 assigns the correct categories ("Produce", "Meat", "Dairy") based on keyword matching logic.                                            |
| P1-7         | Verify item deduplication logic (NFR-8)                 | Attempt to add an item into DE-3 that matches an existing inventory record (same name, barcode, and purchase time) | DE-3 detects the duplicate, merges the quantity into the existing record, and does not create a new entry.                                   |
| P1-8         | Verify data encryption at rest (NFR-2)                  | Inspect the DE-6 after saving sensitive user data                                                                  | Data is not readable in plain text; AES-256 (or equivalent) encryption is verified on the stored bytes.                                      |
| P1-9         | Validate basic operational logging (NFR-9)              | Trigger a standard inventory action (e.g., item added) and inspect DE-5                                            | DE-5 captures a structured log entry containing the correct timestamp, severity level (INFO), and operation ID.                              |
| P1-10        | Verify telemetry PII redaction (NFR-9)                  | Inject a log event into DE-5 containing a mock email address or personally identifiable info                       | DE-5 captures the log but replaces the sensitive string with "REDACTED" or a hash before processing.                                         |

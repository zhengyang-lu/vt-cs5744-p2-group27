---
title: "Phase 3: Full Client/UI Integration"
layout: post
---

In this phase, we are performing front-end to cloud integration testing to ensure that the application's user interface correctly interacts with cloud services. The goal is to validate end-to-end workflows such as scanning receipts or barcodes, adding items manually, syncing inventory data, receiving alerts, and fetching recipe suggestions. This testing focuses on real user interactions and verifies that UI actions trigger the correct cloud requests and handle responses properly.

The following table summarizes the key test cases for front-end → cloud integration:

# Test Case Skeletons

| Test Case ID | Test Case Objective | Test Description | Expected Results |
|--------------|------------------|------------------|------------------|
| 1 | Validate receipt upload triggers OCR processing in the cloud and returns structured items | Upload a receipt image from the UI → UI sends image to OCR API → wait for processed results → check that parsed items appear in the UI | 1. UI displays “Processing receipt…” while waiting.<br>2. Cloud OCR endpoint receives the image payload.<br>3. OCR response returns structured item data (name, quantity, category).<br>4. UI inventory screen displays extracted items as new entries. |
| 2 | Validate barcode scanning triggers cloud barcode decoder and returns product metadata | Open barcode scanner → scan a valid UPC/EAN barcode → UI sends scanned code to backend → verify returned product data is displayed | 1. UI camera opens and captures the barcode.<br>2. Cloud barcode-decoder API receives the barcode string.<br>3. API returns correct product info (name, category, typical expiration estimate).<br>4. UI auto-populates the Add Item form with the returned data. |
| 3 | Validate offline item creation syncs to cloud when network is restored | Turn off network → add a new item manually in UI → reconnect → allow system to sync → verify item appears in cloud DB and UI refreshes | 1. Item is stored locally in SQLite while offline.<br>2. When network returns, sync job sends the item to the cloud.<br>3. Cloud database now contains the same item with correct timestamps.<br>4. UI receives updated inventory state from cloud without duplicates. |
| 4 | Validate cloud-based expiration alert scheduling correctly delivers notifications to UI | Create an item with expiration 3 days away → UI sends item to backend → cloud scheduler registers alert → check UI receives notification | 1. Cloud scheduler receives item expiration date and user alert preference.<br>2. Alert job is scheduled and queued within <2 seconds of item creation.<br>3. Push notification is delivered near the due time.<br>4. UI displays the alert in the notifications panel with correct item name and countdown. |
| 5 | Validate recipe suggestion feature integrates with external recipe API and returns results to UI | Open “Recipe Suggestions” → UI sends near-expiry ingredient list to cloud → cloud queries external recipe API → results displayed | 1. UI sends filtered ingredient list to backend.<br>2. Backend successfully queries TheMealDB (or mock service).<br>3. Cloud returns recipe titles + image URLs + steps.<br>4. UI renders recipe cards containing the returned data. |

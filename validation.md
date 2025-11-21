---
title: "Validation Checks"
layout: post
---
In this phase, we verify that the completed software satisfies all functional and non-functional requirements. The tests examine every module together to confirm the system behaves correctly from end to end.

# Test Case Skeletons

| Test Case ID | Test Description | Expected Results | Requirement Satisfied |
|--------------|------------------|------------------|------------------------|
| V-1 | User scans a grocery receipt containing multiple items | All items are correctly parsed, categorized, assigned expiration estimates, and appear in the user’s inventory | FR-1 |
| V-2a | User scans an item barcode with full product data available | Item is added with product name, category, and expiration date (from database) | FR-2 |
| V-2b | User scans a barcode that lacks expiration info | Item is added and expiration date is estimated using shelf-life rules | FR-6 |
| V-3 | User manually enters a new item (name, quantity, category, and expiration) | Item appears in inventory with all fields stored accurately | FR-3, FR-5 |
| V-4a | User edits an item (for example, adjusts expiration date or quantity) | The updated values appear when viewing the item | FR-4 |
| V-4b | User deletes an item from inventory | Item is removed and is not displayed in inventory lists | FR-4 |
| V-5 | System stores item metadata for new/updated items | Item record includes name, category, purchase date, quantity, expiration, location | FR-5 |
| V-6 | System categorizes items during receipt scan or manual entry | Items appear in the correct category grouping | FR-7 |
| V-7a | Item is within user-configured expiration alert window | User receives a push notification at the correct time | FR-8, FR-9 |
| V-7b | User changes the alert timing preference | Notifications are rescheduled and delivered according to the new setting | FR-9 |
| V-8a | Daily summary is generated when multiple items are nearing expiration | Summary list accurately displays all near-expiration items | FR-10 |
| V-8b | Weekly summary is generated when scheduled | Summary contains accurate week-ahead expiring items | FR-10 |
| V-9a | User views recipe suggestions for an item near expiration | Recipes match ingredients in inventory and load successfully | FR-11 |
| V-9b | External recipe API is temporarily unavailable | App defaults to cached/basic recipes | FR-11|
| V-10a | User searches inventory by keyword | Only items whose names match the keyword are shown | FR-12 |
| V-10b | User filters by category or storage location | Only items in that category/location appear | FR-12 |
| V-11a | User marks an item as consumed | Item status updates to Consumed and disappears from active inventory | FR-13 |
| V-11b | User marks an item as donated or wasted | Item transitions to the correct lifecycle state and is logged | FR-13 |
| V-12 | User opens the insights dashboard | Summary correctly shows waste history, usage patterns, and trends | FR-14 |
| V-13 | User interacts with UI on a mid-tier device | Response time is < 1 second at 95th percentile | NFR-1a |
| V-14a | Item is created or updated; system must schedule expiry alert | System enqueues alert within < 2 seconds after write | NFR-1b |
| V-14b | Scheduled expiration alert time arrives | Notification delivered within 60 seconds of scheduled fire time | NFR-1b |
| V-15 | User data is stored locally and synced to cloud | All verified traffic is encrypted (TLS) and local store is encrypted (AES) | NFR-2 |
| V-16 | User loses connection while editing items | All changes function normally and sync correctly once online | NFR-3 |
| V-17 | User scans barcodes: UPC, EAN, Code128 | All formats decode successfully | NFR-4 |
| V-18 | User navigates via screen reader, high contrast mode, large text | All screens readable and operable per WCAG 2.1 AA | NFR-5 |
| V-19 | Full system exercised on mobile and desktop | All application features fully function on each platform | NFR-7 |
| V-20 | User creates, updates, and deletes items rapidly | System maintains consistent inventory; duplicates merged automatically | NFR-8 |
| V-21 | User performs scanning, editing, and item consumption | System records events, traces workflows, and logs errors properly | NFR-9 |

NFR-6 is the only non-functional requirement that could not be directly testable and were not included in these validation checks because the requirement was an architectual quality and not directly end-to-end testable.
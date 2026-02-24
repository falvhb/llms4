---
name: zakk.de navigation
description: Instructions for navigating zakk.de to fetch the next events and detail pages.
---

# zakk.de Navigation Skill

Use these instructions to fetch event data from zakk.de.

## 1. Get Event List
- **URL**: `https://www.zakk.de/programm/alle`
- **Main Container**: Each event is wrapped in an `li.single-ticket`.
- **Selectors**:
    - **Date**: `.ticket-date` (child of `li.single-ticket`)
    - **Content Container**: `.ticket-content` (sibling of `.ticket-date`, child of `li.single-ticket`)
    - **Title**: `.ticket-info a` (inside `.ticket-content`)
    - **Link**: `.ticket-info a` (href inside `.ticket-content`)

## 2. Get Event Details
- **URL**: Found in the **Link** from the event list.
- **Selectors**:
    - **Title**: `.event-content-info h2`
    - **Description**: `.event-additional`
    - **Details Container**: `.event-overview`
    - **Time/Price/Venue**: Search within `.event-overview` for text labels like "Uhr", "VVK", "Einlass".
    - **Tickets**: `.tickets` (if present)

## 3. Simple Workflow
1. Navigate to `/programm/alle`.
2. Loop through all `.ticket-content` elements.
3. Extract Title, Link, and Date.
4. For each Link, navigate to the detail page.
5. Extract Description and Pricing from the detail selectors.

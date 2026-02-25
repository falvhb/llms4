---
name: zakk.de navigation
description: Instructions for navigating zakk.de to fetch the next events and detail pages.
---

# zakk.de Navigation Skill

Use these instructions to fetch event data from zakk.de.

## 1. Bulk Extraction Hints (Low-level LLMs)
If you cannot process complex CSS selectors, use these hints to find the core content:
- **Bulk Target**: Extract all text and links inside the `<main>` tag. This container holds all relevant event data.
- **Patterns**:
    - Look for repeated `<li>` blocks for the event list.
    - Inside these blocks, the first link usually points to the event detail page.
    - Dates are typically short strings at the beginning of these blocks (e.g., "Mo. 10.3.").

## 2. Detailed Selectors (High-level LLMs)
Use these specific selectors for precise data extraction.

### Event List
- **URL**: `https://www.zakk.de/programm/alle` (for all events)
- **Categories**: You can filter by category by changing the URL suffix:
    - **Musik**: `/programm/musik`
    - **Wort & Bühne**: `/programm/wort-und-buehne`
    - **Party**: `/programm/party`
    - **Politik & Gesellschaft**: `/programm/politik-und-gesellschaft`
    - **Interkultur**: `/programm/interkultur`
- **Main Container**: Each event is wrapped in an `li.single-ticket`.
- **Selectors**:
    - **Date**: `.ticket-date` (child of `li.single-ticket`)
    - **Content Container**: `.ticket-content` (sibling of `.ticket-date`, child of `li.single-ticket`)
    - **Title**: `.ticket-info a` (inside `.ticket-content`)
    - **Link**: `.ticket-info a` (href inside `.ticket-content`)

### Event Details
- **URL**: Found in the **Link** from the event list.
- **Selectors**:
    - **Title**: `.event-content-info h2`
    - **Description**: `.event-additional`
    - **Details Container**: `.event-overview`
    - **Time/Price/Venue**: Search within `.event-overview` for text labels like "Uhr", "VVK", "Einlass".
    - **Tickets**: `.tickets` (if present)

## 3. Simple Workflow
1. Navigate to `/programm/alle`.
2. Loop through all `li.single-ticket` elements.
3. Extract Title, Link, and Date.
4. For each Link, navigate to the detail page.
5. Extract Description and Pricing from the detail selectors.

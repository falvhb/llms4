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
  - You can also ready all events without details at `https://zakk.de/tickets/`
- **Categories**: You can filter by category by changing the URL suffix:
    - **Musik**: `/programm/musik`
    - **Wort & Bühne**: `/programm/wort-und-buehne`
    - **Party**: `/programm/party`
    - **Politik & Gesellschaft**: `/programm/politik-und-gesellschaft`
    - **Interkultur**: `/programm/interkultur`


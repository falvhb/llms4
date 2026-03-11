# zakk Düsseldorf

## Context
**zakk** (Zentrum für Aktion, Kultur und Kommunikation) is a sociocultural center in Düsseldorf, Germany. It hosts concerts, readings, political debates, parties, and workshops.

## Base URL
`https://zakk.de`

## Core Tasks & URLs

### 1. Finding Events (The Program)
To find upcoming events, do not check the root page. Use the specific program pages to get a list.

*   **Complete Calendar (Best Start):**
    `https://zakk.de/programm/alle`
    *Use this to see everything chronologically.*

*   **Music & Concerts:**
    `https://zakk.de/programm/musik`

*   **Literature, Comedy & Cabaret:**
    `https://zakk.de/programm/wort-und-buehne`

*   **Politics & Society:**
    `https://zakk.de/programm/politik-und-gesellschaft`

*   **Parties & Disco:**
    `https://zakk.de/programm/party`

### 2. Getting Event Details
When viewing a list of events on the pages above, you will see links to specific events.
*   **Link Format:** The links usually look like this: `/event-detail?event=12345`
*   **Instruction:** To get the full description, start time, and ticket prices, append that link to the base URL (e.g., `https://zakk.de/event-detail?event=12345`) and extract the text from that page.

### 3. General Information
*   **Food & Drinks:** `https://zakk.de/gastronomie/kneipe-biergarten`
*   **Tickets & FAQ:** `https://zakk.de/service/faq`
*   **Opening Hours/Contact:** `https://zakk.de/kontakt/kontaktliste`

## How to Parse the Content
Since this is a standard HTML website (not an API):
1.  **Visit** one of the Program URLs listed above.
2.  **Identify** the list of events. Each item usually contains a Date, a Title, and a "More Info" link.
3.  **Follow** the "More Info" link (containing `?event=...`) to read the full program text.
4.  **Note:** If a page appears empty or says "leer", there are no upcoming events in that specific category.

## Project Archives
If the user asks about social projects, workshops, or community work (not specific date-based events), search here:
*   **Current Projects:** `https://zakk.de/projekte/aktuelle-projekte`
*   **Archive:** `https://zakk.de/projekte/projektarchiv`
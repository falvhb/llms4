# Tonhalle Düsseldorf - Context & Scope

## What is the Tonhalle Düsseldorf?
The Tonhalle Düsseldorf is a major concert hall located in Düsseldorf, Germany. It is primarily dedicated to classical music and is the home venue of the Düsseldorfer Symphoniker. The venue also hosts jazz, pop, cabaret, and family concerts.

## What Data is Available?
By accessing the API and website, an agent can discover:
*   **Complete Schedules:** A comprehensive calendar of all upcoming performances for current and future seasons.
*   **Event Metadata:** Specific details including artists (soloists, conductors), program titles, and exact dates/times.
*   **Categorization:** Events are organized into specific "Series" (e.g., "Ignition" for young adults, "Sternzeichen" for symphonic concerts, "Piano Solo" for recitals).
*   **Booking Information:** Direct links to ticket purchase pages. Do not book directly, but show the user a link instead


## Hints

- The website is a SPA mess, fetching html directly does only work for detail pages
- Use the Event API to browse events
- Use a external search engine for searching specific content. no internal search available
- If checking for user interest for an event, always fetch the details to validate it

## Goal
Retrieve concert dates, ticket links, and event details for the Tonhalle Düsseldorf.

## Event List API
**URL:** `https://www.tonhalle.de/api/events`
**Method:** GET

### Required Parameters
*   **type**: Always set this to "dynamic".
*   **size**: Set to "100" to get a large list of events.
*   **from**: Pagination offset. Start at "0". To see the next page, increase this number (e.g., 100, 200).

### Optional Filter Parameters
*   **dateFrom**: Start date as a Unix Timestamp (seconds).
*   **dateUntil**: End date as a Unix Timestamp (seconds).
*   **series**: Filter by category. You can use this parameter multiple times. **Important:** You must URL-encode the values (e.g., "Piano Solo" becomes "Piano%20Solo").

### Valid Values for Series Filter
*   Himmelblau
*   Plutino
*   Sterntaler
*   Sternschnuppen
*   Junior-Sternzeichen
*   Ultraschall
*   Tonhalle geht aus
*   Komet
*   Sternzeichen
*   Meisterkonzerte
*   Piano Solo
*   Heinersdorff - Sonderkonzerte
*   Faszination Klassik
*   Na hör'n Sie mal
*   Fixsterne
*   Sternstunden
*   Ehring geht ins Konzert
*   #IGNITION

## Interpreting the Results
The API returns a list of events. Look for these fields:
*   **title**: The name of the artist or concert.
*   **subTitle**: A description or program name.
*   **isoDateStrings**: A list of dates and times in ISO format.
*   **ticketLink**: Direct URL to buy tickets.
*   **link**: A relative path to the full event page (e.g., /veranstaltung/komet/123-event).

## How to Get Full Event Details
The API list only provides summaries. To get the full text description:
1.  Get the **link** field from the event list (e.g., /veranstaltung/example).
2.  Combine it with the base URL: `https://www.tonhalle.de` + `link`.
3.  Visit that URL and extract the text content from the page.
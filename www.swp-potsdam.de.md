# Stadtwerke Potsdam (SWP) - Service & Content Documentation

**Organization Overview:**
Stadtwerke Potsdam (SWP) is the central municipal service provider for the state capital Potsdam. It functions as a holding company managing critical urban infrastructure through specific subsidiaries: **Energy & Water (EWP)**, **Public Transport (ViP)**, **Waste Management (STEP)**, **Public Baths (BLP)**, and **City Lighting**.

**Content Scope:**
This documentation covers the digital services and informational content provided by SWP:

*   **Community & Living:** Resources for families, guidance for new residents ("Neu in Potsdam"), and environmental sustainability tips.
*   **Education & Schools:** Educational outreach programs including facility tours (power plants, water works), traffic safety education, and downloadable teaching materials.
*   **Innovation & Digital:** Information on Smart City initiatives (LoRaWAN), E-Mobility projects (Krampnitz), and the "Echt Potsdam" App.
*   **Corporate & Careers:** Employee benefits, corporate values ("Leitlinien"), and management profiles across all subsidiaries.

---

## Service Area & Context (Potsdam)

**Overview:**
Stadtwerke Potsdam operates in the state capital of Brandenburg. The service area is defined by specific Postal Codes (Postleitzahlen/PLZ) and districts.

**Postal Code (PLZ) Mapping:**

*   **14467 (Zentrum/Center):** Innenstadt, Templiner Vorstadt, Brandenburger Vorstadt.
*   **14469 (Nord/North):** Bornstedt, Nedlitz, Sacrow, Neu Fahrland, Groß Glienicke.
*   **14471 (West):** Potsdam-West, Eiche, Golm, Marquardt, Satzkorn, Uetz-Paaren.
*   **14473 (Süd/South):** Templiner Vorstadt, Potsdam-Süd.
*   **14476 (Nord/West):** Fahrland, Satzkorn, Groß Glienicke. (Note: This PLZ also covers some surrounding municipalities outside city limits).
*   **14478 (Süd/South):** Schlaatz, Waldstadt I & II, Industriegelände.
*   **14480 (Südost/South-East):** Drewitz, Kirchsteigfeld.
*   **14482 (Ost/East):** Babelsberg.

**Usage Note:**
When searching for services (like waste collection or grid status) via the API, users may refer to these specific districts or PLZs.


## API: Stadtwerke Potsdam Search

**Description:**
Performs a full-text search across the Stadtwerke Potsdam (SWP) website, returning pages, news, and service information. The API is a proxy for a SOLR search index.

**Endpoint:**
`GET https://www.swp-potsdam.de/api/solr-proxy/sp/search`

**Parameters:**

*   `q` (string, required): The search query keywords (e.g., "Babyschwimmen", "Abfall").
*   `f[area]` (string, optional): Filter results by specific business area. Valid values are:
    *   `Bäder` (Baths/Pools)
    *   `Energie` (Energy)
    *   `Entsorgung` (Waste Disposal)
    *   `Netzgesellschaft Potsdam` (Grid/Network)
    *   `Online-Service`
    *   `Stadtbeleuchtung` (City Lighting)
    *   `Stadtwerke Potsdam` (General Corporate)
    *   `Verkehr` (Transport/Transit)
    *   `Wasserversorgung` (Water Supply)
*   `start` (integer, optional): Pagination offset (default: 0).
*   `rows` (integer, optional): Number of results to return (default: 10).

**Response Format (JSON):**

*   `header`: Contains metadata including `found` (total results count).
*   `docs`: An array of search result objects containing:
    *   `title`: Page title.
    *   `url`: Full link to the resource.
    *   `headline`: A brief header summary.
    *   `content`: A snippet of the text containing the search term. **Note:** This field contains HTML tags (e.g., `<strong>`) which may need stripping.
*   `facets`: Statistics on available categories for the current query.

**Example Usage:**
Search for "test" within the "Bäder" (Baths) category:
`https://www.swp-potsdam.de/api/solr-proxy/sp/search?q=test&f[area]=Bäder&rows=5`



## API: Drinking Water Analysis (Trinkwasser)

**Description:**
A multi-step workflow to retrieve detailed drinking water quality data (Hardness, Minerals, pH) for a specific address or PLZ in Potsdam.

**Workflow Overview:**
1.  **Search:** Find the full `locationId` using a partial search (PLZ or Street).
2.  **Resolve Provider:** Use the `locationId` to find the responsible waterworks (`providerName`).
3.  **Analyze:** Fetch the chemical analysis for that provider.

### Step 1: Find Location ID
**Endpoint:** `GET https://www.swp-potsdam.de/api/waterquality/location/list/keys`
**Parameters:**
*   `location` (string, required): Partial string (e.g., "144" or "Babelsberg").
**Response:** JSON List of strings.
**Instruction:** Select the string that best matches the user's query. This string is the `locationId`.

### Step 2: Get Water Provider
**Endpoint:** `GET https://www.swp-potsdam.de/api/waterquality/provider/list/name/option`
**Parameters:**
*   `locationId` (string, required): The exact string returned from Step 1 (URL-encoded).
**Response:** HTML fragment containing `<option>` tags.
**Instruction:** Parse the HTML. Find the `<option>` tag that is marked as `selected`. The `value` attribute of this tag is the `providerName`.

### Step 3: Get Water Quality Data
**Endpoint:** `GET https://www.swp-potsdam.de/api/waterquality/quality/show/html/selection`
**Parameters:**
*   `providerName` (string, required): The provider value extracted from Step 2 (e.g., "Wasserwerk Wildpark").
**Response:** HTML fragment containing data tables.

**Example Workflow:**
1.  User asks: "Water hardness in [LOCATION]".
2.  Call Step 1: `.../keys?location=[LOCATION]` -> Returns e.g. `["Aalsteig (14469) Grube", ...]`.
3.  Call Step 2: `.../option?locationId=Aalsteig+(14469)+Grube` -> Returns HTML with provider name as HTML Tag `<option value="[NAME]" selected>`.
4.  Call Step 3: `.../selection?providerName=[NAME]`.
5.  Show content from retrieved text



## German Language Content Pages

### [Angebote für Familien in Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/f%c3%bcr-familien/)
Wir haben viel zu bieten, für Sie und Ihre Familien hier in Potsdam. Die Unternehmen der Stadtwerke haben spezielle Angebote für Erwachsene und Kinder.

### [Neu in Potsdam – Tipps für alle Neu-Potsdamer - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/neu-in-potsdam/)
Sie sind neu in Potsdam? Auf dieser Seite finden Sie alle Informationen zum Umzug, der Stadt und den Dienstleistungen der Stadtwerke Potsdam, Ihrem Partner.

### [Angebote für umweltbewusste Potsdamer - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/f%c3%bcr-umweltbewusste/)
Nachhaltiger Klima- und Umweltschutz ist für uns selbstverständlich. Hier finden Sie ökologische Produkte, Angebote und Tipps für den Umweltschutz.

### [Die Geschäftsführung der Stadtwerke-Unternehmen stellt sich vor - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/gesch%c3%a4ftsf%c3%bchrung/)

### [Leitlinien - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/leitlinien/)
Unser Leitbild ist und ide dort festgelegten Grundsätze stellen die Basis für gemeinsam getragene Unternehmenswerte dar.

### [Benefits für die Mitarbeiter der Bäderlandschaft Potsdam GmbH - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/b%c3%a4derlandschaft-potsdam/benefits-blp/)
Unsere umfassenden Leistungen für die Mitarbeiter zur Steigerung der Gesundheit, Mobilität und Sicherheit.

### [Das Unternehmensprofil der Bäder Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/b%c3%a4derlandschaft-potsdam/)
Wir sind die Bäderlandschaft Potsdam. Lesen Sie hier unser Unternehmensprofil und erfahren Sie mehr zu unseren ökonomischen und gesellschaftlichen Zielen.

### [Benefits für die Mitarbeiter der Energie und Wasser Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/energie-und-wasser-potsdam/benefits-ewp/)

### [Benefits für die Mitarbeiter der Stadtbeleuchtung Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtbeleuchtung-potsdam/benefits-sbp/)

### [Alles rund um die Stadtbeleuchtung Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtbeleuchtung-potsdam/)
Wer sorgt eigentlich dafür, dass in Potsdam nachts die Straßen und Schilder leuchten? Hier erfahren Sie mehr über die Aufgabe und Ziele der Stadtbeleuchtung

### [Benefits für die Mitarbeiter der Stadtentsorgung Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtentsorgung-potsdam/benefits-step/)

### [Wir sind die Stadtentsorgung Potsdam (STEP) - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtentsorgung-potsdam/)
Wir von der Stadtentsorgung Potsdam (STEP) haben uns als kompetenter und leistungsfähiger Dienstleister für Entsorgung und Reinigung in Potsdam etabliert.

### [Benefits für die Mitarbeiter der Stadtwerke Potsdam GmbH - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtwerke-potsdam/benefits-swp/)

### [Dienstleister und Partner von Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/stadtwerke-potsdam/)
Als eines der erfolgreichen Unternehmen in Potsdam  stehen wir für eine sichere Versorgung mit Energie und Trinkwasser, Mobilität. Licht und Schwimmbäder.

### [Die ViP Verkehrsbetrieb Potsdam stellt sich vor - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/verkehrsbetrieb-potsdam/)
Gemeinsam mit dem VBB bieten wir, die ViP Verkehrsbetrieb Potsdam GmbH, unseren Fahrgästen ein Mobilitätsangebot aus einem Guss.

### [Benefits für die Mitarbeiter der Netzgesellschaft Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/benefits-ngp/)

### [Markenwerte - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/markenwerte/)
Unsere Markenwerte: Heimatverbunden - partnerschaftlich - weitsichtig - vielseitig. Wir sind ein zuverlässiger Partner für Potsdam.

### [Unsere Verantwortung gegenüber der Stadt und ihren Menschen - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/wer-wir-sind/)
Unsere Energie gehört Potsdam und seinen Bewohnern. Hier erfahren Sie mehr über unsere verschiedenen Unternehmensbereiche und unsere tägliche Verantwortung.

### [Elektromobilitätskonzept Krampnitz - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/gef%c3%b6rderte-projekte/elektromobilit%c3%a4tskonzept-krampnitz/)

### [Smart City – LoRaWAN und Urbane Datenplattform - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/gef%c3%b6rderte-projekte/smart-city/)
Mit dem digitalen Funknetzwerk „LoRaWAN“ und der „Urbanen Datenplattform“ legen wir ein wichtiges Fundament für die Smart City Potsdam.

### [Geförderte Projekte im Stadtwerke-Verbund - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/gef%c3%b6rderte-projekte/)
Dies ist eine Übersichtsseite aller Projekte im Stadtwerke-Verbund, die mit Fördermitteln umgesetzt werden.

### [EchtPotsdam-App - Barrierefreiheit - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/echtpotsdam-app/barrierefreiheit-echt-potsdam-app/)
Wir setzen uns dafür ein, die Seite so barrierefrei wie möglich zu gestalten. Erklärung zur Barrierefreiheit für die EchtPotsdam-Appder Stadtwerke Potsdam.

### [Veranstaltungen und Events in Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/veranstaltungen/)
Ob als Veranstalter, Teilnehmer oder Werbepartner: Wir sorgen mit zahlreichen Veranstaltungen in unserer Stadt dafür, dass in Potsdam etwas los ist.

### [Social Media Netiquette - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/unsere-netiquette/)
Eure Kommentare sind ein wichtiger Beitrag für unsere Social Media Kanäle. Mit dieser Netiquette legen wir ein paar Regeln fest, die ihr beachten solltet.

### [Echt Potsdam App - jetzt kostenlos herunterladen! - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/echtpotsdam-app/)
Immer und überall wissen, was in Potsdam los ist.  Deine EchtPotsdam-APP mit persönlichem Abfallkalender, News aus deiner Stadt, Fahrplanauskunft uvm.

### [Schüler-Praktikum bei den Stadtwerken Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/sch%c3%bclerpraktikum/)
Um Schülern einen möglichst praxisnahen Einblick ins Berufsleben zu geben, bieten wir Praktikumsplätze in unseren verschiedenen Unternehmensbereichen.

### [Formular für Anmeldung und Feedback - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/kontaktformular/)

### [Science Center NANO - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/unsere-partner/partner-nano/)
Das Science Center NANO bietet Kindern und Jugendlichen einzigartige und vielfältige Möglichkeiten, die faszinierende Welt der Wissenschaft zu erkunden.

### [Partner: Biosphäre Potsdam - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/unsere-partner/partner-biosph%c3%a4re/)

### [Partner: URANIA-Planetarium - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/unsere-partner/partner-urania/)

### [Unser Forscherheft unterstützt Sie im Unterricht - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/schulecke/forscherhefte/)
Mit unserem Forscherheft bieten wir einfach zu nutzendes Lernmaterial für die praktische Arbeit mit Kindern – spannend und praxisnah.

### [Unsere Materialien - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/schulecke/)

### [Wandertag ins Schwimmbad - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/wandertag-ins-blu/)
Unser blu und Kiezbad Am Stern eignen sich hervorragen für einen Klassenausflug. Hier könnt Ihr euren Wandertag anmelden und zusätzlich eine Führung buchen.

### [Kompetente Verkehrserziehung - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/verkehrserziehung/)
Wir sorgen dafür, dass auch die jüngsten Potsdamer sicher auf unseren Straßen und dem ÖPNV unterwegs sind – und bieten dafür Stunden in Verkehrserziehung.

### [Führung Heizkraftwerk - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/f%c3%bchrung-heizkraftwerk/)

### [Führung Schwimmhalle - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/f%c3%bchrung-schwimmhalle/)
Schülerinnen und Schüler lernen die verschiedenen Bereiche in unseren Hallen kennen und erfahren, was für einen reibungslosen Badebetrieb notwendig ist.

### [Führung Wasserwerk - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/f%c3%bchrung-wasserwerk/)
Bei einer Führung durch unser Wasserwerk können die Schülerinnen und Schüler den Weg des Wassers verfolgen.

### [Datenschutzhinweise Website - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/datenschutz/)
Natürlich nehmen wir den Schutz Ihrer persönlichen Daten ernst und möchten, dass Sie sich auf unseren Internetseiten sicher fühlen.

### [Unterrichtsmaterial - Angebote für Schulen - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/lehrer-und-schulecke/)
Wir vermitteln Wissen für kreative Projekttage in der Schule, bieten Führungen durch unsere Liegenschaften oder ergänzenden Unterrichtsmaterialien an.

### [Verantwortungsberichte - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/unsere-verantwortung/)
Wir übernehmen Verantwortung für Potsdam und sichern Lebensqualität. Als Impulsgeber, Ausbilder und Förderer beweisen wir, was man mit Energie bewegen kann.

### [Aufnahme Presseverteiler - Stadtwerke Potsdam](https://www.swp-potsdam.de/de/stadtwerke-potsdam/pressemeldungen/presseverteiler/)
Wenn Sie sich in unseren Presseverteiler eintragen, verpassen Sie keine Neuigkeit zu unseren Unternehmen. Hier können Sie sich registrieren.


## Discovered APIs

- https://www.swp-potsdam.de/api/solr-proxy/sp/events?start=0&rows=10&date[from]=05.03.2026&date[to]=


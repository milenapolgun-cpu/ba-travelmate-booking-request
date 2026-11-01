# Business Requirements Document (BRD)

**TravelMate Hotel Search and Filtering Module**

October 2026

Version 1.1

TravelMate

## 1 Document Revisions

| Date | Version Number | Document Changes |
|---|---|---|
| 23.10.2026 | 1.0 | Initial version of the document |
| 26.10.2026 | 1.1 | Functional and non-functional requirements revised: traceability to stakeholder requirements added (“Implements SR” column); requirements for filters, hotel card, recommendations, multi-currency and multi-language support refined; glossary updated |

## 2 Approvals

| Role | Name | Title | Signature | Date |
|---|---|---|---|---|
| Project Sponsor |  |  |  |  |
| Business Owner |  |  |  |  |
| Project Manager |  |  |  |  |
| System Architect |  |  |  |  |
| Development Lead |  |  |  |  |
| UX Lead |  |  |  |  |
| QA Lead |  |  |  |  |
| Content Lead |  |  |  |  |

## 3 Introduction

### 3.1 Project Summary

#### 3.1.1 Objectives

**Primary objective:**

Increase search-to-booking conversion from 3% to 5% within 6 months after release by introducing hotel recommendations and marketing tags in search results.

**Secondary objectives:**

- Ensure up-to-date search results: hotels with no available rooms are removed from the results or receive the “No rooms available” status no later than 2 seconds after an availability change.
- Collect data on searches that return no results in order to expand the hotel base in under-served segments.
- Support multiple currencies and language versions to enter new markets.

#### 3.1.2 Background

Currently, the TravelMate system generates search results solely based on the filters and sorting selected by the user. The system does not take into account the commercial interests of the platform: partnership agreements with hotels and different sales margins.

As a result, offers that are profitable for the business are not prioritized in the results. A user who does not quickly find a suitable option may leave the platform without booking. The current search-to-booking conversion rate is 3%.

The following additional problems have been identified:

- room availability data is not synchronized in real time, so hotels with no available rooms may be shown in the results;
- there is no data on which filter combinations return no results for users, which makes it difficult to develop the hotel base;
- the platform does not support multiple currencies and languages, which limits the audience.

Introducing recommendations, marketing tags and real-time data synchronization will increase conversion, reduce bounce rate and lay the foundation for scaling the platform.

##### 3.1.2.1 Business Drivers

The development of this feature is driven by the following business needs:

- Revenue growth through higher search-to-booking conversion
- Higher sales margins through prioritized display of profitable offers
- Fulfillment of obligations under partnership agreements with hotels
- Fewer bounces and complaints related to booking unavailable rooms
- Data on unmet demand to develop the partner network
- Entry into new markets through multi-currency and multi-language support

### 3.2 Project Scope

| In Scope | Out of Scope |
|---|---|
| The guest can enter a destination, and the system suggests autocomplete options (city, district, landmark) | Booking checkout and entry of guest details |
| The guest can select check-in and check-out dates in a calendar where the system highlights weekends | Payment and refund processing |
| The guest can specify the number of adults, children and the age of each child | Detailed hotel page (gallery, room descriptions, full reviews) |
| The guest can filter results by price per night using a slider (minimum and maximum value) | Writing and moderating reviews |
| The guest can filter results by review score (8+) and by free cancellation | Amenity filters other than Wi-Fi, parking and breakfast (e.g., pool, gym, transfer, pets allowed) |
| The guest can filter results by amenities: Wi-Fi, parking, breakfast | Development of a proprietary Channel Manager (integration only) |
| The system places recommended hotels (based on margin and partnership agreements) that match the search parameters and filters selected by the guest at the top of the search results list | Loyalty programs and promo codes |
| The system displays accurate marketing tags on the hotel card based on up-to-date data | Chat between the guest and the hotel |
| The system displays a hotel card with the total price for the entire stay, a photo of the main room and the distance to the city center |  |
| The guest can view the hotel location on a map using the “Show on map” button placed next to each hotel in the search results list and on the hotel card |  |
| The system synchronizes room availability in real time via API / Channel Manager with a maximum synchronization delay of 2 seconds |  |
| The system logs searches with no results and generates a report on them |  |
| The system supports multiple currencies: the guest can select the currency in which prices are displayed |  |
| The system supports multiple languages: the guest can select the interface language |  |

### 3.3 System Perspective

The hotel search and filtering module is part of the front end of the TravelMate platform and covers the user journey from entering the destination and dates to selecting a specific hotel from the results list.

The module interacts with the following systems:

| № | Component | Purpose |
|---|---|---|
| 1 | Hotel database | Hotel descriptions, amenities, photos, coordinates, review scores |
| 2 | Channel Manager / hotel APIs | Up-to-date room availability and prices in real time |
| 3 | Maps and geolocation service | Destination autocomplete in the selected language, hotel display on the map, calculation of distance to the city center |
| 4 | Exchange rate service | Conversion of prices into the selected currency |
| 5 | Recommendation module | Identification of recommended hotels based on margin and partnership agreements |
| 6 | Localization module | Storage of translations of interface elements and reference data (amenities, statuses, tags) into supported languages |
| 7 | Analytics system | Collection of data on searches with no results and generation of reports |
| 8 | Hotel page (adjacent module, out of scope) | Transfer of the selected hotel and search parameters (dates, number of guests, children’s ages) for further viewing and booking |

#### 3.3.1 Assumptions, Constraints, Risks and Issues

| Category | Description |
|---|---|
| **Assumptions** |  |
| Assumption 1 | Hotels are connected to a Channel Manager or provide an API for transmitting room availability data |
| Assumption 2 | Margin and partnership agreement data is available in the system and kept up to date by the business owner |
| Assumption 3 | The acceptable availability synchronization delay is no more than 2 seconds |
| Assumption 4 | For each hotel, the database contains coordinates, review score, amenity information and a photo of the main room |
| **Constraints** |  |
| Constraint 1 | Marketing tags are displayed only based on up-to-date room availability and price data (consumer protection law requirements) |
| Constraint 2 | The solution must comply with the existing architecture and database structure of the TravelMate platform |
| Constraint 3 | Integration is possible only with Channel Managers that provide an API |
| **Risks** |  |
| Risk 1 | Delays or failures of hotel APIs may result in unavailable rooms being displayed and double bookings |
| Risk 2 | Inaccurate marketing tags may have legal and reputational consequences |
| Risk 3 | During peak load, the time to display results may exceed 2 seconds |
| **Issues** |  |
| Issue 1 | Not all hotels are connected to a Channel Manager — a method of updating availability for such hotels needs to be defined |
| Issue 2 | The list of supported currencies and languages for the first release has not been defined |

## 4 Stakeholder Requirements

| ID | Requirement |
|---|---|
| SR-001 | The guest must be able to quickly specify the destination (with autocomplete), dates (in a calendar with highlighted weekends) and the party composition, including children’s ages |
| SR-002 | The guest must receive search results almost instantly (within 1–2 seconds) |
| SR-003 | The guest must be able to filter results by price per night using a slider |
| SR-004 | The guest must be able to filter results by review score (8+) and by free cancellation |
| SR-005 | The guest must be able to filter results by amenities: Wi-Fi, parking, breakfast |
| SR-006 | The guest must see on the hotel card the total price for the entire stay, a photo of the main room and the distance to the city center |
| SR-007 | The guest must be able to view the hotel location on a map using a button placed next to each hotel in the search results list and on the hotel card |
| SR-008 | The system must place recommended hotels (based on margin and partnership agreements) that match the search parameters and filters selected by the guest at the top of the search results list |
| SR-009 | The system must display marketing tags on hotel cards (e.g., “Only 1 room left”, “Best price”) that are accurate and based on up-to-date room availability and price data |
| SR-010 | Search results must reflect up-to-date room availability in real time with a maximum synchronization delay of 2 seconds |
| SR-011 | The system must collect data on searches for which users found no results |
| SR-012 | The system must support multiple currencies and multiple languages |

The requirements in this document are prioritized as follows:

| Value | Rating | Description |
|---|---|---|
| 1 | Critical | This requirement is critical to the success of the project. The project will not be possible without this requirement. |
| 2 | High | This requirement is high priority, but the project can be implemented at a bare minimum without this requirement. |
| 3 | Medium | This requirement is somewhat important, as it provides some value but the project can proceed without it. |
| 4 | Low | This is a low priority requirement, or a “nice to have” feature, if time and cost allow it. |
| 5 | Future | This requirement is out of scope for this project, and has been included here for a possible future release. |

### 4.1 Functional Requirements

Each functional requirement implements one or more stakeholder requirements (Section 4); the link is shown in the “Implements SR” column.

| Req. ID | Priority | Description | Rationale | Implements SR | Affected Stakeholders |
|---|---|---|---|---|---|
| **General / Core Functionality** |  |  |  |  |  |
| FR‑G‑001 | 1 | The system must provide a destination input field; after 2 characters are entered, the system displays autocomplete suggestions grouped by type: city, district, landmark | Users expect a “smart” input field; autocomplete speeds up input and prevents misspelled names | SR‑001 | Guest |
| FR‑G‑002 | 1 | Autocomplete suggestions must be displayed in the selected interface language; if there are no matches, the system displays the message “Destination not found” | Prevents searches for non-existent destinations; supports different language versions | SR‑001<br>SR‑012 | Guest |
| FR‑G‑003 | 1 | The system must provide a calendar for selecting check-in and check-out dates in which weekends (Saturday and Sunday) are visually highlighted | Weekend rates usually differ from weekday rates; highlighting weekends helps plan the trip with price in mind | SR‑001 | Guest |
| FR‑G‑004 | 1 | The system must not allow selecting a check-in date in the past or a check-out date that is the same as or earlier than the check-in date | Prevents invalid queries and empty results caused by input errors | SR‑001 | Guest |
| FR‑G‑005 | 1 | The system must allow the guest to specify the party composition:<br> – number of adults — mandatory field, at least 1;<br> – number of children — optional field; allowed values: not specified, 0 or more;<br> – age of each child (0 to 17 years) — if the number of children is greater than 0, a separate age selection field is displayed for each child.<br>A child is a guest who is under 18 years of age on the hotel check-out date. | Children’s ages critically affect the price of the stay; the guest must see the correct price already at the search stage | SR‑001 | Guest |
| FR‑G‑006 | 1 | The system must disable the “Search” button and highlight empty fields until the following are specified:<br> – destination;<br> – check-in and check-out dates;<br> – number of adults (at least 1);<br> – age of each child, if the number of children is greater than 0.<br>If the number of children is not specified or equals 0, children’s ages are not requested, and the search is performed for adults only. | Prevents incorrect prices and availability that depend on the party composition and children’s ages; guests without children do not have to fill in unnecessary fields. | SR‑001 | Guest |
| FR‑G‑007 | 1 | From the moment the “Search” button is clicked until the search results appear, the system must display the message “Your request is being processed, please wait” | Gives the guest feedback that the request is being processed; reduces bounces while waiting | SR‑002 | Guest |
| FR‑G‑008 | 2 | The system must display search results in batches of 20 hotels and load the next batch as the list is scrolled | The first results are displayed quickly regardless of the total number of hotels found | SR‑002 | Guest |
| FR‑G‑009 | 1 | The system must display in the search results only hotels that match the search parameters (destination, check-in and check-out dates, number of adults and children, children’s ages). If the guest has applied filters, the system takes them into account together with the search parameters (AND logic): only hotels that match both the search parameters and all selected filters are displayed. If no filters are applied, all hotels matching the search parameters are displayed. | Unambiguous filtering logic: the result is clear to the guest and can be implemented unambiguously by developers | SR‑001<br>SR‑003<br>SR‑004<br>SR‑005 | Guest |
| FR‑G‑010 | 2 | The system must provide a “Reset filters” button that returns all filters to their default state | Allows the guest to quickly return to the full results without clearing each filter manually | SR‑003<br>SR‑004<br>SR‑005 | Guest |
| FR‑G‑011 | 1 | If no hotels are found for the selected parameters and filters, the system must display the message “No hotels match the selected parameters” and offer to reset the filters | Keeps the guest on the platform instead of an empty screen; captures the “search with no results” scenario | SR‑003<br>SR‑004<br>SR‑005<br>SR‑011 | Guest |
| FR‑G‑012 | 1 | The system must provide a “Price per night” filter as a slider with two handles (minimum and maximum price). The slider range is from UAH 200 to UAH 50,000, with a step of UAH 100. The leftmost position means “up to UAH 200”, the rightmost position means “UAH 50,000 and above”. The selected price range is displayed above the slider (e.g., “UAH 1,500 – 8,000”). For other currencies, the limits and step are set in the settings as rounded values equivalent to the UAH values. | One of the three most popular filters; helps quickly find options within budget. A fixed range simplifies implementation and testing, and open-ended extreme values do not exclude hotels cheaper than UAH 200 or more expensive than UAH 50,000 from the search. | SR‑003<br>SR‑012 | Guest |
| FR‑G‑013 | 1 | The “Price per night” filter must be applied to the price calculated based on the party composition and children’s ages, and displayed in the selected currency | The guest filters by the price they will actually pay, in their preferred currency | SR‑003<br>SR‑012 | Guest |
| FR‑G‑014 | 1 | The system must provide a “Review score” filter with a single-choice selection: “Any”, “8+”, “9+”; hotels with a score not lower than the selected value are displayed | One of the three most popular filters; users choose hotels with a score of 8 or higher, which increases trust in the results | SR‑004 | Guest |
| FR‑G‑015 | 1 | The system must provide a “Free cancellation” filter as a checkbox; when it is selected, only hotels that have at least one available rate with free cancellation for the selected dates are displayed | One of the three most popular filters; increases user loyalty during periods of uncertain planning | SR‑004 | Guest |
| FR‑G‑016 | 2 | For hotels with free cancellation, the card must contain the label “Free cancellation until DD.MM.YYYY” | The guest sees the cancellation terms without going to the hotel page, which reduces uncertainty when choosing | SR‑004<br>SR‑006 | Guest |
| FR‑G‑017 | 1 | The system must provide an “Amenities” filter as a set of checkboxes: “Wi-Fi”, “Parking”, “Breakfast”. Multiple checkboxes can be selected; hotels that have all the selected amenities are displayed | Users consider these amenities a critical criterion when choosing a hotel | SR‑005 | Guest |
| FR‑G‑018 | 2 | The “Breakfast” checkbox must select hotels where breakfast is included in the price in at least one rate available for the selected dates | Prevents situations where the hotel offers breakfast but it is not available in the rates for the selected dates | SR‑005 | Guest |
| FR‑G‑019 | 1 | The hotel card in the search results must contain: name, photo of the main room, review score, distance to the city center, price per night, total price for the entire stay, marketing tags, as well as the “Recommended by TravelMate” label and the free cancellation label (if applicable) | Users want to see key information immediately and compare options without going to the hotel page | SR‑006 | Guest |
| FR‑G‑020 | 1 | The system must calculate the total price as the sum of the prices for all nights of the stay for the entire party, taking children’s ages into account, including mandatory taxes and fees; next to the price, the note “for N nights, X adults, Y children” is displayed | Users want to see the total cost for the entire stay, not just per night; prevents price “surprises” at later stages | SR‑006 | Guest |
| FR‑G‑021 | 1 | The system must calculate the distance from the hotel to the city center based on coordinates and display it in kilometers with an accuracy of 0.1 km. If the guest searched by district or landmark, the distance to that point is displayed | Users assess the hotel location relative to the center or a key point of the trip | SR‑006 | Guest |
| FR‑G‑022 | 1 | When a hotel is selected in the list, the system must open the hotel page and pass the search parameters to it: dates, number of adults and children, children’s ages, selected currency and language | The guest does not have to re-enter the parameters; the journey from search to booking remains seamless | SR‑006 | Guest |
| FR‑G‑023 | 1 | The system must display a “Show on map” button next to each hotel in the results list and on the hotel card; when clicked, a map opens with the hotel marker, centered on it | Hotel location is critical for guests while traveling; users want to go to the map directly from the list | SR‑007 | Guest |
| FR‑G‑024 | 1 | On mobile devices, the map must open in full-screen mode with a “Back to list” button; when returning, the list scroll position is preserved | Searches are often done “on the go” from a phone; the guest does not lose their place in the list after viewing the map | SR‑007 | Guest |
| FR‑G‑025 | 1 | The recommendation module must select recommended hotels only from hotels that match the search parameters and all filters selected by the guest and have available rooms for the selected dates | Recommending an irrelevant or unavailable hotel reduces trust and conversion | SR‑008<br>SR‑010 | Guest, Business Owner, Partner Hotels |
| FR‑G‑026 | 1 | The system must determine the order of recommended hotels by the recommendation score, which is calculated based on margin and the terms of the partnership agreement. The weights of these factors are set in the recommendation settings | The key mechanism for achieving the project objective: prioritizing offers that are profitable for the business and fulfilling partnership agreements | SR‑008 | Guest, Business Owner, Partner Hotels |
| FR‑G‑027 | 1 | When sorting by “Recommended”, the system must display at the top of the results all recommended hotels that match the search parameters and filters, in descending order of recommendation score (FR-G-026), followed by the other hotels. A recommended hotel is not duplicated in the main list. | All profitable offers that suit the guest are prioritized in the results, which drives conversion growth and fulfillment of partnership agreements; no duplicates means the same hotel is not shown twice. | SR‑008 | Guest, Business Owner, Partner Hotels |
| FR‑G‑028 | 1 | The card of a recommended hotel must contain a green “Recommended by TravelMate” label. | The label draws attention to offers that are profitable for the business and encourages the guest to choose; naming the platform increases trust in the recommendation, and openly marking promoted offers complies with consumer protection law. | SR‑008 | Guest, Business Owner |
| FR‑G‑029 | 2 | If there are no hotels in the results that qualify for recommendation, the recommended hotels block must not be displayed | Prevents an empty block and irrelevant recommendations | SR‑008 | Guest |
| FR‑G‑030 | 1 | The system must provide sorting of results: “Recommended” (default), “Lowest price first” (by total price), “By review score”, “By distance to center”. When sorting by “Recommended”, recommended hotels are displayed at the top of the results, including after filters are applied. When another sorting option is selected, recommended hotels are positioned in the list according to the selected sorting, and the “Recommended by TravelMate” label is kept. | By default, the guest sees offers prioritized by the business; at the same time, the guest can still sort the results by their own criteria, and the label continues to draw attention to recommended hotels | SR‑008 | Guest, Business Owner |
| FR‑G‑031 | 1 | The system must display the “Only 1 room left” tag only if, according to Channel Manager data, exactly one room suitable for the party composition is available for the selected dates | The tag encourages the guest to choose; basing it on actual data eliminates legal and reputational risks | SR‑009 | Guest, Business Owner |
| FR‑G‑032 | 1 | The system must display the “Best price” tag only if the hotel’s total price is the lowest among hotels of the same category (star rating) in the current search results | The tag increases conversion; a clear comparison rule makes the tag accurate and verifiable | SR‑009 | Guest, Business Owner |
| FR‑G‑033 | 1 | The system must recalculate marketing tags on every update of room availability and price data; if the data is unavailable or outdated, the tag is not displayed | An inaccurate tag may lead to claims from guests and regulators | SR‑009<br>SR‑010 | Guest, Business Owner |
| FR‑G‑034 | 1 | The system must receive data on changes in room availability and prices from the Channel Manager / hotel APIs on a change event and update the data used for search | Prevents situations where the guest finds a hotel already booked on another platform | SR‑010 | Guest, Business Owner, Partner Hotels |
| FR‑G‑035 | 1 | The system must exclude hotels with no available rooms for the selected dates from the search results. If a hotel becomes unavailable while the guest is viewing the results, its card receives the “No rooms available” status, and navigation to the hotel page becomes unavailable | Business owner requirement for up-to-date results; increases guests’ trust in the platform | SR‑010 | Guest, Business Owner |
| FR‑G‑036 | 1 | The system must re-check room availability when a hotel is selected, before navigating to the hotel page. If rooms are sold out, the guest sees the message “No rooms left for the selected dates” and remains in the search results | Additional protection against navigating to an unavailable room between data updates | SR‑010 | Guest, Business Owner |
| FR‑G‑037 | 1 | If a hotel’s API is unavailable, the system must temporarily hide that hotel from the search results until the connection is restored | Prevents displaying a hotel with unverified availability and double bookings | SR‑010 | Guest, Business Owner, Partner Hotels |
| FR‑G‑038 | 1 | The system must provide currency selection from the list of supported currencies; after selection, all prices in the module (price per night, total price, price filter limits) are displayed in the selected currency. The default currency is the Ukrainian hryvnia (UAH). | Strategic requirement of the business owner for scaling; foreign guests see prices in a familiar currency | SR‑012 | Guest, Business Owner |
| FR‑G‑039 | 1 | The system must convert prices into the selected currency at the rate from the exchange rate service. If the currency differs from the hotel’s currency, the note “Price converted at the exchange rate as of DD.MM.YYYY” is displayed next to the total price | The guest understands that the price in their currency is calculated at the exchange rate and may differ slightly at payment | SR‑012 | Guest, Business Owner |
| FR‑G‑040 | 1 | The system must provide interface language selection; interface elements, filter and amenity names, statuses and marketing tags are translated into the selected language. The default language is Ukrainian. | Strategic requirement of the business owner for scaling; entry into new markets | SR‑012 | Guest, Business Owner |
| FR‑G‑041 | 3 | Date and number formats (separators, day and month order) must correspond to the selected language | Prevents confusion about check-in and check-out dates for guests from different countries | SR‑012 | Guest |
| **Security Requirements** |  |  |  |  |  |
| FR‑S‑001 | 1 | Only a user with the “Administrator” role can change the recommendation settings (weights of the recommendation score factors) and the list of partner hotels | Protection of business logic and confidential margin data from unauthorized changes | SR‑008 | Business Owner |
| FR‑S‑002 | 1 | Margin data and the terms of partnership agreements must not be transferred to the front end or displayed to the guest | Commercial terms are confidential information of the platform and its partners | SR‑008 | Business Owner, Partner Hotels |
| **Reporting Requirements** |  |  |  |  |  |
| FR‑R‑001 | 1 | The system must log searches with no results: destination, dates, number of adults and children, children’s ages, applied filters, currency, language, date and time of the query | Business owner requirement: data on unmet demand is needed to expand the hotel base in under-served segments | SR‑011 | Business Owner |
| FR‑R‑002 | 1 | The system must log cases where the number of results dropped to zero after a filter was applied, indicating the filter that caused it | The business owner wants to know which filters users select but find no results for | SR‑011 | Business Owner |
| FR‑R‑003 | 2 | The system must generate a report on searches with no results broken down by destination, period, price range and filters, with export to Excel / CSV | Enables decisions on onboarding new hotels based on the collected data | SR‑011 | Business Owner |
| FR‑R‑004 | 3 | The system must generate a search-to-booking conversion report broken down by: recommended / other hotels, hotels with / without marketing tags | Allows assessing the contribution of recommendations and tags to conversion growth from 3% to 5% | SR‑008<br>SR‑009 | Business Owner |
| **Usability Requirements** |  |  |  |  |  |
| FR‑U‑001 | 2 | On mobile devices, filters must open in a separate panel; selected filters are marked with checkboxes, and the number of selected filters is displayed on the button that opens the panel (e.g., “Filters (2)”) | Users often search for hotels “on the go”; on a small screen, filters do not cover the results, and the guest sees how many filters are active | SR‑003<br>SR‑004<br>SR‑005 | Guest |
| **Audit Requirements** |  |  |  |  |  |
| FR‑A‑001 | 1 | The system must record the date, time, user and content of changes to the recommendation settings and the list of partner hotels | Ensures control and traceability of changes to business logic | SR‑008 | Business Owner |
| FR‑A‑002 | 2 | The system must record each display of a marketing tag: hotel, tag, the availability and price data on which the tag was displayed, date and time | Allows confirming the accuracy of a tag in case of guest claims or inspections | SR‑009 | Business Owner |

### 4.2 Non-Functional Requirements

Non-functional requirements define the quality attributes of the solution (speed, data freshness, reliability, usability) and are also linked to stakeholder requirements.

| ID | Requirement | Implements SR |
|---|---|---|
| NFR‑001 | Time from clicking the “Search” button to displaying the first batch of results — no more than 2 seconds for 95% of queries under normal load | SR‑002 |
| NFR‑002 | Time to update search results based on the selected filters or sorting — no more than 1 second for 95% of queries | SR‑002<br>SR‑003<br>SR‑004<br>SR‑005 |
| NFR‑003 | Time for autocomplete suggestions to appear — no more than 300 ms after a character is entered | SR‑001 |
| NFR‑004 | Calculation of recommended hotels and marketing tags is performed within the time specified in NFR-001 and does not increase the time to display results | SR‑008<br>SR‑009<br>SR‑002 |
| NFR‑005 | The NFR-001 and NFR-002 targets are maintained under a load 3 times higher than the average normal load (peak load to be confirmed with the business owner) | SR‑002 |
| NFR‑006 | Changes in room availability and prices in the Channel Manager / hotel API are reflected in the search results no later than 2 seconds after the change | SR‑010 |
| NFR‑007 | Marketing tags are generated from availability and price data received no earlier than 2 seconds before display; if the data is older, the tag is not displayed | SR‑009<br>SR‑010 |
| NFR‑008 | Re-checking room availability when a hotel is selected takes no more than 1 second | SR‑010 |
| NFR‑009 | Exchange rates are updated at least once a day | SR‑012 |
| NFR‑010 | Search module availability — at least 99.5% per month | SR‑002<br>SR‑010 |
| NFR‑011 | A failure of an external service does not interrupt search: if the maps service is unavailable, the results list continues to work and the “Show on map” button is hidden; if the exchange rate service is unavailable, the last received rates are used | SR‑007<br>SR‑012<br>SR‑002 |
| NFR‑012 | New currencies and languages are added via settings and reference data without changing the program code | SR‑012 |
| NFR‑013 | The search and results interface is responsive and displays correctly on screens 320 px wide and above (smartphones, tablets, PCs); all search, filter and map functions are available on mobile devices | SR‑001<br>SR‑003<br>SR‑004<br>SR‑005<br>SR‑006<br>SR‑007 |
| NFR‑014 | The tap target size of controls on mobile devices is at least 44×44 px | SR‑001<br>SR‑003<br>SR‑004<br>SR‑005<br>SR‑007 |
| NFR‑015 | The module supports the two latest versions of Chrome, Safari, Firefox and Edge, as well as Safari on iOS and Chrome on Android | SR‑001<br>SR‑006<br>SR‑007 |
| NFR‑016 | Data exchange between the client, the search module and external systems is performed over the secure HTTPS protocol (TLS 1.2 or higher) | SR‑008<br>SR‑010 |
| NFR‑017 | Logs of searches with no results, the log of changes to recommendation settings and the log of marketing tag displays are retained for at least 12 months | SR‑011<br>SR‑008<br>SR‑009 |

## 5 Appendices

### 5.1 List of Acronyms

| Abbreviation | Definition |
|---|---|
| BRD | Business Requirements Document |
| API | Application Programming Interface — a software interface for exchanging data between systems |
| SR | Stakeholder Requirement |
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |

### 5.2 Glossary of Terms

| Term | Definition |
|---|---|
| Search-to-booking conversion | The share of search sessions that ended with a confirmed booking |
| Channel Manager | A system that synchronizes hotel room availability and prices across booking platforms |
| Recommended hotels | Hotels selected by the recommendation module based on margin and partnership agreements that match the guest’s search parameters and filters and have available rooms for the selected dates. They are marked with the “Recommended by TravelMate” label; when sorting by “Recommended”, they are displayed at the top of the search results |
| “Recommended by TravelMate” label | A green label on the card of a recommended hotel that is kept with any sorting of results |
| Recommendation score | An indicator calculated based on margin and the terms of the partnership agreement that determines the order of recommended hotels; the weights of the factors are set in the recommendation settings |
| Marketing tag | A label on the hotel card (“Only 1 room left”, “Best price”) generated based on up-to-date room availability and price data |
| Child | A guest who is under 18 years of age on the hotel check-out date |

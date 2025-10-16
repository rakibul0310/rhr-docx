# Analytics Data Schema Documentation

This document provides a comprehensive overview of the data schema used in the analytics system. It describes the structure and meaning of fields found in two main collections: `analytics_sessions` and `page_views`.

## Analytics Sessions Collection

The `analytics_sessions` collection stores information about user sessions on a website.

| Field           | Type     | Description                                                            |
| --------------- | -------- | ---------------------------------------------------------------------- |
| \_id            | ObjectId | Unique identifier for the session record                               |
| createdAt       | Date     | Timestamp when the session record was created in the database          |
| isArchived      | Boolean  | Flag indicating if the session has been archived                       |
| analyticsDomain | ObjectId | Reference to the domain this session belongs to                        |
| trackingId      | String   | Unique tracking identifier (e.g., "GA-4462CE06C96C8624")               |
| sessionId       | String   | Unique identifier for this particular session                          |
| clientId        | String   | Unique identifier for the client/visitor                               |
| domain          | String   | Domain name of the website (e.g., "www.rakibul.dev")                   |
| ipAddress       | String   | IP address of the visitor                                              |
| userAgent       | String   | Browser user agent string                                              |
| country         | String   | Country of the visitor based on IP geolocation                         |
| region          | String   | Region/state of the visitor based on IP geolocation                    |
| city            | String   | City of the visitor based on IP geolocation                            |
| sessionStart    | Date     | Timestamp when the session started                                     |
| sessionDuration | Number   | Duration of the session in seconds                                     |
| pageViews       | Number   | Number of pages viewed during this session                             |
| events          | Number   | Count of events triggered during the session                           |
| lastActivity    | Date     | Timestamp of the last activity in this session                         |
| isActive        | Boolean  | Whether the session is still active                                    |
| entryPage       | String   | URL of the first page visited in this session                          |
| exitPage        | String   | URL of the last page visited in this session                           |
| referrer        | String   | URL that referred the visitor to this site                             |
| utmSource       | String   | UTM source parameter from tracking URL (marketing source)              |
| utmMedium       | String   | UTM medium parameter (marketing medium)                                |
| utmCampaign     | String   | UTM campaign name                                                      |
| utmTerm         | String   | UTM term parameter (keywords)                                          |
| utmContent      | String   | UTM content parameter (specific content identifier)                    |
| sessionEnd      | Date     | Timestamp when the session ended (only present for completed sessions) |

## Page Views Collection

The `page_views` collection stores detailed information about each individual page view.

| Field             | Type     | Description                                                            |
| ----------------- | -------- | ---------------------------------------------------------------------- |
| \_id              | ObjectId | Unique identifier for the page view record                             |
| createdAt         | Date     | Timestamp when the record was created in the database                  |
| isArchived        | Boolean  | Flag indicating if the record has been archived                        |
| analyticsDomain   | ObjectId | Reference to the domain this page view belongs to                      |
| trackingId        | String   | Tracking identifier matching the session tracking ID                   |
| domain            | String   | Domain of the website (e.g., "www.rakibul.dev")                        |
| clientId          | String   | Unique identifier for the client/visitor                               |
| sessionId         | String   | ID of the session this page view belongs to                            |
| eventType         | String   | Type of event (e.g., "pageview", "heartbeat", "session_end")           |
| pageLocation      | String   | Full URL of the page viewed                                            |
| pageReferrer      | String   | URL that referred to this page (for pageview events)                   |
| pageTitle         | String   | Title of the page                                                      |
| userAgent         | String   | Browser user agent string                                              |
| ipAddress         | String   | IP address of the visitor                                              |
| country           | String   | Country based on IP geolocation                                        |
| region            | String   | Region/state based on IP geolocation                                   |
| city              | String   | City based on IP geolocation                                           |
| asnLookup         | String   | Autonomous System Number lookup result                                 |
| hostname          | String   | Hostname from the URL                                                  |
| pathname          | String   | Path portion of the URL                                                |
| search            | String   | Query string portion of the URL                                        |
| searchKeyword     | String   | Extracted search keywords (if any)                                     |
| deviceType        | String   | Type of device (e.g., "desktop", "mobile")                             |
| browser           | String   | Browser name (e.g., "Chrome")                                          |
| browserVersion    | String   | Browser version                                                        |
| os                | String   | Operating system                                                       |
| language          | String   | Visitor's browser language setting                                     |
| timezone          | String   | Visitor's timezone                                                     |
| screenResolution  | String   | Screen resolution (e.g., "1280x832")                                   |
| mouseClicks       | Number   | Number of mouse clicks recorded on the page                            |
| cookieSent        | Number   | Whether cookies were sent with the request                             |
| jsTokenSent       | Number   | Whether a JavaScript token was sent (bot detection)                    |
| pageLoadTime      | Number   | Time taken to load the page in milliseconds                            |
| requestsPerMinute | Number   | Rate of requests per minute from this client                           |
| uniquePages5Min   | Number   | Count of unique pages visited in the last 5 minutes                    |
| crawlerScore      | Number   | Score indicating likelihood of being a crawler/bot (-2 = likely human) |
| aiClassification  | String   | AI-based classification ("HUMAN", "BOT", etc.)                         |
| botClassification | String   | Bot detection classification (e.g., "LIKELY HUMAN")                    |
| honeypotHit       | Number   | Whether visitor triggered honeypot elements (bot detection)            |
| honeypotType      | String   | Type of honeypot triggered, if any                                     |
| sessionDuration   | Number   | Current duration of the session in seconds                             |
| pageCount         | Number   | Count of pages in the current session                                  |
| timestamp         | Date     | Timestamp when the event occurred                                      |
| collectedAt       | Date     | Timestamp when the event data was collected                            |

## Relationships Between Collections

- The `sessionId` in page_views corresponds to the `sessionId` in analytics_sessions, linking individual page views to their session.
- Both collections reference the same `analyticsDomain` which represents the website being tracked.
- Both use the same `trackingId` for associating data with a specific analytics installation.

## Special Event Types

- **pageview**: Records an initial page view
- **heartbeat**: Periodic updates to track ongoing session activity
- **session_end**: Marks the end of a session

## Bot Detection Metrics

The system employs multiple techniques for bot detection:

- `crawlerScore`: Numerical score indicating bot likelihood (negative values suggest human)
- `aiClassification`: AI-based determination of visitor type
- `botClassification`: Overall bot classification
- `honeypotHit`: Whether hidden elements designed to catch bots were triggered
- `requestsPerMinute`: Rate of requests (high rates may indicate automated traffic)
- `uniquePages5Min`: Number of unique pages accessed in a short time period

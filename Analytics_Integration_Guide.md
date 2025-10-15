# Everything Green API - Analytics Integration Guide

[![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)](https://nestjs.com/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)

A comprehensive analytics platform with domain registration, script integration, and real-time tracking capabilities.

## 🚀 Quick Start Guide: Domain Registration to Script Integration via Postman

This guide walks you through the complete process of registering a domain, getting the analytics script, and verifying integration using Postman.

### 📋 Prerequisites

- Postman installed
- Your API server running (default: `https://api-dev.everithinggreen.org`)
- A domain you want to track (e.g., `example.com`)

---

## 🔄 Complete Integration Workflow

### Step 1: Register Your Domain

**Endpoint:** `POST /analytics/collect/register-domain`

**Request:**

```json
{
  "domain": "example.com",
  "description": "My awesome website"
}
```

**Postman Setup:**

1. Create a new POST request
2. URL: `https://api-dev.everithinggreen.org/analytics/collect/register-domain`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
   ```json
   {
     "domain": "example.com",
     "description": "My website analytics"
   }
   ```
5. Send the request

**Expected Response:**

```json
{
  "success": true,
  "message": "Domain registered successfully",
  "data": {
    "domain": "example.com",
    "trackingId": "track_abc123def456",
    "description": "My website analytics",
    "createdAt": "2025-10-15T10:30:00Z",
    "isActive": false
  }
}
```

**📝 Save the `trackingId` - you'll need it for the next steps!**

---

### Step 2: Get Integration Script

**Endpoint:** `GET /analytics/script/{trackingId}/embed`

**Postman Setup:**

1. Create a new GET request
2. URL: `https://api-dev.everithinggreen.org/analytics/script/track_abc123def456/embed`
3. Replace `track_abc123def456` with your actual tracking ID
4. Send the request

**Expected Response:**

```json
{
  "domain": "example.com",
  "trackingId": "track_abc123def456",
  "scriptCode": "(function () { /* Complete analytics script */ })();",
  "scriptTag": "<script>\n/* Complete analytics code */\n</script>",
  "instructions": "Copy and paste this complete script...",
  "externalScriptTag": "<script src=\"https://api-dev.everithinggreen.org/analytics/script/track_abc123def456\" async defer></script>",
  "verificationUrl": "https://api-dev.everithinggreen.org/analytics/dashboard/track_abc123def456/verify",
  "testingInstructions": "After adding the script..."
}
```

---

### Step 3: Implement Script on Website

Copy the `scriptTag` from Step 2 response and add it to your website's HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Website</title>
    <!-- Analytics Script -->
    <script>
      (function () {
        'use strict';
        // Complete analytics script code goes here
        // This will be the full script from the response
      })();
    </script>
  </head>
  <body>
    <h1>Welcome to My Website</h1>
    <!-- Your content here -->
  </body>
</html>
```

**Alternative Method - External Script:**

```html
<head>
  <script
    src="https://api-dev.everithinggreen.org/analytics/script/track_abc123def456"
    async
    defer
  ></script>
</head>
```

---

### Step 4: Verify Integration

**Endpoint:** `GET /analytics/dashboard/{trackingId}/verify`

**Postman Setup:**

1. Create a new GET request
2. URL: `https://api-dev.everithinggreen.org/analytics/dashboard/track_abc123def456/verify`
3. Send the request

**Before Integration (Expected Response):**

```json
{
  "isActive": false,
  "lastActivity": null
}
```

**After Integration (Expected Response):**

```json
{
  "isActive": true,
  "lastActivity": "2025-10-15T10:45:30Z"
}
```

---

### Step 5: Detailed Integration Status

**Endpoint:** `GET /analytics/dashboard/{trackingId}/integration-status`

**Postman Setup:**

1. Create a new GET request
2. URL: `https://api-dev.everithinggreen.org/analytics/dashboard/track_abc123def456/integration-status`
3. Send the request

**Expected Response:**

```json
{
  "domain": "example.com",
  "trackingId": "track_abc123def456",
  "status": "active",
  "statusMessage": "Analytics script is working correctly",
  "isIntegrated": true,
  "isCurrentlyActive": true,
  "dataPoints": {
    "total": 156,
    "lastHour": 5,
    "last24Hours": 89,
    "lastWeek": 156
  },
  "hasPageViews": true,
  "hasCustomEvents": false,
  "firstDataReceived": "2025-10-15T10:45:30Z",
  "lastDataReceived": "2025-10-15T11:30:15Z",
  "scriptUrl": "https://api-dev.everithinggreen.org/analytics/script/track_abc123def456",
  "integrationInstructions": {
    "htmlScript": "<script>/* Complete script */</script>",
    "reactHook": "// React integration code"
  }
}
```

---

### Step 6: View Analytics Dashboard

**Endpoint:** `GET /analytics/dashboard/{trackingId}`

**Postman Setup:**

1. Create a new GET request
2. URL: `https://api-dev.everithinggreen.org/analytics/dashboard/track_abc123def456`
3. Optional query parameters:
   - `startDate`: `2025-10-01T00:00:00Z`
   - `endDate`: `2025-10-15T23:59:59Z`

**Expected Response:**

```json
{
  "stats": {
    "totalVisitors": 1234,
    "totalPageviews": 5678,
    "uniqueIPs": 987,
    "humanVisits": 1100,
    "aiBots": 134,
    "avgLoadTime": 2.3,
    "bounceRate": 0.35,
    "avgSessionDuration": 180
  },
  "topPages": [
    {
      "pageLocation": "/",
      "pageviews": 2345,
      "uniqueVisitors": 1890,
      "engagementRate": 0.65
    }
  ],
  "countryStats": [
    {
      "country": "United States",
      "visits": 567,
      "percentage": 45.2
    }
  ],
  "trafficSources": [
    {
      "source": "direct",
      "visits": 678,
      "percentage": 54.1
    }
  ],
  "dateRange": {
    "startDate": "2025-10-01T00:00:00Z",
    "endDate": "2025-10-15T23:59:59Z"
  }
}
```

---

## 🛠️ Additional API Endpoints

### Get All Registered Domains

```http
GET /analytics/dashboard/domains
```

### Get Real-time Data

```http
GET /analytics/dashboard/{trackingId}/realtime
```

### Get Time Series Data

```http
GET /analytics/dashboard/{trackingId}/timeseries?granularity=day&startDate=2025-10-01&endDate=2025-10-15
```

### Manual Data Collection (for testing)

```http
POST /analytics/collect
Content-Type: application/json

{
  "tracking_id": "track_abc123def456",
  "domain": "example.com",
  "client_id": "client_xyz789",
  "session_id": "ses_abc123_1697364000",
  "event_type": "pageview",
  "page_url": "https://example.com/",
  "page_title": "Home Page",
  "referrer": "",
  "screen_width": 1920,
  "screen_height": 1080,
  "device_type": "desktop",
  "browser": "Chrome",
  "os": "Windows",
  "language": "en-US",
  "timestamp": "2025-10-15T10:45:30Z"
}
```

---

## 🐛 Troubleshooting Guide

### Issue: Domain Registration Failed

**Symptoms:** Getting 400 error on domain registration
**Solutions:**

1. Check if domain already exists
2. Ensure domain format is correct (no http/https)
3. Verify Content-Type header is set

### Issue: Script Not Loading

**Symptoms:** `isActive: false` in verification
**Solutions:**

1. Ensure script is added to `<head>` section
2. Check browser console for JavaScript errors
3. Verify tracking ID is correct
4. Test with both complete script and external script methods

### Issue: No Data Appearing

**Symptoms:** Integration shows active but no dashboard data
**Solutions:**

1. Wait 2-3 minutes after first visit
2. Check browser network tab for POST requests to `/analytics/collect`
3. Ensure cookies and JavaScript are enabled
4. Verify domain matches exactly (with/without www)

---

## 📊 Understanding Analytics Data

### Event Types

- `pageview`: Page visits and navigation
- `event`: Custom events (button clicks, form submissions)
- `heartbeat`: User activity signals
- `session_end`: Session completion

### Device Detection

- `desktop`: Traditional computers
- `mobile`: Mobile phones
- `tablet`: Tablet devices

### Bot Detection

- AI bots are automatically detected and flagged
- Honeypot traps catch malicious crawlers
- Behavior analysis filters out non-human traffic

---

## 🔧 Environment Configuration

Make sure your `.env` file contains:

```env
# Analytics Configuration
ANALYTICS_API_BASE_URL=https://api-dev.everithinggreen.org
BASE_URL=https://api-dev.everithinggreen.org

# Database
MONGODB_URI=mongodb://localhost:27017/everything-green-api

# Server
PORT=3000
```

---

## 📈 Best Practices

1. **Script Placement**: Always put analytics script in `<head>` for best performance
2. **Domain Format**: Register domains without protocol (use `example.com`, not `https://example.com`)
3. **Testing**: Use verification endpoints before going live
4. **Monitoring**: Check integration status regularly
5. **Privacy**: Analytics respects user privacy with anonymous tracking

---

## 🤝 Support

If you encounter issues:

1. Check the troubleshooting section above
2. Verify your API server is running
3. Test endpoints with the provided Postman examples
4. Check server logs for error details

---

## Description

A NestJS-based analytics platform providing comprehensive website tracking with bot detection, real-time monitoring, and detailed reporting capabilities.

## Installation

```bash
$ yarn install
```

## Running the app

```bash
# development
$ yarn run start

# watch mode
$ yarn run start:dev

# production mode
$ yarn run start:prod
```

## Test

```bash
# unit tests
$ yarn run test

# e2e tests
$ yarn run test:e2e

# test coverage
$ yarn run test:cov
```

## Support

Nest is an MIT-licensed open source project. It can grow thanks to the sponsors and support by the amazing backers. If you'd like to join them, please [read more here](https://docs.nestjs.com/support).

## Stay in touch

- Author - [Kamil Myśliwiec](https://kamilmysliwiec.com)
- Website - [https://nestjs.com](https://nestjs.com/)
- Twitter - [@nestframework](https://twitter.com/nestframework)

## License

Nest is [MIT licensed](LICENSE).

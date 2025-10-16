# EGL Analytics API Documentation

This document provides comprehensive API documentation for the basic analytics functionality in the EGL (Everything Green Limited) API. It covers registering domains, retrieving tracking IDs, obtaining analytics scripts, and verifying script integration.

## Table of Contents

1. [Register Domain](#1-register-domain)
2. [Get Tracking ID by Website ID](#2-get-tracking-id-by-website-id)
3. [Get Analytics Script](#3-get-analytics-script)
4. [Verify Script Integration](#4-verify-script-integration)

## 1. Register Domain

Register a new domain to begin collecting analytics data.

### Endpoint

```
POST /egl-analytics/collect/register-domain
```

### Request Body

```json
{
  "websiteId": "5f8d0d55b54764429c2ca73a",
  "domain": "example.com",
  "description": "My website analytics"
}
```

#### Parameters

| Parameter   | Type   | Required | Description                          |
| ----------- | ------ | -------- | ------------------------------------ |
| websiteId   | string | Yes      | The ID of the website to register    |
| domain      | string | No       | The domain to register for analytics |
| description | string | No       | Optional description of the domain   |

### Response

**Status Code**: 201 Created

```json
{
  "domain": "example.com",
  "trackingId": "track_abc123def456",
  "description": "My website analytics",
  "isActive": true,
  "registeredAt": "2025-10-16T08:30:00.000Z"
}
```

### Error Responses

**Status Code**: 400 Bad Request

```json
{
  "statusCode": 400,
  "message": "Domain already registered",
  "error": "Bad Request"
}
```

## 2. Get Tracking ID by Website ID

Retrieve the analytics tracking ID associated with a specific website ID.

### Endpoint

```
GET /egl-analytics/collect/tracking-id/:websiteId
```

#### Path Parameters

| Parameter | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| websiteId | string | The website ID to get tracking ID for |

### Response

**Status Code**: 200 OK

```json
{
  "trackingId": "track_abc123def456",
  "domain": "example.com"
}
```

## 3. Get Analytics Script

Retrieve the analytics tracking script for integration into your website.

```
GET /egl-analytics/script/{trackingId}/embed
```

#### Path Parameters

| Parameter  | Type   | Description                    |
| ---------- | ------ | ------------------------------ |
| trackingId | string | The tracking ID for the domain |

### Response

**Status Code**: 200 OK
**Content-Type**: application/json

```json
{
  "domain": "example.com",
  "trackingId": "track_abc123def456",
  "scriptTag": "<script>\n(function() { /* Complete inline script */ })();\n</script>",
  "externalScriptTag": "<script src=\"http://api.everythinggreen.com/egl-analytics/script/track_abc123def456\" async defer data-analytics-id=\"track_abc123def456\"></script>",
  "verificationUrl": "http://api.everythinggreen.com/egl-analytics/dashboard/track_abc123def456/verify",
  "instructions": "Copy and paste this complete script into the <head> section of your website...",
  "testingInstructions": "After adding the script:\n1. Visit your website in a browser\n2. Check the verification URL\n3. Look for \"isActive\": true in the response..."
}
```

### Error Responses

**Status Code**: 404 Not Found

```json
{
  "statusCode": 404,
  "message": "Tracking ID not found",
  "error": "Not Found"
}
```

## 4. Verify Script Integration

Verify that the analytics script is properly integrated and collecting data.

### Endpoint

```
GET /egl-analytics/dashboard/{trackingId}/verify
```

#### Path Parameters

| Parameter  | Type   | Description                    |
| ---------- | ------ | ------------------------------ |
| trackingId | string | The tracking ID for the domain |

### Response

**Status Code**: 200 OK

```json
{
  "isActive": true,
  "lastActivity": "2025-10-16T15:28:45.000Z"
}
```

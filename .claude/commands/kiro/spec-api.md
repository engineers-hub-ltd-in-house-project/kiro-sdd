---
description: Design REST API specifications with OpenAPI format
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# API Specification Design

Design comprehensive REST API for feature: **$ARGUMENTS**

## Context Validation

### Previous Phases Check

- Use cases: @.kiro/specs/$ARGUMENTS/usecase.md
- Sequence diagrams: @.kiro/specs/$ARGUMENTS/sequence.md
- Database schema: @.kiro/specs/$ARGUMENTS/schema.md
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

**CRITICAL**: API design requires approved schema and sequence diagrams.

## Interactive Approval

Before generating API spec, ask the user:
```
Ready to generate API specification for $ARGUMENTS?
Database schema should be reviewed first.
Have you reviewed schema.md? [y/N]: 
```

If 'N': Stop and request review of schema first.
If 'y': Update spec.json to mark schema as approved and proceed:
```json
{
  "approvals": {
    "schema": {
      "generated": true,
      "approved": true
    }
  }
}
```

## Task: Generate API Specification

Generate api.md with complete OpenAPI 3.0 specification:

### 1. API Document Structure

```markdown
# API Specification: [Feature Name]

## Overview
RESTful API design based on sequence diagrams and database schema.

## API Design Principles
- RESTful: Follow REST conventions
- Consistent: Uniform response format
- Versioned: API versioning strategy
- Documented: OpenAPI 3.0 specification
- Secure: Authentication and authorization

## Base Configuration

### Base URL
```
Development: http://localhost:8080/api/v1
Staging: https://staging.example.com/api/v1
Environment variable: https://{env}.example.com/api/v1
```

### Authentication
```yaml
securitySchemes:
  bearerAuth:
    type: http
    scheme: bearer
    bearerFormat: JWT
```

### Common Headers
```yaml
headers:
  X-Request-ID:
    description: Unique request identifier for tracing
    schema:
      type: string
      format: uuid
  X-Client-Version:
    description: Client application version
    schema:
      type: string
```

## API Endpoints

### Analytics Overview
```yaml
/analytics/overview:
  get:
    summary: Get analytics overview
    operationId: getAnalyticsOverview
    tags:
      - Analytics
    security:
      - bearerAuth: []
    parameters:
      - name: start_date
        in: query
        required: false
        schema:
          type: string
          format: date-time
          example: "2025-01-01T00:00:00Z"
        description: Start date for analytics period (ISO 8601)
      - name: end_date
        in: query
        required: false
        schema:
          type: string
          format: date-time
          example: "2025-01-31T23:59:59Z"
        description: End date for analytics period (ISO 8601)
    responses:
      200:
        description: Analytics overview retrieved successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AnalyticsOverview'
            example:
              total_sent: 10000
              total_delivered: 9500
              open_rate: 25.5
              click_rate: 3.2
              bounce_rate: 5.0
              period:
                start: "2025-01-01T00:00:00Z"
                end: "2025-01-31T23:59:59Z"
      400:
        description: Invalid request parameters
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ErrorResponse'
            example:
              error:
                code: "INVALID_DATE_RANGE"
                message: "End date must be after start date"
      401:
        $ref: '#/components/responses/UnauthorizedError'
      500:
        $ref: '#/components/responses/InternalServerError'
```

### Campaign Metrics
```yaml
/analytics/campaigns:
  get:
    summary: Get campaign metrics list
    operationId: getCampaignMetrics
    tags:
      - Analytics
    security:
      - bearerAuth: []
    parameters:
      - name: page
        in: query
        schema:
          type: integer
          minimum: 1
          default: 1
      - name: per_page
        in: query
        schema:
          type: integer
          minimum: 1
          maximum: 100
          default: 20
      - name: sort_by
        in: query
        schema:
          type: string
          enum: [created_at, open_rate, click_rate, sent_count]
          default: created_at
      - name: sort_order
        in: query
        schema:
          type: string
          enum: [asc, desc]
          default: desc
      - name: start_date
        in: query
        schema:
          type: string
          format: date-time
      - name: end_date
        in: query
        schema:
          type: string
          format: date-time
      - name: status
        in: query
        schema:
          type: string
          enum: [draft, scheduled, sending, sent, failed]
    responses:
      200:
        description: Campaign metrics retrieved successfully
        content:
          application/json:
            schema:
              type: object
              properties:
                campaigns:
                  type: array
                  items:
                    $ref: '#/components/schemas/CampaignMetrics'
                pagination:
                  $ref: '#/components/schemas/Pagination'
            example:
              campaigns:
                - id: "550e8400-e29b-41d4-a716-446655440000"
                  name: "January Newsletter"
                  status: "sent"
                  sent_count: 5000
                  open_rate: 28.5
                  click_rate: 4.2
                  created_at: "2025-01-15T10:00:00Z"
              pagination:
                page: 1
                per_page: 20
                total: 45
                total_pages: 3
```

### Campaign Details
```yaml
/analytics/campaigns/{campaign_id}:
  get:
    summary: Get detailed campaign analytics
    operationId: getCampaignDetails
    tags:
      - Analytics
    security:
      - bearerAuth: []
    parameters:
      - name: campaign_id
        in: path
        required: true
        schema:
          type: string
          format: uuid
    responses:
      200:
        description: Campaign details retrieved successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CampaignDetails'
      404:
        description: Campaign not found
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ErrorResponse'
```

### Time Series Data
```yaml
/analytics/time-series:
  get:
    summary: Get time series analytics data
    operationId: getTimeSeries
    tags:
      - Analytics
    security:
      - bearerAuth: []
    parameters:
      - name: campaign_id
        in: query
        schema:
          type: string
          format: uuid
        description: Optional campaign filter
      - name: metric_type
        in: query
        required: true
        schema:
          type: string
          enum: [opens, clicks, bounces, unsubscribes]
      - name: aggregation
        in: query
        schema:
          type: string
          enum: [hour, day, week, month]
          default: day
      - name: start_date
        in: query
        required: true
        schema:
          type: string
          format: date-time
      - name: end_date
        in: query
        required: true
        schema:
          type: string
          format: date-time
    responses:
      200:
        description: Time series data retrieved successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TimeSeriesData'
```

### Export Analytics
```yaml
/analytics/export:
  post:
    summary: Export analytics data
    operationId: exportAnalytics
    tags:
      - Analytics
    security:
      - bearerAuth: []
    requestBody:
      required: true
      content:
        application/json:
          schema:
            type: object
            required:
              - export_type
              - format
            properties:
              export_type:
                type: string
                enum: [overview, campaigns, subscribers, events]
              format:
                type: string
                enum: [csv, xlsx, json]
              campaign_ids:
                type: array
                items:
                  type: string
                  format: uuid
              start_date:
                type: string
                format: date-time
              end_date:
                type: string
                format: date-time
              email_delivery:
                type: boolean
                default: false
                description: Send export via email for large files
    responses:
      200:
        description: Export completed immediately
        content:
          application/csv:
            schema:
              type: string
              format: binary
      202:
        description: Export queued for processing
        content:
          application/json:
            schema:
              type: object
              properties:
                export_id:
                  type: string
                  format: uuid
                status:
                  type: string
                  enum: [queued, processing]
                message:
                  type: string
```

## Component Schemas

### AnalyticsOverview
```yaml
AnalyticsOverview:
  type: object
  required:
    - total_sent
    - total_delivered
    - open_rate
    - click_rate
  properties:
    total_sent:
      type: integer
      minimum: 0
    total_delivered:
      type: integer
      minimum: 0
    unique_opens:
      type: integer
      minimum: 0
    unique_clicks:
      type: integer
      minimum: 0
    open_rate:
      type: number
      format: float
      minimum: 0
      maximum: 100
    click_rate:
      type: number
      format: float
      minimum: 0
      maximum: 100
    bounce_rate:
      type: number
      format: float
      minimum: 0
      maximum: 100
    unsubscribe_rate:
      type: number
      format: float
      minimum: 0
      maximum: 100
    period:
      type: object
      properties:
        start:
          type: string
          format: date-time
        end:
          type: string
          format: date-time
```

### ErrorResponse
```yaml
ErrorResponse:
  type: object
  required:
    - error
  properties:
    error:
      type: object
      required:
        - code
        - message
      properties:
        code:
          type: string
          description: Machine-readable error code
        message:
          type: string
          description: Human-readable error message
        details:
          type: array
          items:
            type: object
            properties:
              field:
                type: string
              message:
                type: string
        request_id:
          type: string
          format: uuid
```

### Pagination
```yaml
Pagination:
  type: object
  required:
    - page
    - per_page
    - total
    - total_pages
  properties:
    page:
      type: integer
      minimum: 1
    per_page:
      type: integer
      minimum: 1
      maximum: 100
    total:
      type: integer
      minimum: 0
    total_pages:
      type: integer
      minimum: 0
    has_next:
      type: boolean
    has_prev:
      type: boolean
```

## Response Examples

### Success Response Pattern
```json
{
  "data": {
    // Response payload
  },
  "meta": {
    "request_id": "550e8400-e29b-41d4-a716-446655440000",
    "timestamp": "2025-01-15T10:00:00Z"
  }
}
```

### Error Response Pattern
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "start_date",
        "message": "Invalid date format"
      }
    ],
    "request_id": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

## Error Codes

| Code | HTTP Status | Description |
|------|------------|-------------|
| UNAUTHORIZED | 401 | Missing or invalid authentication |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| VALIDATION_ERROR | 400 | Request validation failed |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Internal server error |
| SERVICE_UNAVAILABLE | 503 | Service temporarily unavailable |

## Rate Limiting

```yaml
headers:
  X-RateLimit-Limit:
    description: Request limit per hour
    schema:
      type: integer
  X-RateLimit-Remaining:
    description: Remaining requests in current window
    schema:
      type: integer
  X-RateLimit-Reset:
    description: Time when limit resets (Unix timestamp)
    schema:
      type: integer
```

## Versioning Strategy

- URL versioning: `/api/v1/`, `/api/v2/`
- Breaking changes require new version
- Deprecation notice: 6 months minimum
- Sunset period: 12 months after deprecation
```

### 2. API Design Best Practices

Ensure API follows:
- **RESTful conventions**: Proper HTTP methods and status codes
- **Consistent naming**: snake_case for JSON, kebab-case for URLs
- **Pagination**: Limit/offset or cursor-based
- **Filtering**: Query parameters for filtering
- **Sorting**: Consistent sort parameter format
- **Error handling**: Consistent error response format

### 3. Security Considerations

Include:
- **Authentication**: JWT/OAuth2 specification
- **Authorization**: Role-based access control
- **Rate limiting**: Prevent abuse
- **Input validation**: Parameter constraints
- **CORS policy**: Cross-origin configuration

### 4. Update Metadata

Update spec.json:
```json
{
  "phase": "api-defined",
  "endpoints_count": [number],
  "approvals": {
    "usecase": {
      "approved": true
    },
    "sequence": {
      "approved": true
    },
    "schema": {
      "approved": true
    },
    "api": {
      "generated": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Review sequence diagrams** - Understand all interactions
2. **Map to REST endpoints** - Convert sequences to APIs
3. **Define request/response** - Complete payload schemas
4. **Add all parameters** - Query, path, headers
5. **Document error cases** - All possible errors
6. **Include examples** - Real request/response examples
7. **Add rate limiting** - Protect against abuse
8. **Define security** - Authentication/authorization
9. **Ensure consistency** - Naming, format, structure
10. **Update metadata** - Record completion

Generate a complete API specification that **prevents integration issues** by defining exact contracts upfront.

## Output

Write `.kiro/specs/$ARGUMENTS/api.md` with complete OpenAPI specification.
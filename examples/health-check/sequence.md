# Sequence Diagrams: Health Check

## Overview

Detailed interaction flows for all use cases defined in usecase.md, showing the complete request-response cycle for health check operations including caching, database connectivity verification, and error handling scenarios.

## Diagram Legend
- Solid arrow (->): Synchronous call
- Dashed arrow (-->): Asynchronous response
- Note: Internal processing
- Alt/Else: Conditional flow
- Loop: Repeated operations
- Par: Parallel processing

## Primary Sequences

### Sequence 1: UC-1 Basic Health Status Check

#### Success Flow
```mermaid
sequenceDiagram
    autonumber
    participant MS as Monitoring System
    participant LB as Load Balancer
    participant API as Backend API
    participant Cache as In-Memory Cache
    participant DB as PostgreSQL
    participant Log as Logger
    
    MS->>LB: GET /api/health
    LB->>API: Forward request
    Note over API: No authentication required
    
    API->>Cache: Check for cached response
    alt Cache Hit (< 1 second old)
        Cache-->>API: Return cached response
        Note over API: Skip health checks
    else Cache Miss or Expired
        API->>API: Check application status
        Note over API: Verify internal components
        
        API->>DB: Execute simple query
        Note over DB: SELECT 1 or similar
        alt Database Connected
            DB-->>API: Query successful
            Note over API: Set database = "connected"
        else Database Timeout (5 seconds)
            Note over API: Set database = "disconnected"
            Note over API: Set status = "degraded"
        end
        
        API->>API: Construct response
        Note over API: Add timestamp, version
        
        API->>Cache: Store response
        Note over Cache: TTL = 1 second
        Cache-->>API: Cached
    end
    
    API->>Log: Log health check request
    Note over Log: Include response time
    
    API-->>LB: 200 OK {response}
    LB-->>MS: 200 OK {response}
    Note over MS: Process health status
```

#### Error Flow - Database Connection Failed
```mermaid
sequenceDiagram
    participant MS as Monitoring System
    participant API as Backend API
    participant DB as PostgreSQL
    participant Log as Logger
    
    MS->>API: GET /api/health
    API->>API: Check application status
    
    API->>DB: Execute simple query
    Note over DB: Connection attempt
    DB--xAPI: Connection failed/timeout
    Note over API: Wait up to 5 seconds
    
    API->>API: Set database = "disconnected"
    API->>API: Set status = "degraded"
    
    API->>Log: Log database failure
    Note over Log: Error level, include details
    
    API-->>MS: 200 OK {degraded status}
    Note over MS: System degraded but operational
```

#### Error Flow - Internal Server Error
```mermaid
sequenceDiagram
    participant MS as Monitoring System
    participant API as Backend API
    participant Log as Logger
    
    MS->>API: GET /api/health
    API->>API: Check application status
    Note over API: Unexpected error occurs
    
    API->>Log: Log critical error
    Note over Log: Include stack trace
    
    API-->>MS: 500 Internal Server Error
    Note over MS: Service unavailable
```

### Sequence 2: UC-2 Load Balancer Health Verification

#### Success Flow with Degraded Status
```mermaid
sequenceDiagram
    autonumber
    participant ALB as AWS ALB
    participant TG as Target Group
    participant API as Backend API
    participant Cache as In-Memory Cache
    participant DB as PostgreSQL
    
    loop Every 30 seconds
        ALB->>API: GET /api/health
        Note over ALB: Health check interval
        
        API->>Cache: Check for cached response
        Cache-->>API: Return response (if valid)
        
        alt Database Disconnected
            Note over API: status = "degraded"
            API-->>ALB: 200 OK {degraded}
            Note over ALB: Target remains healthy
            ALB->>TG: Update health status
            Note over TG: May adjust routing weight
        else All Checks Pass
            Note over API: status = "ok"
            API-->>ALB: 200 OK {ok}
            ALB->>TG: Mark target healthy
            Note over TG: Full traffic routing
        end
    end
```

#### Error Flow - Health Check Timeout
```mermaid
sequenceDiagram
    participant ALB as AWS ALB
    participant TG as Target Group
    participant API as Backend API
    participant ECS as ECS Service
    
    ALB->>API: GET /api/health
    Note over API: Processing delayed
    Note over ALB: Wait 5 seconds (timeout)
    
    ALB->>ALB: Mark request as timeout
    ALB->>TG: Mark target unhealthy
    Note over TG: Stop routing new traffic
    
    loop Retry health checks
        ALB->>API: GET /api/health
        alt Consecutive failures
            ALB->>TG: Target failed threshold
            TG->>ECS: Signal unhealthy task
            Note over ECS: May replace container
        else Recovery
            API-->>ALB: 200 OK
            ALB->>TG: Mark target healthy
            Note over TG: Resume traffic routing
        end
    end
```

### Sequence 3: UC-3 Manual Status Verification

#### Success Flow with Browser
```mermaid
sequenceDiagram
    autonumber
    participant DE as DevOps Engineer
    participant BR as Browser
    participant API as Backend API
    participant Cache as In-Memory Cache
    participant DB as PostgreSQL
    
    DE->>BR: Navigate to /api/health
    BR->>API: GET /api/health
    Note over BR: No auth headers sent
    
    API->>Cache: Check for cached response
    
    alt Cache Miss
        API->>DB: Check connectivity
        DB-->>API: Connection status
        API->>Cache: Store response
    else Cache Hit
        Cache-->>API: Cached response
    end
    
    API-->>BR: 200 OK {JSON response}
    BR->>BR: Format JSON for display
    BR-->>DE: Display formatted JSON
    
    Note over DE: Review status information
    alt Action Required
        DE->>DE: Initiate remediation
    else System Healthy
        DE->>DE: No action needed
    end
```

## Data Flow Details

### Request/Response Payloads

#### Request to /api/health
```
GET /api/health HTTP/1.1
Host: api.markmail.com
Accept: application/json
```
No request body or parameters required.

#### Success Response - All Healthy
```json
{
  "status": "ok",
  "timestamp": "2025-01-15T13:45:30.123Z",
  "database": "connected",
  "version": "1.2.3",
  "cached": false
}
```

#### Success Response - Degraded
```json
{
  "status": "degraded",
  "timestamp": "2025-01-15T13:45:30.123Z",
  "database": "disconnected",
  "version": "1.2.3",
  "cached": false
}
```

#### Success Response - From Cache
```json
{
  "status": "ok",
  "timestamp": "2025-01-15T13:45:29.500Z",
  "database": "connected",
  "version": "1.2.3",
  "cached": true
}
```

#### Error Response - Internal Error
```json
{
  "status": "error",
  "timestamp": "2025-01-15T13:45:30.123Z",
  "database": "unknown",
  "version": "1.2.3",
  "cached": false,
  "error": "Internal server error"
}
```

## Timing Constraints

### Performance Requirements
- API response time: < 100ms (p95)
- Database query: < 50ms (typical SELECT 1)
- Cache lookup: < 1ms
- Total end-to-end: < 100ms

### Timeout Settings
- Database connection timeout: 5 seconds
- Load balancer health check timeout: 5 seconds
- Cache TTL: 1 second (exactly)
- Rate limit window: 1 second (10 requests max)

## Concurrency Handling

### Concurrent Health Checks
```mermaid
sequenceDiagram
    participant LB as Load Balancer
    participant MS as Monitoring System
    participant API as Backend API
    participant Cache as In-Memory Cache
    participant RL as Rate Limiter
    
    par Parallel requests
        LB->>API: GET /api/health
    and
        MS->>API: GET /api/health
    and
        MS->>API: GET /api/health (2nd)
    end
    
    API->>RL: Check rate limit
    Note over RL: Per-IP tracking
    
    alt Within limit (< 10/sec)
        RL-->>API: Allowed
        API->>Cache: Get cached response
        Note over Cache: Single cache for all
        Cache-->>API: Same response for all
    else Rate limit exceeded
        RL-->>API: Rejected
        API-->>MS: 429 Too Many Requests
    end
```

## State Transitions

### Health Status State Machine
```mermaid
stateDiagram-v2
    [*] --> Starting: Application startup
    Starting --> Healthy: All checks pass
    Starting --> Degraded: App ok, DB failed
    Starting --> Error: Startup failed
    
    Healthy --> Degraded: Database disconnected
    Healthy --> Error: Critical failure
    
    Degraded --> Healthy: Database reconnected
    Degraded --> Error: Application failure
    
    Error --> Starting: Restart/Recovery
    Error --> [*]: Shutdown
```

### Cache State Transitions
```mermaid
stateDiagram-v2
    [*] --> Empty: Initial state
    Empty --> Valid: First request
    Valid --> Expired: After 1 second
    Expired --> Valid: New request
    Valid --> Valid: Cache hit
```

## Integration Points

### External Services
- **PostgreSQL Database**: Connection pool for health checks
- **In-Memory Cache**: Local cache, not Redis (for simplicity)
- **Load Balancer**: AWS ALB with configured health checks
- **Monitoring Systems**: CloudWatch, Datadog, etc.
- **Container Orchestration**: ECS, Kubernetes health probes

### Message Queue Events
None - Health check is read-only and doesn't publish events

### Dependencies
- Database connection pool must be initialized
- Application must be fully started
- Port 3000 must be bound and listening
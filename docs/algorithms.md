```md
# Rate Limiting Algorithm

This design uses a Fixed Window Counter.

It is simple and efficient.

## Rule

Allow 100 requests per 60 seconds per IP address

## Redis Key Format

rl:ip:{ip}:{windowStart}

Example

rl:ip:203.0.113.10:1710000000

Where windowStart represents the start time of the current 60-second window.

## Steps

1. Extract client IP
2. Create Redis key using IP and time window
3. Increment the counter using Redis INCR
4. If counter exceeds limit → block request
5. Otherwise allow request

## Redis Commands

INCR key  
EXPIRE key 60

Logic

count = INCR key

if count == 1
    EXPIRE key 60

if count > 100
    return 429
else
    allow request

## Limitation

This algorithm allows burst traffic at window boundaries.

Example:

100 requests at end of minute  
100 requests at start of next minute
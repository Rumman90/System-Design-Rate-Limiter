# System Design Rate Limiter

This repository explains a simple IP-based rate limiter system design.

The goal is to protect APIs from abuse by limiting the number of requests from a single IP address.

Example rule:
- 100 requests per minute per IP

If the limit is exceeded, the system returns:
HTTP 429 Too Many Requests

## High Level Architecture

```mermaid
graph LR
    Client --> API
    API --> RateLimiter
    RateLimiter --> Redis
    API --> BackendService
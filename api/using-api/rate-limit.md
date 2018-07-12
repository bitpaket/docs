---
title: Rate Limits | BitPaket Docs
description: Rate limits for API endpoints
service: api
main: 'using-api'
author: gencer
icon: pulse
seo: ''
priority: 0
---

# Rate Limits
{:tools}

We have flexible rate limits available for our clients.

{:toc}

As we are very generous about rates, we can work with custom rate limiting per paket or per client (all pakets) basis. Contact with us for custom rate limits and their pricing.

For standard rate limits, we send headers to clients. Headers consist from 3 pieces.

| Key                     | Description              |
| ----------------------- | ------------------------ |
| `X-RateLimit-Limit`     | Total limit until Reset |
| `X-RateLimit-Remaining` | Remaining request until Reset |
| `X-RateLimit-Reset` | When rate limits gets reset |

> **Note**: We may increase or decrease those limits with or without prior notice. For better throughput we suggest you to purchase custom rate limits if you expect huge amount of requests.

We send `429 - Too Many Requests` error if you reached these limits.
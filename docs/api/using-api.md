---
title: Using API | BitPaket Docs
description: Learn how to master on BitPaket
service: api
main: ''
author: gencer
icon: help
seo: ''
priority: 1000
---

# Overview
{:tools}

This documentation describes the resources that make up the official BitPaket REST API v2. If you have any problems or requests please contact BitPaket support.

{:toc}

## Current version

We already deprecated /v1 api endpoint and currently only support v2 api endpoint.

All Request URLs should emit /v2 in their path before any command. We also do not support header-based version. Which means only paths that has version will find its route.

## Schema

All API access can be either HTTP or HTTPS. We always recommend HTTPS due to encryption. In some cases, clients may want to use unencrypted HTTP instead of encrypted connection. **In these case we cannot guarantee your data safety.**

```shell
curl -i http://api.bitpaket.com/v2/metadata

HTTP/1.1 200 OK
Date: Tue, 10 Jul 2018 19:38:43 GMT
Content-Type: application/json
Content-Length: 1813
Connection: keep-alive
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
X-RateLimit-Reset: 1531251524
ETag: W/"24cbf038a1f381007064f47ba46bf1fa"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: c0U0oFK1T9womCJnwsxSDcJ2MuKQNnN1VFZIOzzNR3BNRqol5Tvz1LGekhqPofSi
Vary: Origin

<CONTENT>
```


## Authentication

As you may login your control panel with your Nienbo ID and Password, API Endpoints are protected with OAuth2 Authentication mechanism. Means that, you need an Authentication (Oauth2) Token for successfully authenticate on server-side.

You can find appropriate steps for creating API Endpoints and Tokens in Help Center. This part is only cover how to authenticate with already existing tokens. We assume you already created an endpoint and atl least one working token for this endpoint.

```shell
curl -H "Authorization: Bearer <OAUTH-TOKEN>" https://api.bitpaket.com
```

> **Note:** We do not support access_token in query parameters. All authorizations should be done via HTTP headers.

### Client Secret

Some endpoints such as `DELETE` requires `client_secret` to be given. In this case, you can pass `client_secret` either in query parameter or in JSON Body.

For example;

```json
{
    "client_secret": "your-client-secret",
    "objects": ...
}
```

*or if you prefer GET Query:*

```http
GET /delete?client_secret=<YOUR-CLIENT-SECRET> HTTP/1.1
Host: api.bitpaket.com
```

> **Note:** For securely transmit your `client_secret`, we always recommend you to send this parameter inside of request body.

### Failed authentications

When authentication fails, we send this error body as response:

#### Request

```http
DELETE /4e49527a-947c-48af-8ddb-a4c0c907e686 HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <INVALID_TOKEN>
```

#### Response

```
Status: 401 Unauthorized
```

```json
{
	"success": false,
	"error": {
		"code": 10401,
		"message": "The access token is invalid",
		"data": {
			"domain": null
		}
	}
}
```

**Attention!** There is an exception on error responses. When Upload an object, you may get different error objects like following:

```json
{
	"error": {
		"ns": "ERR_INVALID_AUTH_TOKEN_OR_PAKET_NOT_FOUND",
		"code": 401,
		"message": "Invalid Auth Token or Paket is not active"
	},
	"success": false
}
```

### How to handle errors

To properly handle errors, you need to check 2 things.

1. HTTP Status Code. We always return `200 OK` or `201 Created` status codes when successful.
2. JSON Object's `:status` attribute. 

`:status` attribute always returns `true` on success and `false` on failure.

`:error` object can give you some insights. But do not depent them on checking errors. This is absolutely an optional field.

> **Attention!** `:error` object can be different from one endpoint to another. Always rely on two options we mentioned above. And always store `:error` object for further reference.

# Rate Limiting

We have flexible rate limits available for our clients.

As we are very generous about rates, we can work with custom rate limiting per paket or per client (all pakets) basis. Contact with us for custom rate limits and their pricing.

For standard rate limits, we send headers to clients. Headers consist from 3 pieces.

| Key                     | Description              |
| ----------------------- | ------------------------ |
| `X-RateLimit-Limit`     | Total limit until Reset |
| `X-RateLimit-Remaining` | Remaining request until Reset |
| `X-RateLimit-Reset` | When rate limits gets reset |

> **Note**: We may increase or decrease those limits with or without prior notice. For better throughput we suggest you to purchase custom rate limits if you expect huge amount of requests.

We send `429 - Too Many Requests` error if you reached these limits.

# User agent is **not** required

All API Requests can either include or not include `UserAgent` field.

# Timezone

We always use `UTC` timezone on every requests and responses. We do not impose user's browser setting or IP location based timezone. Please keep this in mind. This will allow everyone to convert the source time to any desired timezone easily.

---
title: Creating ZipBalls | BitPaket Docs
description: You can directly combine multiple objects and even folders and add them to the container and generate unique ID, later you can download all of them in single link using this unique ID.
service: api
main: ''
author: gencer
icon: box
seo: ''
priority: 0
---

# Create and Download Zipped Objects (ZipBalls)
{:tools}

One of great advance tools of BitPaket is ZipBalls. You can directly combine multiple objects and even folders and add them to the container and generate unique ID, later you can download all of them in single link using this unique ID.

{:toc}

## Request ZipBall

```http
POST /v2/zipball HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"objects": [
		{
			"path": "/dir/myfile.exe"
		},
		{
			"path": "/dir/myfile2.exe"
		},
		{
            "folder": "/anotherDir"
		}
	]
}
```

`:objects` parameter can have multiple arrays. Each array should have **exactly one of** these parameters:

| Key      | Description                        |
| -------- | ---------------------------------- |
| `folder` | /dir. **Root '/' is not allowed.** |
| `id`     | Object ID (UUIDv4)                 |
| `path`   | /relative/object-path.exe          |

## Response

```
Status: 201 CREATED
```
```json
{
	"success": true,
	"data": {
		"ball_id": "e7d86f8c-f53e-4ec5-aee6-5e0408296dbe",
		"urls": {
			"url": "https://mypaket.bitpaket.com/e7d86f8c-f53e-4ec5-aee6-5e0408296dbe.zip?zipball",
			"domains": []
		}
	}
}
```

### Data

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `ball_id` | Unique ZipBall ID. |
| `urls`        | Object URIs. See below |

### URI

`:urls` parameter is an object. This object contains URIs of your object. 

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `url`      | Base URI for this object |
| `domains`        | `array` See below |

`:domains` parameter is an array. This array contains any custom domains associated to your paket and their URIs of your object. 

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `domain`      | Custom domain name without http:// |
| `url`        | URI for this object with above domain |

## Downloading ZipBalls

See [/stream-or-download#zipballs](/stream-or-download#zipballs)
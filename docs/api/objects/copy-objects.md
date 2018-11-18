---
title: Copying Objects and Folders | BitPaket Docs
description: Copy and merge (if necessary) folders and objects within your paket
service: api
main: objects
author: gencer
icon: duplicate
seo: ''
priority: 0
---

# Copying Objects
{:tools}

While you can move folders and objects across folders, you can also copy folders and objects (duplicate) too. This process initializes a long process of copying your objects in the target directory. That being said, long process, do not delete source objects until copy operation finishes. If you do so, operation may fail. 

{:toc}

## Copy an Object

```http
POST /v2/copy HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"client_secret": "<APP_SECRET>",
	"objects": [
		{
			"folder": "/existing_dir"
		}
	],
	"overwrite": true,
	"target": "/another_dir"
}
```

### Response

```
Status: 200 OK
```
```json
{
	"success": true,
	"data": {
		"objects": {
			"valid": {
				"object_ids": [],
				"object_names": []
			},
			"invalid": []
		}
	}
}
```
`:objects` parameter can have multiple arrays. Each array should have **exactly one of** these parameters:

| Key      | Description               |
| -------- | ------------------------- |
| `folder` | /dir                      |
| `id`     | Object ID (UUIDv4)        |
| `path`   | /relative/object-path.exe |

`:overwrite` can be set one of these values:

| Value   | Description                                                       |
| ------- | ----------------------------------------------------------------- |
| `true`  | Any same object on target will be destroyed. Others will be kept. |
| `false` | Operation will fail if any same named object found on target.     |

`:target` can be either base directory `/` or any existing directory in your paket. `NULL` is not accepted.

`:client_secret` is required to authorize this operation. You can grab this from your paket manager.

> **Attention!** Overwrited objects will be permanently destroyed and cannot be restored.

---
title: Renamig Objects | BitPaket Docs
description: Rename objects without hassle
service: api
main: objects
author: gencer
icon: chevron-right
seo: ''
priority: 0

# Renaming Objects
{:tools}

Renaming objects withing your pakets is easy as moving them. Only rule is that new file name or folder name should not exists already in the same current directory. For overwrite, use move operation.

{:toc}

## Rename an Object

```http
POST /v2/rename HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"client_secret": "<APP_SECRET>",
	"object":	{
			"folder": "/a/dir"
		},
	"filename": "new_name"
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
`:object` parameter can have only single object. This object should have **exactly one of** these parameters:

| Key        | Description |
| ------------- |--------------|
| `folder`      | /dir |
| `id`      | Object ID (UUIDv4)      |
| `path` | /relative/object-path.exe      |

`:filename` is the new filename (for folder or object. Doesn't matter)

`:client_secret` is required to authorize this operation. You can grab this from your paket manager.

> **Attention!** You cannot rename an object if the new name already exists in the same directory.

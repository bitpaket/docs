# Deleting Objects
{:tools}

Deleting objects, folders are easy. You have two options. Either delete multiple objects and folders in single request using batch or single DELETE request to delete single object.

> Deleting folders with `DELETE` request is not supported.

{:toc}

## Delete Multiple Objects

```http
POST /v2/delete HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"client_secret": "<APP_SECRET>",
	"objects": [
		{
			"path": "/352-400.exe"
		},
		{
			"path": "/dir/file.ai"
		}
	]
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
				"object_names": [
					{
						"object_name": "352-400.ai",
						"object_path": "/"
					},
					{
						"object_name": "file.ai",
						"object_path": "/dir"
					}
				]
			},
			"invalid": []
		}
	}
}
```

`:objects` parameter can have multiple arrays. Each array should have **exactly one of** these parameters:

| Key        | Description |
| ------------- |--------------|
| `folder`      | /dir. **Root '/' is not allowed.** |
| `id`      | Object ID (UUIDv4)      |
| `path` | /relative/object-path.exe      |

## Delete Single Object

If you want to delete specific version (tied to UUIDv4) or single object in preference, `DELETE` request should be made. Only one ID is allowed at once.

```http
DELETE /v2/delete/534ad135-6f5b-4661-9c40-bc0bd1b8fa5e HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
  "client_secret": "<APP_SECRET>"
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
				"object_ids": [
					"534ad135-6f5b-4661-9c40-bc0bd1b8fa5e"
				],
			},
			"invalid": []
		}
	}
}
```

`:client_secret` is required to authorize this operation. You can grab this from your paket manager.

> **Attention!** Deleted objects cannot be recovered. BitPaket does not cache or keep limited time for deleted files.

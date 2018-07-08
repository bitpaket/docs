# Moving Objects

Moving objects withing your pakets is easy. However, If you decide to move files and its siblings with overwrite mode enabled, All of your target (existing) objects will be completely replaced and all existing objects with all their versions will be destroyed.


## Move an Object

```http
POST /move HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Aurhorization: Bearer <YOUR_TOKEN>

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

`:objects` parameter can have multiple arrays. Each array should have at least one of these parameters:

| Key        | Sample Value           |
| ------------- |--------------|
| `folder`      | /dir |
| `id`      | Object ID (UUIDv4)      |
| `path` | /relative/object-path.exe      |

`:overwrite` can be set one of these values:

| Value        | Definition           |
| ------------- |--------------|
| `true`      | Any same object on target will be destroyed. Others will be kept. |
| `false`      | Operation will fail if any same named object found on target.     |

`:target` can be either base directory `/` or any existing directory in your paket. `NULL` is not accepted.

`:client_secret` is required to authorize this operation. You can grab this from your paket manager.
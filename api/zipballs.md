# ZipBalls
{:tools}

One of great advance tools of BitPaket is ZipBalls. You can directly combine multiple objects and even folders and add them to the container and generate unique ID, later you can download all of them in single link using this unique ID.

{:toc}

## Request ZipBall

```http
POST /v2/zipball HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Aurhorization: Bearer <YOUR_TOKEN>

{
	"objects": [
		{
			"path": "/dir/myfile.exe"
		},
		{
			"path": "/dir/myfile.exe"
		},
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

## Querying paket with parameters

Querying with default parameters is always recommended. You may want to extend this. In this case this is how you can fetch more detailed object information.

> **Note**: `X-Bit-Detailed` and `X-Bit-All` parameters can be a little slow compared to default querying method. Keep this in mind.

```http
GET /v2/query?PARAMETER=VALUE&PARAMETER2=VALUE2 HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Aurhorization: Bearer <YOUR_TOKEN>
```

### Response

```
Status: 200 OK
```
```
For response content, see above.
```

### Parameters

| Parameter      | Description                                                        |
| -------------- | ------------------------------------------------------------ |
| `X-Bit-Search` | Search query. Write any words to this parameter. Any matched objects will be listed. This is non-recursively. |
| `X-Bit-Folder` | Set folder. Use full paths that starts with slash. (E.g.: /dir/another) |
| `X-Bit-Tags` | Set Tags. You can use multiple tags separated with comma. (E.g.: tag1,tag2,tag3). Any matched object will be fetched. |
| `X-Bit-Detailed` | Should be `1` or `0`. This indicates that response should be detailed and not shortened. Default is `0` |
| `X-Bit-All` | Should be `1` or `0`. This indicates that all objects and if exists any of its versions should be also fetched. Default is `0` |

## Downloading ZipBalls

See [/stream-or-download#zipballs](/stream-or-download#zipballs)
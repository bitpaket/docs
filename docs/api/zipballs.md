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

# Create and Download Zipped Balls (ZipBalls)
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

| Key       | Description            |
| --------- | ---------------------- |
| `ball_id` | Unique ZipBall ID.     |
| `urls`    | Object URIs. See below |

### URI

`:urls` parameter is an object. This object contains URIs of your object. 

| Key       | Description              |
| --------- | ------------------------ |
| `url`     | Base URI for this object |
| `domains` | `array` See below        |

`:domains` parameter is an array. This array contains any custom domains associated to your paket and their URIs of your object. 

| Key      | Description                           |
| -------- | ------------------------------------- |
| `domain` | Custom domain name without http://    |
| `url`    | URI for this object with above domain |

## Adding specific or multiple versions

Sometimes, you need to include same object's multiple versions to the same ZIP file. In this case you need to pass :version or :revision along with :path.

### Request

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
			"path": "/dir/myfile.exe",
			"version": 5
		},
		{
			"path": "/dir/myfile2.exe",
			"revision": "0e4b563b"
		}
	]
}
```

### Response

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

### Reference table:

| Key        | Description                     |
| ---------- | ------------------------------- |
| `version`  | `Integer` value.                |
| `revision` | Hexademical value. Revision ID. |

## How we handle multiple versions?

In case multiple versions requested, we maintain and keep the folder and path structures but rename older versions. For example above request will result ZIP content like this;

```
/myfile.exe
/myfile (Version 5).exe
/myfile2.exe
```

One of `myfile.exe` renamed (appended) to `Version 5`. We also do not touch to the extension, so opening with an associated program will always work.

## Overriding object paths

In case of paths being overrided by giving another path, this is possible via `:override` attribute.

### Request

```http
POST /v2/zipball HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"objects": [
		{
			"id": "b6233520-0ab2-465e-b698-c69c7ed53e2a",
			"override": "/my/custom/path/myfile.zip"
		}
	]
}
```

### Response

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

> **Attention!** `:override` attribute only works along with `:id` parameter. **`:path` and `:folder` does not support this at the moment.**

## Downloading ZipBalls

See [/stream-or-download#zipballs](/stream-or-download#zipballs)

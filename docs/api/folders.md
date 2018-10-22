---
title: Creating Folder | BitPaket Docs
description: Create Folder and upload objects. Nested folders can help on organizing your objects.
service: api
main: ''
author: gencer
icon: folder
seo: ''
priority: 0
---

# Create Folder in your Paket
{:tools}

To create folder within your paket is easy.

{:toc}

## Create

```http
POST /v2/create HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"folder": "/my_folder"
}
```

> **Attention!** `:folder` parameter can have nested folder such as `/folder/in/another/folder`. However, before nested folders created, base (one before) folder created first. We do not recursively create folders.

| Key      | Description                        |
| -------- | ---------------------------------- |
| `folder` | /dir. **Root '/' is not allowed.** |

## Response

```
Status: 201 CREATED
```
```json
{
	"success": true,
	"data": {
		"folder_id": "9WFzCrQt10Yb6gO9KaO3",
		"folder_name": "my_folder",
		"size": 0,
		"size_pretty": "0 B",
		"created_on": "2018-07-13T20:57:43Z",
		"modified_on": "2018-07-13T20:57:43Z",
		"created_on_ts": 1531515463,
		"modified_on_ts": 1531515463
	}
}
```

| Key              | Description                                           |
| ---------------- | ----------------------------------------------------- |
| `folder_id`      | Unique Folder ID represented in BitPaket              |
| `folder_name`    | Created folder name                                   |
| `size`           | Size in `bytes`                                       |
| `created_on`     | Folder creation date in UTC                           |
| `modified_on`    | Folder last modification date in UTC                  |
| `created_on_ts`  | Folder creation date in Unix Timestamp (UTC)          |
| `modified_on_ts` | Folder last modification date in Unix Timestamp (UTC) |

## Error

```
Status: 400 BAD REQUEST
```
```json
{
	"success": false,
	"error": {
		"code": 10209,
		"message": "Folder already exists.",
		"data": {}
	}
}
```

### Create folder recursively (Ensure Exists)

We have noticed that create endpoint should have the base folder exists. In this example we will create all folders **recursively**, nonethless base exists or not. This endpoint will return true on success along wişth last folder and false on failure.

> **Attention!** If all folders already exists or created, last folder object will return as :data.


```http
POST /v2/create HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"folder": "/my_folder/in/another/last_folder",
	"ensure": true
}
```

> **Attention!** `:ensure` parameter is optional and represents that all structure should be created. If already exists, nothing will happen but last folder object will return as success.

| Key      | Description                                          |
| -------- | ---------------------------------------------------- |
| `folder` | /dir/with/sub. **Root '/' is not allowed.**          |
| `ensure` | **Boolean**. true for recursively ensure dir exists. |  |


#### Response

```
Status: 201 CREATED
```
```json
{
	"success": true,
	"data": {
		"folder_id": "zJh24UZKpVHNTHoD0ip5",
		"folder_name": "last_folder",
		"size": 0,
		"size_pretty": "0 B",
		"created_on": "2018-10-22T11:31:10Z",
		"modified_on": "2018-10-22T11:31:10Z",
		"created_on_ts": 1540207870,
		"modified_on_ts": 1540207870
	}
}
```

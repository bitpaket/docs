---
title: Metadata | BitPaket Docs
description: Use Metadata to get properties of your object
service: api
main: ''
author: gencer
icon: list
seo: ''
priority: 0
---

# Metadata
{:tools}

Metadata gives you all needed information about your Paket.

{:toc}

## Get metadata by Object ID

```http
GET /v2/metadata/{object-id} HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
```

### Response

```
Status: 200 OK
```
```json
{
	"success": true,
	"data": {
		"object_id": "c644b732-f1ce-4d50-81f9-96884dbcd84d",
		"properties": {
			"name": "myfile.zip",
			"size": 19256,
			"size_pretty": "18.8 KB",
			"hash": "5129795d6e0aeca2fa56aaa56d71d2e9809c2ad77c14265abcb51fe832105e00",
			"hash_type": "SHA256",
			"description": null,
			"mime_type": "application/x-dosexec",
			"is_sealed": false,
			"is_secure": false,
			"is_streamable": false,
			"is_block": false,
			"is_versioned": true,
			"version": 1,
			"revision": "264a7cee61",
			"created_on": "2018-07-08T11:19:11Z",
			"updated_on": "2018-07-08T11:19:11Z",
			"created_on_ts": 1531048751,
			"updated_on_ts": 1531048751
		},
		"deploy": {
			"type": "DREPLICATED_TREDUNDANCY_STORAGE",
			"chunked": false,
			"location": "AMS-01",
			"responsible": "self"
		},
		"tags": null,
		"urls": {
			"url": "https://deneme.bitpaket.com/dir/myfile.zip",
			"domains": [
				{
					"domain": "www.mydomain.com",
					"url": "http://www.mydomain.com/dir/myfile.zip"
				}
			]
		},
		"media": null,
		"blocks": []
	}
}
```

### Objects

`:data` parameter is an object. This object contains your metadata. 

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `object_id`      | Unique Object ID |
| `properties`        | Object properties. See below |
| `deploy`        | Deploy information. See below |
| `tags`        | Object tags. See below |
| `urls`        | Object URIs. See below |
| `media`        | Media info. This is same as `urls`. Only enabled in stream-able objects |
| `blocks`        | TBD (Blocks is in internal test stage.) |

### Properties

`:properties` parameter is an object. This array contains properties of your object.

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `name`      | Unique Object ID |
| `size`        | Object size in bytes |
| `size_pretty`        | Object size in prettified format |
| `is_secure`        | Indicates if this object requires a key to be accessible |
| `hash`        | Object hash |
| `hash_type`        | Object hash type. Currently we do support only SHA256. |
| `description`        | Object description |
| `mime_type`        | Object MIME type |
| `is_sealed`        | Determines if object sealed. Sealed objects cannot be overwritten. |
| `is_secure`        | Determines if object sealed. Sealed objects cannot be overwrited. |
| `is_streamable`        | Determines if object stream-able. Only audio, video and images are stream-able. |
| `is_versioned`        | Determines if object has versioned scheme. |
| `is_block`        | Indicates if this is a block object |
| `version`        | Current object version |
| `revision`        | Current object revision |
| `created_on`        | Creation date |
| `updated_on`        | Modification date |
| `created_on_ts`        | Creation date in unix timestamp |
| `updated_on_ts`        | Modification date in unix timestamp |

### Tags

`:tags` parameter is an array without any key. This array contains tags of your object. 

### URLs and Media

`:urls` parameter is an object. This object contains URIs of your object. 

`:media` parameter is an object. This object contains same content with `:urls`. Only enabled if object is stream-able. Otherwise, `null`

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `url`      | Base URI for this object |
| `domains`        | `array` See below |

`:domains` parameter is an array. This array contains any custom domains associated to your paket and their URIs of your object. 

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `domain`      | Custom domain name without http:// |
| `url`        | URI for this object with above domain |

### Deploy

`:deploy` parameter is an object. This object contains information about deployment of your object. This is shown to the clients to help for support related requests.

| Key        | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `type`     | Deployment mode. There are 2 different deployment mode. DREPLICATED_TREDUNDANCY_STORAGE and DREPLICATED_INFREQUENTACC_STORAGE |
| `location` | Object's stored location. First part is datacenter and second part is datacenter id. (E.g. AMS-01 for Datacenter 1 located in Amsterdam.) |

## Get Object IDs using Object Path

You can get Object ID by using object's ful path from your paket and if available together with all of it's versions.



```http
POST /v2/metadata HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>

{
	"path": "/myfile.zip"
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
		"objects": [
			{
				"object_id": "45b68a9e-7c65-4e77-a866-9e451d7e9453",
				"properties": {
					"name": "myfile.zip",
					"size": 234,
					"size_pretty": "234 B",
					"hash": "f12fdbefac4d8144690f4f9a92b58437391f213b82a749b9bb196781e6038374",
					"hash_type": "SHA256",
					"description": null,
					"mime_type": "image/png",
					"is_sealed": false,
					"is_secure": false,
					"is_streamable": false,
					"is_block": false,
					"is_versioned": true,
					"version": 2,
					"revision": "cc48a5df18",
					"created_on": "2018-06-10T19:11:07Z",
					"updated_on": "2018-06-10T19:11:07Z",
					"created_on_ts": 1528657867,
					"updated_on_ts": 1528657867
				},
				"deploy": {
					"type": "DREPLICATED_TREDUNDANCY_STORAGE",
					"chunked": false,
					"location": "AMS-01",
					"responsible": "self"
				},
				"tags": null,
				"urls": {
					"url": "https://mypaket.bitpaket.com/myfile.zip",
					"domains": [
						{
							"domain": "www.maydomain.com",
							"url": "http://www.maydomain.com/myfile.zip"
						}
					]
				},
				"media": null,
				"blocks": []
			},
			{
				"object_id": "7909eb31-0b60-4c81-9871-9e90309591c9",
				"properties": {
					"name": "myfile.zip",
					"size": 234,
					"size_pretty": "234 B",
					"hash": "f12fdbefac4d8144690f4f9a92b58437391f213b82a749b9bb196781e6038374",
					"hash_type": "SHA256",
					"description": null,
					"mime_type": "image/png",
					"is_sealed": false,
					"is_secure": false,
					"is_streamable": false,
					"is_block": false,
					"is_versioned": true,
					"version": 1,
					"revision": "7ce8374596",
					"created_on": "2018-06-10T19:08:36Z",
					"updated_on": "2018-06-10T19:08:36Z",
					"created_on_ts": 1528657716,
					"updated_on_ts": 1528657716
				},
				"deploy": {
					"type": "DREPLICATED_TREDUNDANCY_STORAGE",
					"chunked": false,
					"location": "AMS-01",
					"responsible": "self"
				},
				"tags": null,
				"urls": {
					"url": "https://mypaket.bitpaket.com/myfile.zip",
					"domains": [
						{
							"domain": "www.maydomain.com",
							"url": "http://www.maydomain.com/myfile.zip"
						}
					]
				},
				"media": null,
				"blocks": []
			}
		]
	}
}
```

`:objects` parameter is an array. This array contains information about your object. Each array contains metadata object. See [above](/getting-metadata) for metadata properties.
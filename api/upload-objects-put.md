---
title: Upload Objects (PUT Object) | BitPaket Docs
description: Upload objects to BitPaket in classical way using PUT requests.
service: api
main: ''
author: gencer
icon: cloud-upload
seo: ''
priority: 0
---

# Upload Objects (PUT Object)
{:tools}

At BitPaket, you have 2 options top upload object to our service. One of them is classic PUT Request. Other option is using [tus](https://tus.io) protocol. We will inspect classic way on this article.

{:toc}

## PUT Request

```http
PUT /v2/object/{object-name} HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Expect: 100-Continue

<BINARY_DATA>
```

### Headers

| Key                  | Description                                                                                                                                                                                            |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `X-Bit-Tag`          | `String` attribute separated by comma. Indicates if object has tags. Helpful on search. Separate each tag with comma. Default is `null`. (E.g. `tag1,tag2,tag3`)                                       |
| `X-Bit-Overwrite`    | `Integer` attribute. Indicates if object should be overwrited if exists. Set to `1` or `0`. Default is `0`                                                                                             |
| `X-Bit-Secure`       | `Integer` attribute. Indicates if object requires **pre signed URL** upon access. Set to `1` or `0`. Default is `0`                                                                                    |
| `X-Bit-Seal`         | `Integer` attribute. Indicates if object should be sealed and **cannot be** overwrited later even if new object has overwrite attribute se to `1` if exists. Set to `1` or `0`. Default is `0`         |
| `X-Bit-Versioning`   | `Integer` attribute. Indicates if object is based on versioning. On new upload, old one will not be deleted and assigned with an unique revision and version number. Set to `1` or `0`. Default is `0` |
| `X-Bit-Content-Type` | `String` attribute. Overrides server-side detected object MIME type. Be careful of using this. It may lead broken download for your clients.                                                           |
| `X-Bit-Folder`       | `String` attribute. Folder path to put this object on. Default is  `/`. This path should exists before uploading an object.                                                                            |
| `Expect`             | We use `100-Continue` to fail before whole data being uploaded. Helpful for large data being uploaded in case of failures.                                                                             |

### Response

```
Status: 201 CREATED
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

#### Response Headers

```
Status: 201 CREATED
```
```http
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Access-Control-Expose-Headers: X-SHA256,X-Session-Id,Content-Length,X-RateLimit-Limit,X-RateLimit-Remaining,X-RateLimit-Reset
X-Session-Id: 8558da8b708cbf48d660cc5041ad15922eeb6482
X-SHA256: 51b93e0bc7e6d697ccc29703e2ebc9210c231c931fe764c372e5ba0d26098d3b
Content-Length: 1092
```

| Key                             | Description                                                                                                                                                       |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `X-SHA256`                      | Object's last hash based on SHA256. You can verify this by sending your hash to the server. Please read below for further reference on integrity of your objects. |
| `X-Session-Id`                  | Upload ID. Used for chunked upload. See below of this article for chunked uploads.                                                                                |
| `Access-Control-Expose-Headers` | Accesible headers for CORS.                                                                                                                                       |
| `Content-Length`                | Current chunk's length (size)                                                                                                                                     |

See [RateLimit](/using-api/rate-limit) for Rate Limiting.

#### Objects

`:data` parameter is an object. This object contains your metadata. 

| Key          | Description                                                             |
| ------------ | ----------------------------------------------------------------------- |
| `object_id`  | Unique Object ID                                                        |
| `properties` | Object properties. See below                                            |
| `deploy`     | Deploy information. See below                                           |
| `tags`       | Object tags. See below                                                  |
| `urls`       | Object URIs. See below                                                  |
| `media`      | Media info. This is same as `urls`. Only enabled in stream-able objects |
| `blocks`     | TBD (Blocks is in internal test stage.)                                 |

#### Properties

`:properties` parameter is an object. This array contains properties of your object.

| Key             | Description                                                                     |
| --------------- | ------------------------------------------------------------------------------- |
| `name`          | Unique Object ID                                                                |
| `size`          | Object size in bytes                                                            |
| `size_pretty`   | Object size in prettified format                                                |
| `is_secure`     | Indicates if this object requires a key to be accessible                        |
| `hash`          | Object hash                                                                     |
| `hash_type`     | Object hash type. Currently we do support only SHA256.                          |
| `description`   | Object description                                                              |
| `mime_type`     | Object MIME type                                                                |
| `is_sealed`     | Determines if object sealed. Sealed objects cannot be overwritten.              |
| `is_secure`     | Determines if object sealed. Sealed objects cannot be overwrited.               |
| `is_streamable` | Determines if object stream-able. Only audio, video and images are stream-able. |
| `is_versioned`  | Determines if object has versioned scheme.                                      |
| `is_block`      | Indicates if this is a block object                                             |
| `version`       | Current object version                                                          |
| `revision`      | Current object revision                                                         |
| `created_on`    | Creation date                                                                   |
| `updated_on`    | Modification date                                                               |
| `created_on_ts` | Creation date in unix timestamp                                                 |
| `updated_on_ts` | Modification date in unix timestamp                                             |

#### Tags

`:tags` parameter is an array without any key. This array contains tags of your object. 

#### URLs and Media

`:urls` parameter is an object. This object contains URIs of your object. 

`:media` parameter is an object. This object contains same content with `:urls`. Only enabled if object is stream-able. Otherwise, `null`

| Key       | Description              |
| --------- | ------------------------ |
| `url`     | Base URI for this object |
| `domains` | `array` See below        |

`:domains` parameter is an array. This array contains any custom domains associated to your paket and their URIs of your object. 

| Key      | Description                           |
| -------- | ------------------------------------- |
| `domain` | Custom domain name without http://    |
| `url`    | URI for this object with above domain |

#### Deploy

`:deploy` parameter is an object. This object contains information about deployment of your object. This is shown to the clients to help for support related requests.

| Key        | Description                                                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `type`     | Deployment mode. There are 2 different deployment mode. DREPLICATED_TREDUNDANCY_STORAGE and DREPLICATED_INFREQUENTACC_STORAGE             |
| `location` | Object's stored location. First part is datacenter and second part is datacenter id. (E.g. AMS-01 for Datacenter 1 located in Amsterdam.) |


### Error

```
Status: 400 BAD REQUEST
```
```json
{
	"success": false,
	"error": {
		"code": 10209,
		"message": "Object already exists",
		"data": {}
	}
}
```

You cannot use **OVERWRITE** and **VERSIONING** headers at the same time. In this case;

```
Status: 400 BAD REQUEST
```
```json
{
	"success": false,
	"error": {
		"code": 10210,
		"message": "You cannot use ~OVERWRITE and ~VERSIONING at the same time. Use only one.",
		"data": {}
	}
}
```

You cannot use **OVERWRITE** and **VERSIONING** headers at the same time on **SELAED** objects. In this case;

```
Status: 400 BAD REQUEST
```
```json
{
	"success": false,
	"error": {
		"code": 10211,
		"message": "You have sealed this object before. You cannot ~OVERWRITE or ~VERSIONED on any sealed object without deleting them first.",
		"data": {}
	}
}
```

You cannot use **VERSIONING** header on already existsing and non-versioned objects. You need to delete them first. In this case;

```
Status: 400 BAD REQUEST
```
```json
{
	"success": false,
	"error": {
		"code": 10215,
		"message": "You cannot create ~VERSIONED scheme over an existing ~NONVERSIONED object. Remove this object first and start a new versioned object.",
		"data": {}
	}
}
```

## Integrity Check on Objects

When every PUT requests success on BitPaket Service, We return `X-SHA256` header with SHA256 hash for last uploaded chunk.

On every chunk you need to send this  `X-SHA256` back with `X-Verify-SHA256` header.

### Request

```http
PUT /v2/object/{object-name} HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Expect: 100-Continue
X-Verify-SHA256: Last returned X-SHA256 in hex. (SHA-256 of data uploaded so far)

<BINARY_DATA>
```

> **Attention!** You need to upload all chunks in order.

### Error

```
Status: 400 BAD REQUEST
```
```json
{
	"error": {
		"ns": "ERR_WRONG_DATA_MANIPULATION",
		"code": 400,
		"message": "Bad X-Verify-SHA256 format: WRONG_STRING"
	},
	"success": false
}
```

> **Note:** We only check integrity on chunked uploads. You need to do checksums yourself on single uploads.

## Chunked Uploads (Append)

We support chunked uploads for large files. Use `X-Content-Range` to identify chunks position. Do not overwirte failed chunks. Instead, start over.

Let's illustrate our example:

### First chunk

We upload our first chunk. Let `X-Content-Range`. If successful, server will return headers including `X-SHA256` and `X-Session-Id`.

#### Request

```http
PUT /v2/object/{object-name} HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Expect: 100-Continue
X-Content-Range: bytes 0-9/250

<CHUNK_1_DATA>
```

We set `X-Content-Range` so that server understands this is a chunked upload.

#### Response

```
Status: 201 CREATED
```
```http
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Access-Control-Expose-Headers: X-SHA256,X-Session-Id,Content-Length,X-RateLimit-Limit,X-RateLimit-Remaining,X-RateLimit-Reset
X-Session-Id: df0b106922fa50ed7bff4bcb04d203bb62142e52
X-SHA256: 342c07c4d34ba39a2f727832677b6bcaade3c9c2806057916dd21079af030066
Content-Length: 10
```

Now we successfully created our object. We've got `X-Session-Id` and `X-SHA256`. We will post these two headers in our next chunk.

### Second and later chunks (Append)

Always post previous `X-SHA256` and `X-Session-ID` with every new chunk.

> **Atention!** Do not post very first `X-SHA256` with every new chunk. Always post one previous hash. This way we ensure our integrity of uploaded data so far.

#### Request

```http
PUT /v2/object/{object-name} HTTP/1.1
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Expect: 100-Continue
X-Session-Id: df0b106922fa50ed7bff4bcb04d203bb62142e52
X-Verify-SHA256: 342c07c4d34ba39a2f727832677b6bcaade3c9c2806057916dd21079af030066
X-Content-Range: bytes 10-250/250

<CHUNK_2_DATA>
```

#### Response

```
Status: 201 CREATED
```
```http
Host: api.bitpaket.com
Content-Type: application/json; charset=utf-8
Authorization: Bearer <YOUR_TOKEN>
Access-Control-Expose-Headers: X-SHA256,X-Session-Id,Content-Length,X-RateLimit-Limit,X-RateLimit-Remaining,X-RateLimit-Reset
X-Session-Id: df0b106922fa50ed7bff4bcb04d203bb62142e52
X-SHA256: 9e191c5f1d1327edf51085732de4d298b2e96bbff315876f3df2406e148c3e6e
Content-Length: 240
```

As you can see we've got our new SHA256 hash of data uploaded so far. On next chunk use this hash value.

## Last Chunk (Finalize)

As we get `X-Content-Range`, we already know if current chunk is last one or not. In this case, we return object metadata to the client.

See above for metadata.

Congratulations, you've successfully processed chunked upload.

Want more? See [tus protocol](/upload-using-tus-protocol)
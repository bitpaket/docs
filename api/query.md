# Querying Objects
{:tools}

Querying endpoint allows you to browse, search and inspect your paket.

{:toc}

## Querying paket without search

```http
GET /v2/query HTTP/1.1
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
		"storage": {
			"objects": 115278457007,
			"folders": 62069202,
			"objects_pretty": "107 GB",
			"folders_pretty": "59.2 MB"
		},
		"folders": [
			{
				"folder_id": null,
				"folder_name": "/",
				"size": 0,
				"size_pretty": "0 B",
				"created_on": null,
				"modified_on": null,
				"created_on_ts": 0,
				"modified_on_ts": 0
			},
			{
				"folder_id": "d9WTppip38MU0c2rHw10",
				"folder_name": "anotherDir3",
				"size": 123181,
				"size_pretty": "120 KB",
				"created_on": "2018-06-08T14:20:55Z",
				"modified_on": "2018-06-08T14:20:55Z",
				"created_on_ts": 1528467655,
				"modified_on_ts": 1528467655
			},
			{
				"folder_id": "dI1ucivoAAovdGu4j73H",
				"folder_name": "anotherDir2",
				"size": 8017,
				"size_pretty": "7.83 KB",
				"created_on": "2018-06-08T14:15:57Z",
				"modified_on": "2018-06-08T14:15:57Z",
				"created_on_ts": 1528467357,
				"modified_on_ts": 1528467357
			},
			{
				"folder_id": "hO3dYrz4wfZoH02Cmy3T",
				"folder_name": "anotherDir",
				"size": 61938004,
				"size_pretty": "59.1 MB",
				"created_on": "2018-06-02T23:34:07Z",
				"modified_on": "2018-06-02T23:34:07Z",
				"created_on_ts": 1527982447,
				"modified_on_ts": 1527982447
			}
		],
		"objects": [
			{
				"object_id": "0ebc48d5-894d-453e-91fe-224df058e7f7",
				"properties": {
					"name": "06a68645e8e7b93926210888ef3f4c22.svg",
					"size": 450,
					"size_pretty": "450 B",
					"is_secure": false,
					"is_block": false,
					"version": 1,
					"revision": "3e889ce7af",
					"created_on": "2018-06-10T19:11:06Z",
					"updated_on": "2018-06-10T19:11:06Z",
					"created_on_ts": 1528657866,
					"updated_on_ts": 1528657866
				},
				"urls": {
					"url": "https://test.bitpaket.com/a68645e8e7b93926210888ef3f4c22.svg",
					"domains": []
				}
			},
			{
				"object_id": "f772c73c-d00a-4535-b6f0-0a1062f491b4",
				"properties": {
					"name": "30167824_10216214425218791_2733326637603703107_o.jpg",
					"size": 245476,
					"size_pretty": "240 KB",
					"is_secure": false,
					"is_block": false,
					"version": null,
					"revision": "d1a190abc5",
					"created_on": "2018-05-22T13:16:41Z",
					"updated_on": "2018-05-22T13:16:41Z",
					"created_on_ts": 1526995001,
					"updated_on_ts": 1526995001
				},
				"urls": {
					"url": "https://test.bitpaket.com/30167824_10216214425218791_2733326637603703107_o.jpg",
					"domains": []
				}
			},
			{
				"object_id": "3f5fc05c-d30a-4698-805d-34cb3d1e017f",
				"properties": {
					"name": "30420158_10156735928752668_862266529274205025_o.jpg",
					"size": 149440,
					"size_pretty": "146 KB",
					"is_secure": false,
					"is_block": false,
					"version": null,
					"revision": "403018cdad",
					"created_on": "2018-05-22T12:59:06Z",
					"updated_on": "2018-05-22T12:59:06Z",
					"created_on_ts": 1526993946,
					"updated_on_ts": 1526993946
				},
				"urls": {
					"url": "https://test.bitpaket.com/30420158_10156735928752668_862266529274205025_o.jpg",
					"domains": []
				}
			},
			{
				"object_id": "b39487fd-ac29-4548-81d6-37ac34f8ba43",
				"properties": {
					"name": "352-400-01.png",
					"size": 2368,
					"size_pretty": "2.31 KB",
					"is_secure": false,
					"is_block": false,
					"version": 1,
					"revision": "87e92c2db4",
					"created_on": "2018-06-10T18:05:40Z",
					"updated_on": "2018-06-10T18:05:40Z",
					"created_on_ts": 1528653940,
					"updated_on_ts": 1528653940
				},
				"urls": {
					"url": "https://test.bitpaket.com/352-400-01.png",
					"domains": []
				}
			},
            {
				"object_id": "f86ab095-0f14-41a9-a33f-c2d3ef42549e",
				"properties": {
					"name": "test.rar",
					"size": 115149020000,
					"size_pretty": "107 GB",
					"is_secure": false,
					"is_block": false,
					"version": 1,
					"revision": "8c3e065b3f",
					"created_on": "2018-05-16T17:16:20Z",
					"updated_on": "2018-05-16T17:16:40Z",
					"created_on_ts": 1526490980,
					"updated_on_ts": 1526491000
				},
				"urls": {
					"url": "https://test.bitpaket.com/test.rar",
					"domains": []
				}
			}
		]
	}
}
```

`:storage` parameter is an object. This is properties of your current directory. This object includes these information:

| Key        | Description |
| ------------- |--------------|
| `objects` | Total object size of this directory in bytes |
| `folders` | Total folder size which is in this directory in bytes |
| `objects_pretty` | Total object size of this directory (**Prettified**) |
| `folders_pretty` | Total folder size which is in this directory (**Prettified**) |

### Folders

`:folders` parameter is an array. This array contains folders. Each array includes these information:

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `folder_id`      | Folder ID represented in your paket |
| `folder_name`        | Folder name |
| `size` | Total object size of this directory in bytes |
| `size_pretty` | Total object size of this directory (**Prettified**) |
| `created_on` | Creation date |
| `modified_on` | Last modification date (any object modified, also modifies base folder) |
| `created_on_ts` | Creation date in unix timestamp |
| `modified_on_ts` | Last modification date in unix timestamp (any object modified, also modifies base folder) |

### Objects

`:objects` parameter is an array. This array contains objects. Each array includes these information:

| Key              | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `object_id`      | Unique Object ID |
| `properties`        | Object properties. See below |
| `urls`        | Object URIs. See below |

###Properties

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

### Additional

`:tags` parameter is an array without any key. This array contains tags of your object. 

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
Authorization: Bearer <YOUR_TOKEN>
```

### Response

```
Status: 200 OK
```
```
For response content, see above.
```

### Parameters

| Key    | Description                                                        |
| -------------- | ------------------------------------------------------------ |
| `X-Bit-Search` | Search query. Write any words to this parameter. Any matched objects will be listed. This is non-recursively. |
| `X-Bit-Folder` | Set folder. Use full paths that starts with slash. (E.g.: /dir/another) |
| `X-Bit-Tags` | Set Tags. You can use multiple tags separated with comma. (E.g.: tag1,tag2,tag3). Any matched object will be fetched. |
| `X-Bit-Detailed` | Should be `1` or `0`. This indicates that response should be detailed and not shortened. Default is `0`. All metadata properties can be found at [/metadata](/metadata) |
| `X-Bit-All` | Should be `1` or `0`. This indicates that all objects and if exists any of its versions should be also fetched. Default is `0` |
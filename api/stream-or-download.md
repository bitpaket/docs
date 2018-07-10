# Downloading (Getting) Objects and Streaming
{:tools}

You can download and fetch objects or stream all media types easily with BitPaket. For doing so, please follow instructions.

{:toc}

## Download an object

We assume that, your paket name is `test` and your object is in `/myfile.zip` path.

We will use this scheme:

```http
GET /{object-path} HTTP/1.1
Host: {paket-name}.bitpaket.com
```

For this scenario we will send our request like this:

```http
GET /myfile.zip HTTP/1.1
Host: test.bitpaket.com
```
As simply as:

```http
GET http://test.bitpaket.com/myfile.zip
```

### Response

```
Status: 200 OK
```
```json
<BINARY_DATA>
...
```

### Downloading specific version

In case your objects has multiple versions, you can download specific version by letting a parameter to the request.

| Key        | Description |
| ------------- |--------------|
| `X-Bit-Version` | Either `revision` (hex value, all lowercase) or `integer` value for `version` |

### Request using Version

```http
GET /{object-path}?X-Bit-Version=5 HTTP/1.1
Host: {paket-name}.bitpaket.com
```

### Response

```
Status: 200 OK
```

```json
<BINARY_DATA>
...
```

### Request using Revision

```http
GET /{object-path}?X-Bit-Version=1bc7311203 HTTP/1.1
Host: {paket-name}.bitpaket.com
```

### Response

```
Status: 200 OK
```

```json
<BINARY_DATA>
...
```

> **Note:** If object is not secured, versioning will not require any pre-signed URL.
>
> However, If object or one of version has require pre-signed URL, You need to sign your URL before request. See below for pre-signed URL's.

### Advanced Configuration and Pre-Signed URLs

We also support some advanced configuration in runtime. If object entirely or one of its version has an is_secure set to true, you need pre-signed key. Along with this, any of below parameters are also be signed for secured objects.

We assume our object is secure. Now, we need to create signed key. You need Client Secret and Access Key ID for this action. Both parameters can be found in your customer portal. Each secret and key is separately belongs to individual Paket's. Please do not use another Paket's key or secret together.

> **Attention!** Before signing, all parameters should be alphabetically ordered ( A to Z ). Otherwise, server-side verification will fail. You can find ruby sample below. Implementing in other languages shouldn't be harder.

| Key              | Description                                                  |
| :--------------- | ------------------------------------------------------------ |
| `X-Bit-Filename` | Overrides filename. In some cases, you may want to override downloaded content's filename. This query parameter is for that. Works both with or without pre-signed URL. |
| `X-Bit-Expire` | You may want to create pre-signed URLs live for a limited time. This key is doing that. **Only works for secured objects**. |
| `X-Bit-Version` | Hex-valued revision or integer-valued version. Works both with or without pre-signed URL. |
| `X-Bit-Client-Ip` | In some cases, you may want to restrict objects for downloadable for a specific IP address. Currently we do only support IPv4. **Only works for secured objects**. |
| `X-Bit-Access-Key` | Along with `X-Bit-Signature`, this value should be set plain-text Access Key ID which can be found in your Customer Portal. Only works with `X-Bit-Signature`. **Only works for secured objects**. |
| `X-Bit-Signature` | All above headers should be alphabetically signed, and signing key should be provided with this query parameter. **Only works for secured objects**. |



## Signing

Signing your request query parameters are easy. Follow this instructions. We provided Ruby sample, However, Implementing on other languages shouldn't be harder. In any case, you may contact with our support.

### Ruby snippet

```ruby
require 'openssl'
require 'Base64'
require 'json'

# @reference: http://dan.doezema.com/2012/04/recursively-sort-ruby-hash-by-key/
class Hash
  def sort_by_key(recursive = false, &block)
    self.keys.sort(&block).reduce({}) do |seed, key|
      seed[key] = self[key]
      if recursive && seed[key].is_a?(Hash)
        seed[key] = seed[key].sort_by_key(true, &block)
      end
      seed
    end
  end
end

secret_key = "client_secret" # Your Client Secret
access_key = 'BITPAKETXD5wQ_HKXerVtdXc' # Your Access Key ID
t = Time.now.utc
t = t + (60 * 30 * 10)
object_path = '/myfile.zip' # Object full path starts with '/'
# file_name = 'myfile123.zip' # Filename  - if you want to override -

data = { :'X-Bit-Expire' => t.to_i, 
         :'X-Bit-Resource' => object_path,
         :'X-Bit-Access-Key-Id' => access_key,
		 :'X-Bit-Version' => 11 }

# if we override file name, then we need to add
#    :'X-Bit-Filename' => file_name
# to the object list.
	 
data = data.sort_by_key(true) {|x, y| x.to_s <=> y.to_s}
f = OpenSSL::HMAC.hexdigest('SHA256', access_key, Base64.encode64(data.to_json)).strip()

puts 'Your PreSigned Key is:'
puts OpenSSL::HMAC.hexdigest('SHA256', secret_key, f).strip()

# URL shoudl be similar to this:
# http://{paket-name}.bitpaket.com/myfile.zip?X-Bit-Access-Key-Id=BITPAKETXD5wQ_HKXerVtdXc&X-Bit-Expire=1531238826&X-Bit-Signature=45477408dfc9d949f4d308ed1e26a1dc5d2b6e03ab72479f4fc15a7ea7ab8570&X-Bit-Version=11

```
> **Note:** All hashed and signed params should exists in URL in plain-text. We compare those values in server-side.

> **Attention:** As you can see in above sample, ver also set Version. In this way, clients cannot download another version without forcibly requesting a new signed URL from you for that specific version. With this way, we able to limit access on specific versions. 

This is scheme for signing key:

```ruby
HMAC_SHA256(HMAC_SHA256(BASE64(<ORDERED_DATA_AS_JSON>), access_key_id), client_secret)
```

> **Warning:** Do not beautify JSON. Please use standard JSON output.

## Streaming

If any object supports streaming, you can enable streaming (forcibly stream not download) with adding `?stream` parameter to the URL. See below:

### Request

```http
GET /content.avi?stream HTTP/1.1
Host: {paket-name}.bitpaket.com
```

### Response

```
Status: 200 OK
```
```json
<CHUNKED_BINARY_DATA>
...
```

> **Warning** Streaming only works if object is has streamable content. Any static files will foricbly downloaded and treated as attachment in browsers.

## ZipBalls

When zipball is requested to download, you need to add one extra parameter to the URL like stream. It is `?zipball`

### Request

```http
GET /06ef223f-70de-4acd-b0d7-77faf22fbcc9.zip?zipball HTTP/1.1
Host: {paket-name}.bitpaket.com
```

### Response

```
Status: 200 OK
```
```json
<ZIPPED_DATA>
...
```

> **Info**: We do support resumable downloads on zipballs. You can pause and later resume zipballs downloads.
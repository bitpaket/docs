---
title: Object | BitPaket Docs
description: BitPaket's Objects
service: api
main: ''
author: gencer
icon: copy
seo: ''
priority: 0
---

# Objects
{:tools}

{:toc}

BitPaket allows you to do some operations on objects in your paket. Those would be MOVE and DELETE operations. You can move objects and even nested folders and their siblings to another target.

## MOVE Operation

Move operation can be set to overwrite so that any target found will be replaced with source.

> **Attention!**  Any overwritten object will be permamently deleted including previous versions

See [/objects/move-objects](/objects/move-objects)

## DELETE Operation

Delete operation requires **SECRET KEY**. Without secret key provided, termination will not be possible.

> **Note**: It's recommended to keep DELETE requests in backend system and not expose to end users.

See [/objects/delete-objects](/objects/delete-objects)

## COPY Operation

Copy operation can be set to overwrite so that any target found will be replaced with source.

> **Note**: It's recommended to keep COPY requests in backend system and not expose to end users.

See [/objects/copy-objects](/objects/copy-objects)

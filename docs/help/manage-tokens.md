---
title: Managing your Tokens | BitPaket Docs
description: Learn how to List, Create and Delete Tokens on BitPaket
service: help
main: ''
author: gencer
icon: network-4
seo: ''
priority: 8900
---

# Manage your Tokens
{:tools}

After create your very first [Endpoint](https://www.bitpaket.com/paket/api), yopu can now manage your tokens by reading this article.

{:toc}

## Create and Delete Tokens

Visit Manage > [Endpoints](https://www.bitpaket.com/paket/api) to see your endpoints.

![pakets](./images/api.png)

Click **Show Tokens** as seen in above image.

### Create

When you click **Show Tokens** as stated above, click on **Tokens** and you will see this screen:

![api_create_token_list](./images/api_tokens_list.png)

Click on **Create** and open this dialog:

![api_create_token_list](./images/api_tokens_create.png)

If you prefer *expiration*, enable this feature and either give time **in seconds** (*like >= 3600*) or by **date**. Otherwise, this token will live forever.

When **success**, you will see below message:

![api_create_done](./images/api_create_done.png)

Now, close this message box and your token list will be refreshed. You can copy your token and start using it.

> Note: While you can do many things with your token, some actions will require **Client Secret**. You can find this in **Credentials** tab.

### Delete

Open Token list and click **Delete* Link that you want to remove token. You will be asked if you are sure. Make sure this is what you want. After clicking yes, Your token will be deleted.

![api_create_delete](./images/api_token_delete.png)

> Attention! There is no way back for this action. Please make sure that this token is not used on critical applications. All applications use this token will receive Authentication Failed message.

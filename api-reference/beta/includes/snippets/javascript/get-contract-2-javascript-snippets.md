---
description: "Automatically generated file. DO NOT MODIFY"
---

```javascript

const options = {
	authProvider,
};

const client = Client.init(options);

let contracts = await client.api('/contracts')
	.version('beta')
	.get();

```
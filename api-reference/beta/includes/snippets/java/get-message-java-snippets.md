---
description: "Automatically generated file. DO NOT MODIFY"
---

```java

GraphServiceClient graphClient = GraphServiceClient.builder().authenticationProvider( authProvider ).buildClient();

Message message = graphClient.me().messages("AAMkAGI1AAAoZCfHAAA=")
	.buildRequest()
	.get();

```
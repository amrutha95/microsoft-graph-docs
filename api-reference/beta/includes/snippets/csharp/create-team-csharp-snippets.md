---
description: "Automatically generated file. DO NOT MODIFY"
---

```csharp

GraphServiceClient graphClient = new GraphServiceClient( authProvider );

var team = new Team
{
	MemberSettings = new TeamMemberSettings
	{
		AllowCreateUpdateChannels = true
	},
	MessagingSettings = new TeamMessagingSettings
	{
		AllowUserEditMessages = true,
		AllowUserDeleteMessages = true
	},
	FunSettings = new TeamFunSettings
	{
		AllowGiphy = true,
		GiphyContentRating = GiphyRatingType.Strict
	},
	DiscoverySettings = new TeamDiscoverySettings
	{
		ShowInTeamsSearchAndSuggestions = true
	}
};

await graphClient.Groups["{group-id}"].Team
	.Request()
	.PutAsync(team);

```
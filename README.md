# ExpChatRemake (Custom Chat System)
I decided to upload the source of chat system because i tired of reuploading the game (Custom Chat) many times because of roblox targeting it (maybe because of mass reporting, if this is true, ik who exactly does that)

# How to install ExpChatRemake
1. Download rbxm file in [releases](https://github.com/Tplay4/Custom-Chat-System/releases/tag/tag) and setup everything.
2. Set 'TextChatService.CreateDefaultTextChannels' to 'false'.
3. You're done!


The point of this chat system is allow players from all age groups and all regions talk without restrictions.


# Tutorial how to make good and secure custom chat

Step 1: Chat system (the most important step)

To make your chat system you need to
1 - Create Script in ServerScriptService that would receive messages sent by players
2 - Chat GUI and localscript that will handle these message received from server

Step 2: Message filtering

You can use function that roblox made in their docs about filtering
```lua
local function GetFilterResult(text, fromUserId)
	local filterResult
	local success, errorMessage = pcall(function()
		filterResult = TextService:FilterStringAsync(text, fromUserId)
	end)

	if success then
		return filterResult
	else
        warn("Error generating TextFilterResult:", errorMessage)
		return nil
	end
end
```

To filter message you can simply do this
```lua
local success, _ = pcall(function()
    message = GetFilterResult(message, player.UserId):GetNonChatStringForBroadcastAsync()
end)
if not success then
    message = table.concat(table.create(#message, "#")) -- CENSORING MESSAGE IF IT FAILED TO GET FILTER RESULT
end
```

Step 3: Prevent players from abusing whitespaces and rich text (ADD ANTI RICH TEXT ONLY WHEN YOUR MESSAGES HAS RICHTEXT = TRUE)

1. Whitespaces - Unicode characters like line seperators or spaces, people can abuse it to create "fake server messages"

To replace all whitespaces you can use this
```lua
message:gsub("[%s]+", " ")
```

2. Rich text - text with some formatting elements (colors, highlights, strokes, bold text, italic text)

If you messages have rich text enabled you should add this to prevent people from abusing it
```lua
message = message:gsub(`[&<>"']`, {
	["&"] = "&amp;",
	["<"] = "&lt;",
	[">"] = "&gt;",
	['"'] = '&quot;',
	["'"] = '&apos;',
})
```

Step 4: You can add some other protections like anti spam or improve text filtering by removing punctiations and not necessary characters in message and then filter this message and original message.

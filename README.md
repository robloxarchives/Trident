# Lobby
```
setfflag("DebugRunParallelLuaOnMainThread","True");
```
# PVP (Amongus enabled)
Note: I removed CamFix script so it doesn't crash the game if you use it together with amongushook. Therefore you won't be able to raid with amongushook.
```
local scripts = {
	'https://raw.githubusercontent.com/NotH4xor/Fallen-Scripts/main/Amongus',
	'https://raw.githubusercontent.com/robloxarchives/Trident/main/NewATVFly',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/FreeSwimhub',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_ESP',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/ChunkRemoval(HoldR)',
	'https://raw.githubusercontent.com/1337h4xx/1/main/X_3rd_Person',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_FREECAM',
	'https://raw.githubusercontent.com/robloxarchives/CurrentConfig/main/Delete'
}
for _, url in ipairs(scripts) do
    local thread = coroutine.create(function()
        loadstring(game:HttpGet(url, true))()
    end)
    coroutine.resume(thread)
end
table.insert(scripts, "new_script_url")
```

# RAID (Amongus disabled)
```
local scripts = {
	'https://raw.githubusercontent.com/robloxarchives/Trident/refs/heads/main/CamFix',
	'https://raw.githubusercontent.com/robloxarchives/Trident/main/NewATVFly',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/FreeSwimhub',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_ESP',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/ChunkRemoval(HoldR)',
	'https://raw.githubusercontent.com/1337h4xx/1/main/X_3rd_Person',
	'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_FREECAM',
	'https://raw.githubusercontent.com/robloxarchives/CurrentConfig/main/Delete'
}
for _, url in ipairs(scripts) do
    local thread = coroutine.create(function()
        loadstring(game:HttpGet(url, true))()
    end)
    coroutine.resume(thread)
end
table.insert(scripts, "new_script_url")
```

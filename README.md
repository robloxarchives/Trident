# Lobby
```
setfflag("DebugRunParallelLuaOnMainThread","True");
```
# PVP
Note: FixCamera script will the game if you use it together with amongushook
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

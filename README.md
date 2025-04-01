# Lobby
```
setfflag("DebugRunParallelLuaOnMainThread","True");
```
# Script
Note: FixCamera script will the game if you use it together with amongushook
```
local scripts = {
    'https://raw.githubusercontent.com/NotH4xor/Fallen-Scripts/main/Amongus',
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

# RaidWithATV
```
local userInput = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local currentCamera = workspace.CurrentCamera
local players = game:GetService("Players")
local localPlayer = players.LocalPlayer

local keyToToggleFly = Enum.KeyCode.N
local flySpeed = 15
local rotateSpeed = math.rad(0.8) -- using radians for rotational speed

local flying = false
local connections = {}
local flyObjects = {}

local criteriaList = {
  {partName="Plastics",partSize=Vector3.new(7.551962852478027,2.7462241649627686,4.972546577453613)},
  {partName="BackLeftRim",partSize=Vector3.new(1.067999005317688, 1.067999005317688, 0.7230425477027893)},
  {partName="BackLeftW",partSize=Vector3.new(2.1518802642822266, 2.1898412704467773, 0.9632723927497864)},
  {partName="BackRightRim",partSize=Vector3.new(1.067999005317688, 1.067999005317688, 0.7230425477027893)},
  {partName="BackRightW",partSize=Vector3.new(2.1518802642822266, 2.1898412704467773, 0.9632723927497864)},
  {partName="Frame",partSize=Vector3.new(6.972168922424316, 2.774535655975342, 5.0449066162109375)},
  {partName="FrontLeftRim",partSize=Vector3.new(1.067999005317688, 1.067999005317688, 0.7230425477027893)},
  {partName="FrontLeftW",partSize=Vector3.new(2.1518802642822266, 2.1898412704467773, 0.7132723927497864)},
  {partName="FrontRightRim",partSize=Vector3.new(1.067999005317688, 1.067999005317688, 0.7230425477027893)},
  {partName="FrontRightW",partSize=Vector3.new(2.1518802642822266, 2.1898412704467773, 0.7132723927497864)},
  {partName="Grip",partSize=Vector3.new(0.5733855366706848, 0.9605950117111206, 2.1716928482055664)},
  {partName="HandleBars",partSize=Vector3.new(0.448628693819046, 1.376966953277588, 2.0683438777923584)},
  {partName="Plastics2",partSize=Vector3.new(5.546261787414551, 1.9555978775024414, 3.854473352432251)},
  {partName="Seat",partSize=Vector3.new(4.404223442077637, 1.439717411994934, 2.2254559993743896)},
  {partName="LeftBackWH",partSize=Vector3.new(0.625, 0.625, 0.125)},
  {partName="LeftFrontWH",partSize=Vector3.new(0.625, 0.625, 0.125)},
  {partName="RightBackWH",partSize=Vector3.new(0.625, 0.625, 0.125)},
  {partName="RightFrontWH",partSize=Vector3.new(0.625, 0.625, 0.125)}
}

local function isMatchingSize(partSize, targetSize, tolerance)
    tolerance = tolerance or 0.001
    return math.abs(partSize.X - targetSize.X) <= tolerance and
        math.abs(partSize.Y - targetSize.Y) <= tolerance and
        math.abs(partSize.Z - targetSize.Z) <= tolerance
end

local function matchesCriteria(part)
    for _, criteria in ipairs(criteriaList) do
        if part:IsA("BasePart") and part.Name == criteria.partName and isMatchingSize(part.Size, criteria.partSize) then
            return true
        end
    end
    return false
end

local function controlFly(targetPart)
    local flyVector = Vector3.new(0, 0, 0)
    local rotateCFrame = CFrame.new()

    if not userInput:GetFocusedTextBox() and flying then
        if userInput:IsKeyDown(Enum.KeyCode.W) then
            flyVector = flyVector + currentCamera.CFrame.LookVector * flySpeed
        end
if userInput:IsKeyDown(Enum.KeyCode.A) then
            flyVector = flyVector - currentCamera.CFrame.RightVector * flySpeed
        end
        if userInput:IsKeyDown(Enum.KeyCode.S) then
            flyVector = flyVector - currentCamera.CFrame.LookVector * flySpeed
        end
        if userInput:IsKeyDown(Enum.KeyCode.D) then
            flyVector = flyVector + currentCamera.CFrame.RightVector * flySpeed
        end
        if userInput:IsKeyDown(Enum.KeyCode.F) then
            flyVector = flyVector + currentCamera.CFrame.UpVector * flySpeed
        end
        if userInput:IsKeyDown(Enum.KeyCode.LeftControl) then
            flyVector = flyVector - currentCamera.CFrame.UpVector * flySpeed
        end
        if userInput:IsKeyDown(Enum.KeyCode.R) then
            rotateCFrame = rotateCFrame * CFrame.Angles(0, -rotateSpeed, 0)
        end
        if userInput:IsKeyDown(Enum.KeyCode.Q) then
            rotateCFrame = rotateCFrame * CFrame.Angles(0, rotateSpeed, 0)
        end
        if userInput:IsKeyDown(Enum.KeyCode.O) then
            rotateCFrame = rotateCFrame * CFrame.Angles(-rotateSpeed, 0, 0)
        end
        if userInput:IsKeyDown(Enum.KeyCode.F) then
            rotateCFrame = rotateCFrame * CFrame.Angles(rotateSpeed, 0, 0)
        end

        targetPart.CFrame = targetPart.CFrame * rotateCFrame
    else
        targetPart.Velocity = Vector3.new()
        targetPart.RotVelocity = Vector3.new()
    end

    targetPart.Velocity = flyVector
end

userInput.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == keyToToggleFly then
        flying = not flying
        if flying then
            for _, obj in ipairs(flyObjects) do
                obj.Anchored = false -- ensure part is not anchored
                local conn = runService.Heartbeat:Connect(function()
                    controlFly(obj)
                end)
                table.insert(connections, conn)
            end
        else
            for _, conn in ipairs(connections) do
                conn:Disconnect()
            end
            connections = {}
        end
    end
end)

workspace.DescendantAdded:Connect(function(descendant)
    if matchesCriteria(descendant) then
        table.insert(flyObjects, descendant)
    end
end)

workspace.DescendantRemoving:Connect(function(descendant)
    for index, obj in ipairs(flyObjects) do
        if obj == descendant then
            table.remove(flyObjects, index)
            break
        end
    end
end)

for _, descendant in ipairs(workspace:GetDescendants()) do
    if matchesCriteria(descendant) then
        table.insert(flyObjects, descendant)
    end
end
```

# ORIGINAL
```
local scripts = {
    'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_ESP',
    'https://raw.githubusercontent.com/NotH4xor/trident/main/ChunkRemoval(HoldR)',
    'https://raw.githubusercontent.com/1337h4xx/1/main/X_3rd_Person',
    'https://raw.githubusercontent.com/NotH4xor/trident/main/UD_FREECAM',
    'https://raw.githubusercontent.com/NotH4xor/trident/main/CameraFix',
    'https://raw.githubusercontent.com/robloxarchives/CurrentConfig/main/Delete',
    'https://raw.githubusercontent.com/NotH4xor/Fallen-Scripts/main/Amongus',
    'https://raw.githubusercontent.com/NotH4xor/trident/main/FreeSwimhub'
}
for _, url in ipairs(scripts) do
    local thread = coroutine.create(function()
        loadstring(game:HttpGet(url, true))()
    end)
    coroutine.resume(thread)
end
table.insert(scripts, "new_script_url")
```

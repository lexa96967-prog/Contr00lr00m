-- Orbital sky destroyer By REI_txt (Versi Simple Merah)
-- GUI simple, warna merah, draggable, tombol DESTROY (ON) dan STOP (OFF)
-- Setelah karakter reset tetap bisa dipakai lagi

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Hapus GUI lama jika ada
if playerGui:FindFirstChild("OrbitalSkyDestroyerGUI") then
    playerGui.OrbitalSkyDestroyerGUI:Destroy()
end

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "OrbitalSkyDestroyerGUI"
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- Frame simple merah
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 210, 0, 126)
main.Position = UDim2.new(0.5, -100, 0.2, 0)
main.BackgroundColor3 = Color3.fromRGB(139, 20, 20)
main.BorderSizePixel = 0
main.Parent = gui
local cr = Instance.new("UICorner", main)
cr.CornerRadius = UDim.new(0, 10)

-- Label judul
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 32)
title.BackgroundTransparency = 1
title.Text = "Orbital|sky|destroyer | By REI_txt"
title.TextColor3 = Color3.new(1,1,1)
title.TextSize = 16
title.Font = Enum.Font.SourceSansBold
title.Parent = main

-- Tombol DESTROY
local destroyBtn = Instance.new("TextButton")
destroyBtn.Size = UDim2.new(1, -20, 0, 36)
destroyBtn.Position = UDim2.new(0, 10, 0, 40)
destroyBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
destroyBtn.Text = "DESTROY"
destroyBtn.TextColor3 = Color3.new(1,1,1)
destroyBtn.TextSize = 18
destroyBtn.Font = Enum.Font.SourceSansBold
destroyBtn.Parent = main
local c1 = Instance.new("UICorner", destroyBtn)

-- Tombol STOP
local stopBtn = Instance.new("TextButton")
stopBtn.Size = UDim2.new(1, -20, 0, 36)
stopBtn.Position = UDim2.new(0, 10, 0, 80)
stopBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
stopBtn.Text = "STOP"
stopBtn.TextColor3 = Color3.new(1,1,1)
stopBtn.TextSize = 18
stopBtn.Font = Enum.Font.SourceSansBold
stopBtn.Parent = main
local c2 = Instance.new("UICorner", stopBtn)

-- HUMANOID selalu update ketika reset
local humanoid
local animationAsset
local ANIMATION_ID = "rbxassetid://70883871260184"

local function loadCharacter()
    local char = player.Character or player.CharacterAdded:Wait()
    humanoid = char:WaitForChild("Humanoid")
    
    animationAsset = Instance.new("Animation")
    animationAsset.AnimationId = ANIMATION_ID
end

loadCharacter()
player.CharacterAdded:Connect(loadCharacter)

-- Heartbeat connection
local heartbeatConnection = nil

-- tombol DESTROY (ON)
destroyBtn.MouseButton1Click:Connect(function()
    if heartbeatConnection then return end
    if not humanoid then return end

    heartbeatConnection = RunService.Heartbeat:Connect(function()
        local t = humanoid:LoadAnimation(animationAsset)
        t:Play()
    end)
end)

-- tombol STOP (OFF)
stopBtn.MouseButton1Click:Connect(function()
    if heartbeatConnection then
        heartbeatConnection:Disconnect()
        heartbeatConnection = nil
    end
end)

-- DRAGGABLE
local dragging = false
local dragStart
local startPos

local function updateDrag(input)
    local delta = input.Position - dragStart
    main.Position = UDim2.new(0, startPos.X.Offset + delta.X, 0, startPos.Y.Offset + delta.Y)
end

title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = main.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)


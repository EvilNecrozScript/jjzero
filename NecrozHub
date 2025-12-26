-- Services
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer

-- SETTINGS
local CRATE_NAME = "HolidayCrate_1"
local ENABLED_CRATE = false
local ENABLED_PUNCH = false -- neuer Toggle für Punch

-- ================= UI =================

local gui = Instance.new("ScreenGui")
gui.Name = "CrateHub"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.fromScale(0.25, 0.3) -- etwas höher für 2 Buttons
frame.Position = UDim2.fromScale(0.375, 0.4)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = gui

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel")
title.Size = UDim2.fromScale(1, 0.25)
title.BackgroundTransparency = 1
title.Text = "NecrozHub"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextScaled = true
title.Parent = frame

-- Toggle Button für Crates
local toggleCrateBtn = Instance.new("TextButton")
toggleCrateBtn.Size = UDim2.fromScale(0.8, 0.25)
toggleCrateBtn.Position = UDim2.fromScale(0.1, 0.3)
toggleCrateBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
toggleCrateBtn.Text = "CRATE OFF"
toggleCrateBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleCrateBtn.Font = Enum.Font.GothamBold
toggleCrateBtn.TextScaled = true
toggleCrateBtn.Parent = frame

local crateBtnCorner = Instance.new("UICorner", toggleCrateBtn)
crateBtnCorner.CornerRadius = UDim.new(0, 10)

-- Toggle Button für Punch
local togglePunchBtn = Instance.new("TextButton")
togglePunchBtn.Size = UDim2.fromScale(0.8, 0.25)
togglePunchBtn.Position = UDim2.fromScale(0.1, 0.65)
togglePunchBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
togglePunchBtn.Text = "PUNCH OFF"
togglePunchBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
togglePunchBtn.Font = Enum.Font.GothamBold
togglePunchBtn.TextScaled = true
togglePunchBtn.Parent = frame

local punchBtnCorner = Instance.new("UICorner", togglePunchBtn)
punchBtnCorner.CornerRadius = UDim.new(0, 10)

-- ================= LOGIC =================

-- Funktion Crate öffnen
local function openCrate()
    ReplicatedStorage.NetworkComm.MapService.OpenExplorationCrate_Method:InvokeServer(CRATE_NAME)
end

Workspace.DescendantAdded:Connect(function(obj)
    if obj.Name == CRATE_NAME and ENABLED_CRATE then
        task.wait(0.2)
        openCrate()
    end
end)

-- Funktion Punch ausführen
local function punchNPCs()
    -- Spieler im Server-Workspace
    local localCharModel = workspace.Characters.Server.Players:FindFirstChild(player.Name.."_Server") 
        or workspace.Characters.Server.Players:GetChildren()[1] -- Fallback

    -- Skill
    local args = {
        [1] = "Punch",
        [2] = localCharModel,
        [3] = Vector3.new(-520.218017578125, 84.97281646728516, 1778.3367919921875),
        [4] = 1,
        [5] = 1
    }
    ReplicatedStorage.NetworkComm.SkillService.StartSkill_Method:InvokeServer(unpack(args))

    -- NPCs
    local npcsFolder = workspace.Characters.Server.NPCs
    local npcList = npcsFolder:GetChildren()
    local npc1, npc2, npc3 = npcList[1], npcList[2], npcList[3]

    local args2 = {
        [1] = { npc1, npc2, npc3 },
        [2] = true,
        [3] = {
            ["CanParry"] = true,
            ["OnCharacterHit"] = function() end,
            ["Origin"] = CFrame.new(-520.218017578125, 84.98957824707031, 1778.3367919921875) * CFrame.Angles(-0, 1.3115626573562622, -0),
            ["LocalCharacter"] = localCharModel,
            ["WindowID"] = localCharModel.Name.."_Punch",
            ["Parries"] = {},
            ["SkillID"] = "Punch"
        }
    }
    ReplicatedStorage.NetworkComm.CombatService.DamageCharacter_Method:InvokeServer(unpack(args2))
end

-- Punch Loop
task.spawn(function()
    while true do
        task.wait(1) -- jede Sekunde
        if ENABLED_PUNCH then
            punchNPCs()
        end
    end
end)

-- ================= TOGGLE =================

toggleCrateBtn.MouseButton1Click:Connect(function()
    ENABLED_CRATE = not ENABLED_CRATE
    if ENABLED_CRATE then
        toggleCrateBtn.Text = "CRATE ON"
        toggleCrateBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    else
        toggleCrateBtn.Text = "CRATE OFF"
        toggleCrateBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    end
end)

togglePunchBtn.MouseButton1Click:Connect(function()
    ENABLED_PUNCH = not ENABLED_PUNCH
    if ENABLED_PUNCH then
        togglePunchBtn.Text = "PUNCH ON"
        togglePunchBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    else
        togglePunchBtn.Text = "PUNCH OFF"
        togglePunchBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    end
end)

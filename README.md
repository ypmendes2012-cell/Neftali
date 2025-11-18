-- Neftali Hub - Full Script Loader Version -- This file will contain only the loadstring loader. -- The full script framework will be added below in future updates.

-- Neftali Hub - Full Script

local NEFTALI = {}

-- UI Loader (RedzHub Style) local function CreateUI() local ScreenGui = Instance.new("ScreenGui") ScreenGui.Name = "NeftaliHub" ScreenGui.Parent = game:GetService("CoreGui")

local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 480, 0, 300)
Main.Position = UDim2.new(0.5, -240, 0.5, -150)
Main.BackgroundColor3 = Color3.fromRGB(25,25,25)
Main.BorderSizePixel = 0
Main.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1,0,0,40)
Title.BackgroundColor3 = Color3.fromRGB(35,35,35)
Title.BorderSizePixel = 0
Title.Text = "Neftali Hub"
Title.TextColor3 = Color3.new(1,1,1)
Title.TextSize = 22
Title.Font = Enum.Font.GothamBold
Title.Parent = Main

return Main

end

local UI = CreateUI()

-- AutoFarm System NEFTALI.AutoFarmEnabled = false NEFTALI.FarmWeapon = "Fruit"

function NEFTALI:GatherNPCs() for _, npc in pairs(workspace:GetDescendants()) do if npc:FindFirstChild("Humanoid") and npc:FindFirstChild("HumanoidRootPart") then npc.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-4) end end end

function NEFTALI:AttackNPCs() local chr = game.Players.LocalPlayer.Character local hum = chr and chr:FindFirstChild("Humanoid")

if hum then
    hum:Move(Vector3.new(0,0,0), true)
    pcall(function()
        if self.FarmWeapon == "Fruit" then
            game:GetService("VirtualUser"):Button1Down(Vector2.new(), workspace.CurrentCamera.CFrame)
        elseif self.FarmWeapon == "Sword" then
            hum:EquipTool(chr:FindFirstChildWhichIsA("Tool"))
        end
    end)
end

end

spawn(function() while task.wait() do if NEFTALI.AutoFarmEnabled then NEFTALI:GatherNPCs() NEFTALI:AttackNPCs() end end end)

-- UI Buttons local FarmButton = Instance.new("TextButton") FarmButton.Size = UDim2.new(0,200,0,40) FarmButton.Position = UDim2.new(0,20,0,60) FarmButton.Text = "Toggle AutoFarm" FarmButton.Parent = UI

FarmButton.MouseButton1Click:Connect(function() NEFTALI.AutoFarmEnabled = not NEFTALI.AutoFarmEnabled end)

local WeaponDropdown = Instance.new("TextButton") WeaponDropdown.Size = UDim2.new(0,200,0,40) WeaponDropdown.Position = UDim2.new(0,20,0,110) WeaponDropdown.Text = "Weapon: Fruit" WeaponDropdown.Parent = UI

WeaponDropdown.MouseButton1Click:Connect(function() if NEFTALI.FarmWeapon == "Fruit" then NEFTALI.FarmWeapon = "Sword" WeaponDropdown.Text = "Weapon: Sword" elseif NEFTALI.FarmWeapon == "Sword" then NEFTALI.FarmWeapon = "Melee" WeaponDropdown.Text = "Weapon: Melee" elseif NEFTALI.FarmWeapon == "Melee" then NEFTALI.FarmWeapon = "Gun" WeaponDropdown.Text = "Weapon: Gun" else NEFTALI.FarmWeapon = "Fruit" WeaponDropdown.Text = "Weapon: Fruit" end end)

-- Safe Mode NEFTALI.SafeMode = true

function NEFTALI:SafeCheck() if self.SafeMode then setfpscap(30) collectgarbage("collect") end end

spawn(function() while task.wait(5) do NEFTALI:SafeCheck() end end)

print("Neftali Hub Loaded.")("Neftali Hub - Loader initialized. Awaiting full script content...")

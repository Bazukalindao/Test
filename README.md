local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 250, 0, 120)
Frame.Position = UDim2.new(0.75, 0, 0.1, 0)
Frame.BackgroundTransparency = 1
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

local BackgroundImage = Instance.new("ImageLabel")
BackgroundImage.Size = UDim2.new(1, 0, 1, 0)
BackgroundImage.Position = UDim2.new(0, 0, 0, 0)
BackgroundImage.Image = "rbxassetid://16843175709"
BackgroundImage.BackgroundTransparency = 1
BackgroundImage.Parent = Frame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 25)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "Bazuka Hub | Blade Ball"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.Parent = Frame

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 230, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0, 35)
ToggleButton.Text = "Auto Parry: OFF"
ToggleButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 18
ToggleButton.Parent = Frame

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -30, 0, 5)
CloseButton.Text = "-"
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 69, 0)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 18
CloseButton.Parent = Frame

local ShowButton = Instance.new("TextButton")
ShowButton.Size = UDim2.new(0, 50, 0, 30)
ShowButton.Position = UDim2.new(0.75, 0, 0.15, 0)
ShowButton.Text = "+"
ShowButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
ShowButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ShowButton.Font = Enum.Font.GothamBold
ShowButton.TextSize = 24
ShowButton.Parent = ScreenGui
ShowButton.Visible = false

local function detectAttack()
    for _, projectile in ipairs(workspace:GetChildren()) do
        if projectile:IsA("Part") and projectile:FindFirstChild("ProjectileData") then
            local target = projectile.ProjectileData.Target.Value
            if target == LocalPlayer then
                return true
            end
        end
    end
    return false
end

local autoParryEnabled = false

local function autoParry()
    RunService.RenderStepped:Connect(function()
        if autoParryEnabled and detectAttack() then
            UserInputService:InputBegan({KeyCode = Enum.KeyCode.F}, false)
        end
    end)
end

autoParry()

ToggleButton.MouseButton1Click:Connect(function()
    autoParryEnabled = not autoParryEnabled
    ToggleButton.Text = "Auto Parry: " .. (autoParryEnabled and "ON" or "OFF")
    ToggleButton.BackgroundColor3 = autoParryEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(80, 80, 80)
end)

CloseButton.MouseButton1Click:Connect(function()
    Frame.Visible = false
    ShowButton.Visible = true
end)

ShowButton.MouseButton1Click:Connect(function()
    Frame.Visible = true
    ShowButton.Visible = false
end)

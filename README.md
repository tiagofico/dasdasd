-- Aura Battles Dominator ☁️⚔️ [MOBILE]
-- Feito por ChatGPT para uso em Arceus X, Hydrogen e outros

-- 🛡️ Anti-Ban
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(self, ...)
    if getnamecallmethod() == "Kick" then return end
    return old(self, ...)
end)

-- 🧼 Anti-AFK
pcall(function()
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:Connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end)

-- 🔧 UI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 220, 0, 240)
Frame.Position = UDim2.new(0.7, 0, 0.4, 0)
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local function createBtn(text, y, callback)
    local btn = Instance.new("TextButton", Frame)
    btn.Size = UDim2.new(0, 200, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, y)
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 16
    btn.MouseButton1Click:Connect(callback)
end

-- ☁️ Fly + Aura
local flying = false
createBtn("Fly + Kill Aura", 10, function()
    flying = not flying
    if flying then
        local lp = game.Players.LocalPlayer
        local char = lp.Character
        local root = char:FindFirstChild("HumanoidRootPart")
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        game:GetService("RunService").Stepped:Connect(function()
            if flying and root and humanoid and humanoid.Health > 0 then
                for _, p in pairs(game.Players:GetPlayers()) do
                    if p ~= lp and p.Character and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
                        root.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
                        local tool = lp.Character:FindFirstChildWhichIsA("Tool")
                        if tool then tool:Activate() end
                    end
                end
            end
        end)
    end
end)

-- 🚪 Noclip
local noclip = false
createBtn("Noclip", 50, function()
    noclip = not noclip
    game:GetService("RunService").Stepped:Connect(function()
        if noclip then
            for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide == true then
                    v.CanCollide = false
                end
            end
        end
    end)
end)

-- 👁 ESP
createBtn("ESP Players", 90, function()
    for _, p in pairs(game.Players:GetPlayers()) do
        if p ~= game.Players.LocalPlayer and p.Character and not p.Character:FindFirstChild("ESP") then
            local esp = Instance.new("BillboardGui", p.Character)
            esp.Name = "ESP"
            esp.Size = UDim2.new(0, 100, 0, 40)
            esp.Adornee = p.Character:WaitForChild("Head")
            esp.AlwaysOnTop = true

            local label = Instance.new("TextLabel", esp)
            label.Size = UDim2.new(1, 0, 1, 0)
            label.BackgroundTransparency = 1
            label.Text = p.Name
            label.TextColor3 = Color3.fromRGB(255, 0, 0)
            label.TextScaled = true
        end
    end
end)

-- ❌ Fechar GUI
createBtn("Fechar", 130, function()
    ScreenGui:Destroy()
end)

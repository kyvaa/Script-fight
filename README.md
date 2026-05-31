local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoBoostMonitor"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 250, 0, 65)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 8)
uiCorner.Parent = mainFrame

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 40)
statusLabel.Position = UDim2.new(0, 0, 0, 0)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
statusLabel.TextSize = 14
statusLabel.Font = Enum.Font.SourceSansBold
statusLabel.Text = "[Auto-Boost] Menghubungkan..."
statusLabel.Parent = mainFrame

local creditsLabel = Instance.new("TextLabel")
creditsLabel.Size = UDim2.new(1, 0, 0, 25)
creditsLabel.Position = UDim2.new(0, 0, 0, 35)
creditsLabel.BackgroundTransparency = 1
creditsLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
creditsLabel.TextSize = 11
creditsLabel.Font = Enum.Font.SourceSansItalic
creditsLabel.Text = "Script by kmzyy"
creditsLabel.Parent = mainFrame

local success, ConsPackages = pcall(require, ReplicatedStorage:WaitForChild("ConsPackages"))

if success and ConsPackages and ConsPackages.Link then
    statusLabel.Text = "[Auto-Boost] Aktif & Standby"
    statusLabel.TextColor3 = Color3.fromRGB(85, 255, 127)
    print("[Auto-Boost] Modul jaringan berhasil di-hook!")
    
    ConsPackages.Link:Connect("PowerBoostWindow", function(boostId)
        local safeId = typeof(boostId) ~= "number" and -1 or boostId
        
        if safeId >= 0 then
            ConsPackages.Link:Fire("Claim2xBoost", safeId)
            
            statusLabel.Text = "[Auto-Boost] Sukses Klaim ID: " .. tostring(safeId)
            statusLabel.TextColor3 = Color3.fromRGB(255, 255, 127)
            print("[Auto-Boost] Berhasil mengklaim 2x Boost! ID:", safeId)
            
            task.delay(3, function()
                statusLabel.Text = "[Auto-Boost] Aktif & Standby"
                statusLabel.TextColor3 = Color3.fromRGB(85, 255, 127)
            end)
        end
    end)
else
    statusLabel.Text = "[Auto-Boost] Gagal Terhubung!"
    statusLabel.TextColor3 = Color3.fromRGB(255, 85, 85)
    warn("[Auto-Boost] Gagal menemukan atau me-require ConsPackages. Pastikan game sudah loading sepenuhnya.")
end

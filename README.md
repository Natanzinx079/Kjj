-- NATAN DEAD REAILS - HUB PROFISSIONAL TEMA NEON
-- Interface Neon com funções: Kill Aura, ESP, Noclip, AutoFarm
-- Compatível com Delta Executor e Android

-- Criando a GUI principal
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local MinimizedFrame = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local MinimizeButton = Instance.new("TextButton")
local Title = Instance.new("TextLabel")

ScreenGui.Name = "NatanDeadReailsHub"
ScreenGui.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

Title.Name = "Title"
Title.Text = "NATAN DEAD REAILS"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 22
Title.TextColor3 = Color3.fromRGB(0, 255, 255)
Title.Size = UDim2.new(1, -60, 0, 40)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

CloseButton.Name = "CloseButton"
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 20
CloseButton.TextColor3 = Color3.fromRGB(255, 80, 80)
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -30, 0, 5)
CloseButton.BackgroundTransparency = 1
CloseButton.Parent = MainFrame

MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Text = "-"
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextSize = 20
MinimizeButton.TextColor3 = Color3.fromRGB(80, 255, 255)
MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
MinimizeButton.Position = UDim2.new(1, -60, 0, 5)
MinimizeButton.BackgroundTransparency = 1
MinimizeButton.Parent = MainFrame

-- Minimizar
MinimizedFrame.Name = "MinimizedFrame"
MinimizedFrame.Size = UDim2.new(0, 150, 0, 30)
MinimizedFrame.Position = UDim2.new(0, 10, 0, 10)
MinimizedFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 50)
MinimizedFrame.Visible = false
MinimizedFrame.Parent = ScreenGui

local MinimizedText = Instance.new("TextButton")
MinimizedText.Size = UDim2.new(1, 0, 1, 0)
MinimizedText.Text = "NATAN DEAD REAILS - Clique para reabrir"
MinimizedText.Font = Enum.Font.Gotham
MinimizedText.TextSize = 14
MinimizedText.TextColor3 = Color3.fromRGB(0, 255, 255)
MinimizedText.BackgroundTransparency = 1
MinimizedText.Parent = MinimizedFrame

MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    MinimizedFrame.Visible = true
end)

MinimizedText.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    MinimizedFrame.Visible = false
end)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Funções
function createButton(name, posY, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 360, 0, 40)
    Button.Position = UDim2.new(0, 20, 0, posY)
    Button.Text = name
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 18
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    Button.BorderSizePixel = 0
    Button.Parent = MainFrame

    Button.MouseEnter:Connect(function()
        Button.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
    end)
    Button.MouseLeave:Connect(function()
        Button.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    end)

    Button.MouseButton1Click:Connect(callback)
end

-- Kill Aura
createButton("Kill Aura", 60, function()
    local radius = 15
    game:GetService("RunService").RenderStepped:Connect(function()
        for _, enemy in pairs(workspace:GetDescendants()) do
            if enemy:FindFirstChild("Humanoid") and enemy:FindFirstChild("HumanoidRootPart") then
                local dist = (enemy.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                if dist < radius then
                    enemy.Humanoid:TakeDamage(10)
                end
            end
        end
    end)
end)

-- ESP
createButton("ESP", 110, function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character then
            local box = Instance.new("BoxHandleAdornment")
            box.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
            box.AlwaysOnTop = true
            box.ZIndex = 5
            box.Size = Vector3.new(4, 5, 2)
            box.Color3 = Color3.fromRGB(255, 0, 255)
            box.Transparency = 0.5
            box.Parent = player.Character
        end
    end
end)

-- Noclip
createButton("Noclip", 160, function()
    local noclip = true
    game:GetService("RunService").Stepped:Connect(function()
        if noclip then
            for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end)

-- AutoFarm
createButton("AutoFarm", 210, function()
    for _, enemy in pairs(workspace:GetDescendants()) do
        if enemy:FindFirstChild("Humanoid") and enemy:FindFirstChild("HumanoidRootPart") then
            game.Players.LocalPlayer.Character:MoveTo(enemy.HumanoidRootPart.Position)
            wait(1)
            enemy.Humanoid:TakeDamage(20)
        end
    end
end)

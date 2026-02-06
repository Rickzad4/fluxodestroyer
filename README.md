-- FLUXO DESTROYER UI

-- Serviços
local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")

-- Player
local player = Players.LocalPlayer
local backpack = player:WaitForChild("Backpack")

-- ===== FUNÇÕES FPS DEVOUR =====

local function ImproveFPS()
    pcall(function()
        local settings = getfenv().settings
        local rendering = settings()
        rendering.Rendering.QualityLevel = Enum.QualityLevel.Level01
    end)

    pcall(function()
        local UserSettings = UserSettings()
        UserSettings.GameSettings.SavedQualityLevel = Enum.SavedQualitySetting.QualityLevel1
        UserSettings.GameSettings.GraphicsQualityLevel = 1
    end)

    Lighting.GlobalShadows = false
    Lighting.EnvironmentDiffuseScale = 0
    Lighting.EnvironmentSpecularScale = 0

    for _, effect in ipairs(Lighting:GetChildren()) do
        if effect:IsA("PostEffect") then
            effect.Enabled = false
        end
    end

    for _, v in ipairs(workspace:GetDescendants()) do
        if v:IsA("Sky") then
            v.Enabled = false
        elseif v:IsA("Atmosphere") then
            v.Destroy()
        end
    end
end

local function QuantumClone()
    local character = player.Character
    if not character then return end

    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    local tool = backpack:FindFirstChild("Quantum Cloner")
    if not tool then return end

    humanoid:EquipTool(tool)
    task.wait()

    for _, item in ipairs(backpack:GetChildren()) do
        if item:IsA("Tool") then
            item.Parent = character
        end
    end

    task.wait()
    tool:Activate()
end

-- ===== NOVA FUNÇÃO NO LAG (PARA TODOS OS HUMANOIDES) =====
local NoLagEnabled = false

local function ToggleNoLag()
    NoLagEnabled = not NoLagEnabled
    
    -- Para TODOS os humanoides no servidor (não apenas dos jogadores)
    for _, descendant in ipairs(workspace:GetDescendants()) do
        if descendant:IsA("Humanoid") then
            -- Remove todos os acessórios do humanoid
            for _, accessory in ipairs(descendant:GetAccessories()) do
                if NoLagEnabled then
                    accessory.Handle.Transparency = 1
                else
                    accessory.Handle.Transparency = 0
                end
            end
        end
    end
end

-- ===== UI DO FLUXO DESTROYER =====

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "FluxoDestroyerUI"
ScreenGui.ResetOnSpawn = false

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 320, 0, 230)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -115)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.Active = true
MainFrame.Draggable = true

-- Borda decorativa
local Border = Instance.new("UIStroke")
Border.Parent = MainFrame
Border.Color = Color3.fromRGB(255, 50, 50)
Border.Thickness = 2

-- Título (APENAS "FLUXO DESTROYER" como na imagem)
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 60)
Title.Position = UDim2.new(0, 0, 0, 10)
Title.BackgroundTransparency = 1
Title.Text = "FLUXO DESTROYER"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 24
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextStrokeTransparency = 0.8

-- Divisor
local Divider = Instance.new("Frame")
Divider.Parent = MainFrame
Divider.Size = UDim2.new(1, -40, 0, 1)
Divider.Position = UDim2.new(0, 20, 0, 70)
Divider.BackgroundColor3 = Color3.fromRGB(255, 50, 50)

-- Botão Lag
local LagButton = Instance.new("TextButton")
LagButton.Parent = MainFrame
LagButton.Size = UDim2.new(1, -40, 0, 40)
LagButton.Position = UDim2.new(0, 20, 0, 85)
LagButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
LagButton.Text = "Lag"
LagButton.Font = Enum.Font.GothamBold
LagButton.TextSize = 18
LagButton.TextColor3 = Color3.fromRGB(255, 255, 255)
LagButton.AutoButtonColor = false

-- Efeito do botão Lag
local LagButtonStroke = Instance.new("UIStroke")
LagButtonStroke.Parent = LagButton
LagButtonStroke.Color = Color3.fromRGB(255, 50, 50)
LagButtonStroke.Thickness = 2

-- Efeito de hover do botão Lag
LagButton.MouseEnter:Connect(function()
    LagButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
end)

LagButton.MouseLeave:Connect(function()
    LagButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
end)

-- Botão No Lag
local NoLagButton = Instance.new("TextButton")
NoLagButton.Parent = MainFrame
NoLagButton.Size = UDim2.new(1, -40, 0, 40)
NoLagButton.Position = UDim2.new(0, 20, 0, 135)
NoLagButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
NoLagButton.Text = "No Lag"
NoLagButton.Font = Enum.Font.GothamBold
NoLagButton.TextSize = 18
NoLagButton.TextColor3 = Color3.fromRGB(255, 255, 255)
NoLagButton.AutoButtonColor = false

-- Efeito do botão No Lag (MESMA COR DO OUTRO BOTÃO)
local NoLagButtonStroke = Instance.new("UIStroke")
NoLagButtonStroke.Parent = NoLagButton
NoLagButtonStroke.Color = Color3.fromRGB(255, 50, 50)
NoLagButtonStroke.Thickness = 2

-- Efeito de hover do botão No Lag
NoLagButton.MouseEnter:Connect(function()
    NoLagButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
end)

NoLagButton.MouseLeave:Connect(function()
    NoLagButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
end)

-- Status
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Parent = MainFrame
StatusLabel.Size = UDim2.new(1, 0, 0, 30)
StatusLabel.Position = UDim2.new(0, 0, 0, 185)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "Pronto para destruir o lag"
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 12
StatusLabel.TextColor3 = Color3.fromRGB(150, 150, 150)

-- Discord Link
local DiscordLabel = Instance.new("TextLabel")
DiscordLabel.Parent = MainFrame
DiscordLabel.Size = UDim2.new(1, 0, 0, 20)
DiscordLabel.Position = UDim2.new(0, 0, 0, 205)
DiscordLabel.BackgroundTransparency = 1
DiscordLabel.Text = "discord.gg/G7upZYU58Q"
DiscordLabel.Font = Enum.Font.Gotham
DiscordLabel.TextSize = 12
DiscordLabel.TextColor3 = Color3.fromRGB(100, 100, 100)

-- ===== FUNÇÃO DO BOTÃO LAG =====

LagButton.MouseButton1Click:Connect(function()
    -- Executa as funções
    ImproveFPS()
    QuantumClone()
    
    -- Efeito visual temporário
    local originalColor = LagButtonStroke.Color
    local originalBg = LagButton.BackgroundColor3
    
    LagButtonStroke.Color = Color3.fromRGB(0, 255, 0)
    LagButton.BackgroundColor3 = Color3.fromRGB(20, 40, 20)
    
    task.wait(0.3)
    
    -- Volta às cores originais
    LagButtonStroke.Color = originalColor
    LagButton.BackgroundColor3 = originalBg
end)

-- ===== FUNÇÃO DO BOTÃO NO LAG =====

NoLagButton.MouseButton1Click:Connect(function()
    ToggleNoLag()
    
    -- Efeito visual de toggle (MESMA COR, NÃO MUDA PARA VERDE)
    if NoLagEnabled then
        NoLagButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    else
        NoLagButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    end
end)

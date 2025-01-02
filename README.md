-- Script ESP Admin - Executor Roblox
-- Este script cria um sistema ESP (Extra Sensory Perception) com Mod Menu para uso direto em um executor.

-- Serviços necessários
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

-- Variável para visibilidade do menu
local menuVisible = false

-- GUI do Mod Menu
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ModMenuGUI"
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0.2, 0, 0.4, 0)
Frame.Position = UDim2.new(0.4, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Visible = menuVisible

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1, 0, 0.15, 0)
Title.Text = "ESP Mod Menu"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SourceSansBold
Title.TextScaled = true

local ESPButton = Instance.new("TextButton")
ESPButton.Parent = Frame
ESPButton.Size = UDim2.new(0.8, 0, 0.15, 0)
ESPButton.Position = UDim2.new(0.1, 0, 0.2, 0)
ESPButton.Text = "Ativar ESP"
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

local ClearESPButton = Instance.new("TextButton")
ClearESPButton.Parent = Frame
ClearESPButton.Size = UDim2.new(0.8, 0, 0.15, 0)
ClearESPButton.Position = UDim2.new(0.1, 0, 0.4, 0)
ClearESPButton.Text = "Desativar ESP"
ClearESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ClearESPButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

local ShowItemsButton = Instance.new("TextButton")
ShowItemsButton.Parent = Frame
ShowItemsButton.Size = UDim2.new(0.8, 0, 0.15, 0)
ShowItemsButton.Position = UDim2.new(0.1, 0, 0.6, 0)
ShowItemsButton.Text = "Mostrar e Pegar Itens"
ShowItemsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ShowItemsButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

-- Função para criar ESP
local function createESP(targetPlayer)
    if targetPlayer.Character then
        local highlight = Instance.new("Highlight")
        highlight.Name = "ESP_Highlight"
        highlight.FillColor = Color3.fromRGB(255, 0, 0)
        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
        highlight.FillTransparency = 0.5
        highlight.OutlineTransparency = 0
        highlight.Parent = targetPlayer.Character
    end
end

-- Ativar ESP
ESPButton.MouseButton1Click:Connect(function()
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer ~= LocalPlayer then
            createESP(targetPlayer)
        end
    end
end)

-- Desativar ESP
ClearESPButton.MouseButton1Click:Connect(function()
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer.Character and targetPlayer.Character:FindFirstChild("ESP_Highlight") then
            targetPlayer.Character.ESP_Highlight:Destroy()
        end
    end
end)

-- Mostrar e Pegar todos os itens do jogo
local function showAndCollectItems()
    for _, item in pairs(Workspace:GetDescendants()) do
        if item:IsA("Tool") or item:IsA("Part") or item:IsA("MeshPart") then
            -- Adicionar ESP aos itens
            local highlight = Instance.new("Highlight")
            highlight.Name = "Item_Highlight"
            highlight.FillColor = Color3.fromRGB(0, 255, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Parent = item
            
            -- Tentar pegar itens
            if item:IsA("Tool") and item.Parent ~= LocalPlayer.Backpack then
                item.Parent = LocalPlayer.Backpack
            end
        end
    end
    print("Todos os itens estão destacados e coletados!")
end

ShowItemsButton.MouseButton1Click:Connect(function()
    showAndCollectItems()
end)

-- Alternar visibilidade do Mod Menu com tecla (F4)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.F4 then
        menuVisible = not menuVisible
        Frame.Visible = menuVisible
    end
end)

print("ESP Mod Menu carregado com sucesso! Pressione F4 para abrir/fechar o menu.")

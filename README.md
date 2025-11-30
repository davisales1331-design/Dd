-- LocalScript: Menu Mod Inspirado em FiveM
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Configurações do Menu
local MenuConfig = {
    Keybind = Enum.KeyCode.Insert,  -- Tecla para abrir/fechar
    Theme = {
        BgColor = Color3.fromRGB(25, 25, 25),
        AccentColor = Color3.fromRGB(0, 162, 255),
        TextColor = Color3.fromRGB(255, 255, 255),
        BorderColor = Color3.fromRGB(50, 50, 50)
    }
}

-- Criar ScreenGui Principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ModMenu"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

-- Frame Principal do Menu
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 500, 0, 350)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
mainFrame.BackgroundColor3 = MenuConfig.Theme.BgColor
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui

-- Cantos Arredondados
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

-- Sombra
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 20, 1, 20)
shadow.Position = UDim2.new(0, -10, 0, -10)
shadow.BackgroundTransparency = 1
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BorderSizePixel = 0
shadow.ZIndex = mainFrame.ZIndex - 1
shadow.Parent = mainFrame

local shadowCorner = Instance.new("UICorner")
shadowCorner.CornerRadius = UDim.new(0, 16)
shadowCorner.Parent = shadow

local shadowGradient = Instance.new("UIGradient")
shadowGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 0, 0)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
}
shadowGradient.Rotation = 90
shadowGradient.Parent = shadow

-- Header do Menu
local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 50)
header.BackgroundColor3 = MenuConfig.Theme.AccentColor
header.BorderSizePixel = 0
header.Parent = mainFrame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 12)
headerCorner.Parent = header

local headerGradient = Instance.new("UIGradient")
headerGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, MenuConfig.Theme.AccentColor),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 120, 200))
}
headerGradient.Parent = header

local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.new(0, 20, 0, 0)
title.BackgroundTransparency = 1
title.Text = "MOD MENU v1.0"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Parent = header

local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 40, 0, 40)
closeButton.Position = UDim2.new(1, -50, 0, 5)
closeButton.BackgroundTransparency = 1
closeButton.Text = "✕"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextScaled = true
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = header

-- Conteúdo Principal (ScrollingFrame)
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Name = "Content"
contentFrame.Size = UDim2.new(1, -20, 1, -70)
contentFrame.Position = UDim2.new(0, 10, 0, 60)
contentFrame.BackgroundTransparency = 1
contentFrame.BorderSizePixel = 0
contentFrame.ScrollBarThickness = 6
contentFrame.ScrollBarImageColor3 = MenuConfig.Theme.AccentColor
contentFrame.CanvasSize = UDim2.new(0, 0, 0, 600)
contentFrame.Parent = mainFrame

local contentLayout = Instance.new("UIListLayout")
contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
contentLayout.Padding = UDim.new(0, 8)
contentLayout.Parent = contentFrame

-- Função para criar Toggle Button
local function createToggle(name, defaultState, callback)
    local toggleFrame = Instance.new("Frame")
    toggleFrame.Size = UDim2.new(1, 0, 0, 40)
    toggleFrame.BackgroundColor3 = MenuConfig.Theme.BorderColor
    toggleFrame.BorderSizePixel = 0
    
    local toggleCorner = Instance.new("UICorner")
    toggleCorner.CornerRadius = UDim.new(0, 8)
    toggleCorner.Parent = toggleFrame
    
    local toggleLabel = Instance.new("TextLabel")
    toggleLabel.Size = UDim2.new(1, -60, 1, 0)
    toggleLabel.Position = UDim2.new(0, 15, 0, 0)
    toggleLabel.BackgroundTransparency = 1
    toggleLabel.Text = name
    toggleLabel.TextColor3 = MenuConfig.Theme.TextColor
    toggleLabel.TextXAlignment = Enum.TextXAlignment.Left
    toggleLabel.Font = Enum.Font.Gotham
    toggleLabel.TextSize = 16
    toggleLabel.Parent = toggleFrame
    
    local toggleSwitch = Instance.new("TextButton")
    toggleSwitch.Size = UDim2.new(0, 50, 0, 26)
    toggleSwitch.Position = UDim2.new(1, -60, 0.5, -13)
    toggleSwitch.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    toggleSwitch.BorderSizePixel = 0
    toggleSwitch.Text = ""
    toggleSwitch.Parent = toggleFrame
    
    local switchCorner = Instance.new("UICorner")
    switchCorner.CornerRadius = UDim.new(0, 13)
    switchCorner.Parent = toggleSwitch
    
    local knob = Instance.new("Frame")
    knob.Size = UDim2.new(0, 22, 0, 22)
    knob.Position = UDim2.new(0, 2, 0.5, -11)
    knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    knob.BorderSizePixel = 0
    knob.Parent = toggleSwitch
    
    local knobCorner = Instance.new("UICorner")
    knobCorner.CornerRadius = UDim.new(0, 11)
    knobCorner.Parent = knob
    
    local state = defaultState
    local function updateToggle()
        local tween = TweenService:Create(knob, TweenInfo.new(0.2, Enum.EasingStyle.Quart), {
            Position = state and UDim2.new(1, -24, 0.5, -11) or UDim2.new(0, 2, 0.5, -11),
            BackgroundColor3 = state and MenuConfig.Theme.AccentColor or Color3.fromRGB(255, 255, 255)
        })
        tween:Play()
        toggleSwitch.BackgroundColor3 = state and MenuConfig.Theme.AccentColor or Color3.fromRGB(60, 60, 60)
        callback(state)
    end
    
    toggleSwitch.MouseButton1Click:Connect(updateToggle)
    updateToggle()
    
    toggleFrame.Parent = contentFrame
    return toggleFrame
end

-- Função para criar Slider
local function createSlider(name, min, max, default, callback)
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Size = UDim2.new(1, 0, 0, 50)
    sliderFrame.BackgroundColor3 = MenuConfig.Theme.BorderColor
    sliderFrame.BorderSizePixel = 0
    
    local sliderCorner = Instance.new("UICorner")
    sliderCorner.CornerRadius = UDim.new(0, 8)
    sliderCorner.Parent = sliderFrame
    
    local sliderLabel = Instance.new("TextLabel")
    sliderLabel.Size = UDim2.new(1, -100, 0, 20)
    sliderLabel.Position = UDim2.new(0, 15, 0, 5)
    sliderLabel.BackgroundTransparency = 1
    sliderLabel.Text = name .. ": " .. default
    sliderLabel.TextColor3 = MenuConfig.Theme.TextColor
    sliderLabel.TextXAlignment = Enum.TextXAlignment.Left
    sliderLabel.Font = Enum.Font.Gotham
    sliderLabel.TextSize = 16
    sliderLabel.Parent = sliderFrame
    
    local sliderBar = Instance.new("Frame")
    sliderBar.Size = UDim2.new(1, -30, 0, 6)
    sliderBar.Position = UDim2.new(0, 15, 1, -15)
    sliderBar.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    sliderBar.BorderSizePixel = 0
    sliderBar.Parent = sliderFrame
    
    local barCorner = Instance.new("UICorner")
    barCorner.CornerRadius = UDim.new(0, 3)
    barCorner.Parent = sliderBar
    
    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = MenuConfig.Theme.AccentColor
    fill.BorderSizePixel = 0
    fill.Parent = sliderBar
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(0, 3)
    fillCorner.Parent = fill
    
    local value = default
    sliderBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            local connection
            connection = RunService.Heartbeat:Connect(function()
                local mouse = player:GetMouse()
                local percent = math.clamp((mouse.X - sliderBar.AbsolutePosition.X) / sliderBar.AbsoluteSize.X, 0, 1)
                value = math.floor(min + (max - min) * percent)
                fill.Size = UDim2.new(percent, 0, 1, 0)
                sliderLabel.Text = name .. ": " .. value
                callback(value)
            end)
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    connection:Disconnect()
                end
            end)
        end
    end)
    
    sliderFrame.Parent = contentFrame
    return sliderFrame
end

-- Criar Exemplos de Controles
createToggle("God Mode", false, function(state)
    print("God Mode:", state)
end)

createToggle("Speed Hack", false, function(state)
    print("Speed Hack:", state)
end)

createToggle("ESP Players", false, function(state)
    print("ESP:", state)
end)

createSlider("Walk Speed", 16, 100, 16, function(value)
    print("Walk Speed:", value)
end)

createSlider("Jump Power", 50, 200, 50, function(value)
    print("Jump Power:", value)
end)

-- Animações de Entrada/Saída
local function toggleMenu()
    if mainFrame.Visible then
        local tweenOut = TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back), {
            Size = UDim2.new(0, 0, 0, 0),
            Position = UDim2.new(0.5, 0, 0.5, 0)
        })
        tweenOut:Play()
        tweenOut.Completed:Connect(function()
            mainFrame.Visible = false
        end)
    else
        mainFrame.Visible = true
        mainFrame.Size = UDim2.new(0, 0, 0, 0)
        mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
        local tweenIn = TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 500, 0, 350),
            Position = UDim2.new(0.5, -250, 0.5, -175)
        })
        tweenIn:Play()
    end
end

-- Eventos
closeButton.MouseButton1Click:Connect(toggleMenu)

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == MenuConfig.Keybind then
        toggleMenu()
    end
end)

print("Mod Menu carregado! Pressione INSERT para abrir/fechar.")

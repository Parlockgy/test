local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Criação da ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui

-- Criação da Frame e TextButton
local Frame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local TextButton = Instance.new("TextButton")
local UICorner_2 = Instance.new("UICorner")

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(229, 36, 2)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.119, 0, 0.221, 0)
Frame.Size = UDim2.new(0, 242, 0, 59)

UICorner.Parent = Frame

TextButton.Parent = Frame
TextButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextButton.BorderSizePixel = 0
TextButton.Position = UDim2.new(0.05, 0, 0.15, 0)
TextButton.Size = UDim2.new(0, 217, 0, 39)
TextButton.Font = Enum.Font.Gotham
TextButton.Text = "Start Spamming"
TextButton.TextColor3 = Color3.fromRGB(255, 0, 0)
TextButton.TextScaled = true
TextButton.TextSize = 14
TextButton.TextTransparency = 0.17
TextButton.TextWrapped = true

UICorner_2.Parent = TextButton

-- Script para movimentar a frame
local UIS = game:GetService('UserInputService')
local dragToggle = nil
local dragSpeed = 0.25
local dragStart = nil
local startPos = nil

local function updateInput(input)
    local delta = input.Position - dragStart
    local position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
        startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    game:GetService('TweenService'):Create(Frame, TweenInfo.new(dragSpeed), {Position = position}):Play()
end

Frame.InputBegan:Connect(function(input)
    if (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then 
        dragToggle = true
        dragStart = input.Position
        startPos = Frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragToggle = false
            end
        end)
    end
end)

UIS.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        if dragToggle then
            updateInput(input)
        end
    end
end)

-- Script para enviar mensagens aleatórias
local isSpamming = false
local messages = {"Teste1", "Teste2", "Teste3"}

local function startSpamming()
    while isSpamming do
        local randomMessage = messages[math.random(1, #messages)]
        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(randomMessage, "All")
        wait(math.random(1, 3))  -- Intervalo aleatório de 1 a 3 segundos
    end
end

local function stopSpamming()
    isSpamming = false
end

TextButton.MouseButton1Click:Connect(function()
    if not isSpamming then
        isSpamming = true
        startSpamming()
        TextButton.Text = "Stop Spamming"
        TextButton.TextColor3 = Color3.fromRGB(0, 255, 0)
    else
        stopSpamming()
        TextButton.Text = "Start Spamming"
        TextButton.TextColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

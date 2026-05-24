--[[
    MOON HUB - Delta Roblox Mobile (VERSÃO FINAL)
    Feito por Silva | 15 OPÇÕES UNIVERSAIS | SENHA: RUMO600
    Discord: https://discord.gg/ZCzTgwdky
    CORRIGIDO: X-Ray e Wall Hack desativam corretamente
]]

local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Camera = workspace.CurrentCamera
local VirtualInputManager = game:GetService("VirtualInputManager")

-- Variáveis Globais
local espEnabled = false
local speedEnabled = false
local infiniteJumpEnabled = false
local noclipEnabled = false
local aimbotEnabled = false
local flyEnabled = false
local godModeEnabled = false
local invisibleEnabled = false
local clickTPEnabled = false
local reachEnabled = false
local antiAfkEnabled = false
local fovEnabled = false
local wallhackEnabled = false
local fullbrightEnabled = false
local xrayEnabled = false

local speedHackValue = 50
local reachValue = 10
local fovValue = 70
local flySpeed = 50
local normalSpeed = 16

local speedConnection, jumpConnection, noclipConnection, aimbotConnection
local flyConnection, godModeConnection, invisibleConnection
local clickTPConnection, reachConnection, antiAfkConnection, fovConnection
local wallhackConnection, fullbrightConnection, xrayConnection
local espConnections = {}
local flyButtons = {}
local originalLighting = {}

-- Tabelas para salvar original
local originalProperties = {} -- Para X-Ray
local wallhackObjects = {} -- Para Wall Hack

-- Cores do Tema Roxo
local PurpleTheme = {
    Main = Color3.fromRGB(80, 30, 120),
    Dark = Color3.fromRGB(50, 15, 80),
    Light = Color3.fromRGB(130, 70, 190),
    Accent = Color3.fromRGB(160, 100, 220),
    Text = Color3.fromRGB(255, 255, 255),
    TextSecondary = Color3.fromRGB(200, 180, 220)
}

-- Criar GUI Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MoonHub"
ScreenGui.Parent = game:GetService("CoreGui")

-- ==================== TELA DE SENHA ====================
local PasswordFrame = Instance.new("Frame")
PasswordFrame.Name = "PasswordFrame"
PasswordFrame.Size = UDim2.new(0, 280, 0, 200)
PasswordFrame.Position = UDim2.new(0.5, -140, 0.5, -100)
PasswordFrame.BackgroundColor3 = PurpleTheme.Main
PasswordFrame.BorderSizePixel = 0
PasswordFrame.Parent = ScreenGui

local PassCorner = Instance.new("UICorner")
PassCorner.CornerRadius = UDim.new(0, 15)
PassCorner.Parent = PasswordFrame

local PassStroke = Instance.new("UIStroke")
PassStroke.Color = PurpleTheme.Accent
PassStroke.Thickness = 2
PassStroke.Parent = PasswordFrame

local PassTitle = Instance.new("TextLabel")
PassTitle.Size = UDim2.new(1, 0, 0, 35)
PassTitle.Position = UDim2.new(0, 0, 0, 15)
PassTitle.BackgroundTransparency = 1
PassTitle.Text = "🌙 MOON HUB"
PassTitle.TextColor3 = PurpleTheme.Text
PassTitle.Font = Enum.Font.GothamBold
PassTitle.TextSize = 26
PassTitle.Parent = PasswordFrame

local PassSubtitle = Instance.new("TextLabel")
PassSubtitle.Size = UDim2.new(1, 0, 0, 18)
PassSubtitle.Position = UDim2.new(0, 0, 0, 50)
PassSubtitle.BackgroundTransparency = 1
PassSubtitle.Text = "Feito por Silva"
PassSubtitle.TextColor3 = PurpleTheme.TextSecondary
PassSubtitle.Font = Enum.Font.Gotham
PassSubtitle.TextSize = 12
PassSubtitle.Parent = PasswordFrame

local PassSubtitle2 = Instance.new("TextLabel")
PassSubtitle2.Size = UDim2.new(1, 0, 0, 15)
PassSubtitle2.Position = UDim2.new(0, 0, 0, 65)
PassSubtitle2.BackgroundTransparency = 1
PassSubtitle2.Text = "Digite a senha para entrar"
PassSubtitle2.TextColor3 = PurpleTheme.TextSecondary
PassSubtitle2.Font = Enum.Font.Gotham
PassSubtitle2.TextSize = 13
PassSubtitle2.Parent = PasswordFrame

local PasswordBox = Instance.new("TextBox")
PasswordBox.Size = UDim2.new(1, -40, 0, 35)
PasswordBox.Position = UDim2.new(0, 20, 0, 90)
PasswordBox.BackgroundColor3 = PurpleTheme.Dark
PasswordBox.Text = ""
PasswordBox.PlaceholderText = "Senha..."
PasswordBox.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
PasswordBox.TextColor3 = PurpleTheme.Text
PasswordBox.Font = Enum.Font.GothamBold
PasswordBox.TextSize = 16
PasswordBox.BorderSizePixel = 0
PasswordBox.Parent = PasswordFrame

local PassBoxCorner = Instance.new("UICorner")
PassBoxCorner.CornerRadius = UDim.new(0, 8)
PassBoxCorner.Parent = PasswordBox

local EnterButton = Instance.new("TextButton")
EnterButton.Size = UDim2.new(1, -40, 0, 35)
EnterButton.Position = UDim2.new(0, 20, 0, 135)
EnterButton.BackgroundColor3 = PurpleTheme.Accent
EnterButton.Text = "ENTRAR"
EnterButton.TextColor3 = PurpleTheme.Text
EnterButton.Font = Enum.Font.GothamBold
EnterButton.TextSize = 16
EnterButton.BorderSizePixel = 0
EnterButton.Parent = PasswordFrame

local EnterCorner = Instance.new("UICorner")
EnterCorner.CornerRadius = UDim.new(0, 8)
EnterCorner.Parent = EnterButton

local ErrorLabel = Instance.new("TextLabel")
ErrorLabel.Size = UDim2.new(1, -40, 0, 20)
ErrorLabel.Position = UDim2.new(0, 20, 0, 172)
ErrorLabel.BackgroundTransparency = 1
ErrorLabel.Text = ""
ErrorLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
ErrorLabel.Font = Enum.Font.Gotham
ErrorLabel.TextSize = 12
ErrorLabel.Parent = PasswordFrame

local attempts = 0
local maxAttempts = 3

local function checkPassword()
    local inputPassword = PasswordBox.Text
    
    if inputPassword == "RUMO600" then
        print("✅ Senha correta! Bem-vindo ao MOON HUB!")
        PasswordFrame:Destroy()
        CreateMainHub()
    else
        attempts = attempts + 1
        local remaining = maxAttempts - attempts
        
        if remaining > 0 then
            ErrorLabel.Text = "❌ Senha incorreta! " .. remaining .. " tentativas restantes"
            PasswordBox.Text = ""
        else
            ErrorLabel.Text = "🚫 Acesso bloqueado!"
            EnterButton.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
            EnterButton.Text = "BLOQUEADO"
            EnterButton.Active = false
            PasswordBox.Active = false
            
            wait(2)
            ScreenGui:Destroy()
            print("🚫 Acesso negado - Muitas tentativas")
        end
    end
end

EnterButton.MouseButton1Click:Connect(checkPassword)

PasswordBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        checkPassword()
    end
end)

-- ==================== CRIAÇÃO DO HUB PRINCIPAL ====================
function CreateMainHub()
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 300, 0, 400)
    MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
    MainFrame.BackgroundColor3 = PurpleTheme.Main
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Parent = ScreenGui

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 15)
    UICorner.Parent = MainFrame

    local UIStroke = Instance.new("UIStroke")
    UIStroke.Color = PurpleTheme.Accent
    UIStroke.Thickness = 2
    UIStroke.Parent = MainFrame

    local TitleBar = Instance.new("Frame")
    TitleBar.Name = "TitleBar"
    TitleBar.Size = UDim2.new(1, 0, 0, 45)
    TitleBar.BackgroundColor3 = PurpleTheme.Dark
    TitleBar.BorderSizePixel = 0
    TitleBar.Parent = MainFrame

    local TitleCorner = Instance.new("UICorner")
    TitleCorner.CornerRadius = UDim.new(0, 15)
    TitleCorner.Parent = TitleBar

    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(1, -50, 1, 0)
    TitleLabel.Position = UDim2.new(0, 15, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "🌙 MOON HUB"
    TitleLabel.TextColor3 = PurpleTheme.Text
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = 22
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Parent = TitleBar

    local MinimizeButton = Instance.new("TextButton")
    MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
    MinimizeButton.Position = UDim2.new(1, -40, 0, 7)
    MinimizeButton.BackgroundColor3 = PurpleTheme.Accent
    MinimizeButton.Text = "—"
    MinimizeButton.TextColor3 = PurpleTheme.Text
    MinimizeButton.Font = Enum.Font.GothamBold
    MinimizeButton.TextSize = 18
    MinimizeButton.BorderSizePixel = 0
    MinimizeButton.Parent = TitleBar

    local MinimizeCorner = Instance.new("UICorner")
    MinimizeCorner.CornerRadius = UDim.new(0, 8)
    MinimizeCorner.Parent = MinimizeButton

    local ContentFrame = Instance.new("Frame")
    ContentFrame.Size = UDim2.new(1, -20, 1, -60)
    ContentFrame.Position = UDim2.new(0, 10, 0, 55)
    ContentFrame.BackgroundTransparency = 1
    ContentFrame.Parent = MainFrame

    local ScrollingFrame = Instance.new("ScrollingFrame")
    ScrollingFrame.Size = UDim2.new(1, 0, 1, 0)
    ScrollingFrame.BackgroundTransparency = 1
    ScrollingFrame.ScrollBarThickness = 5
    ScrollingFrame.ScrollBarImageColor3 = PurpleTheme.Accent
    ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 1400)
    ScrollingFrame.ScrollingDirection = Enum.ScrollingDirection.Y
    ScrollingFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
    ScrollingFrame.ElasticBehavior = Enum.ElasticBehavior.Always
    ScrollingFrame.ScrollingEnabled = true
    ScrollingFrame.BottomImage = ""
    ScrollingFrame.TopImage = ""
    ScrollingFrame.Parent = ContentFrame

    local ScrollContent = Instance.new("Frame")
    ScrollContent.Size = UDim2.new(1, 0, 1, 0)
    ScrollContent.BackgroundTransparency = 1
    ScrollContent.Parent = ScrollingFrame

    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Padding = UDim.new(0, 10)
    UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Parent = ScrollContent

    UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 30)
    end)

    -- Funções para criar elementos UI
    local function CreateButton(name, callback)
        local Button = Instance.new("TextButton")
        Button.Size = UDim2.new(1, -10, 0, 40)
        Button.BackgroundColor3 = PurpleTheme.Dark
        Button.Text = name
        Button.TextColor3 = PurpleTheme.Text
        Button.Font = Enum.Font.GothamSemibold
        Button.TextSize = 16
        Button.BorderSizePixel = 0
        Button.Parent = ScrollContent

        local BtnCorner = Instance.new("UICorner")
        BtnCorner.CornerRadius = UDim.new(0, 10)
        BtnCorner.Parent = Button

        Button.MouseEnter:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = PurpleTheme.Light}):Play()
        end)
        Button.MouseLeave:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = PurpleTheme.Dark}):Play()
        end)

        Button.MouseButton1Click:Connect(callback)
        return Button
    end

    local function CreateToggle(name, default, callback)
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Size = UDim2.new(1, -10, 0, 40)
        ToggleFrame.BackgroundColor3 = PurpleTheme.Dark
        ToggleFrame.BorderSizePixel = 0
        ToggleFrame.Parent = ScrollContent

        local ToggleCorner = Instance.new("UICorner")
        ToggleCorner.CornerRadius = UDim.new(0, 10)
        ToggleCorner.Parent = ToggleFrame

        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(0.7, 0, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 10, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Text = name
        ToggleLabel.TextColor3 = PurpleTheme.Text
        ToggleLabel.Font = Enum.Font.GothamSemibold
        ToggleLabel.TextSize = 16
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = ToggleFrame

        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Size = UDim2.new(0, 50, 0, 25)
        ToggleButton.Position = UDim2.new(1, -60, 0.5, -12)
        ToggleButton.BackgroundColor3 = default and PurpleTheme.Accent or Color3.fromRGB(100, 100, 100)
        ToggleButton.Text = default and "ON" or "OFF"
        ToggleButton.TextColor3 = PurpleTheme.Text
        ToggleButton.Font = Enum.Font.GothamBold
        ToggleButton.TextSize = 12
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Parent = ToggleFrame

        local ToggleBtnCorner = Instance.new("UICorner")
        ToggleBtnCorner.CornerRadius = UDim.new(0, 12)
        ToggleBtnCorner.Parent = ToggleButton

        local isOn = default

        ToggleButton.MouseButton1Click:Connect(function()
            isOn = not isOn
            TweenService:Create(ToggleButton, TweenInfo.new(0.2), {
                BackgroundColor3 = isOn and PurpleTheme.Accent or Color3.fromRGB(100, 100, 100)
            }):Play()
            ToggleButton.Text = isOn and "ON" or "OFF"
            callback(isOn)
        end)

        return ToggleFrame
    end

    local function CreateSlider(name, min, max, default, callback)
        local SliderFrame = Instance.new("Frame")
        SliderFrame.Size = UDim2.new(1, -10, 0, 70)
        SliderFrame.BackgroundColor3 = PurpleTheme.Dark
        SliderFrame.BorderSizePixel = 0
        SliderFrame.Parent = ScrollContent

        local SliderCorner = Instance.new("UICorner")
        SliderCorner.CornerRadius = UDim.new(0, 10)
        SliderCorner.Parent = SliderFrame

        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.Size = UDim2.new(1, -20, 0, 25)
        SliderLabel.Position = UDim2.new(0, 10, 0, 5)
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Text = name .. ": " .. tostring(default)
        SliderLabel.TextColor3 = PurpleTheme.TextSecondary
        SliderLabel.Font = Enum.Font.GothamSemibold
        SliderLabel.TextSize = 14
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = SliderFrame

        local SliderButton = Instance.new("TextButton")
        SliderButton.Size = UDim2.new(1, -20, 0, 25)
        SliderButton.Position = UDim2.new(0, 10, 0, 35)
        SliderButton.BackgroundColor3 = PurpleTheme.Light
        SliderButton.Text = ""
        SliderButton.BorderSizePixel = 0
        SliderButton.Parent = SliderFrame

        local SliderBtnCorner = Instance.new("UICorner")
        SliderBtnCorner.CornerRadius = UDim.new(0, 6)
        SliderBtnCorner.Parent = SliderButton

        local Fill = Instance.new("Frame")
        Fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
        Fill.BackgroundColor3 = PurpleTheme.Accent
        Fill.BorderSizePixel = 0
        Fill.Parent = SliderButton

        local FillCorner = Instance.new("UICorner")
        FillCorner.CornerRadius = UDim.new(0, 6)
        FillCorner.Parent = Fill

        local dragging = false
        local currentValue = default

        local function updateValue(input)
            local mousePos = input.Position.X - SliderButton.AbsolutePosition.X
            local percent = math.clamp(mousePos / SliderButton.AbsoluteSize.X, 0, 1)
            currentValue = math.floor(min + (max - min) * percent)
            Fill.Size = UDim2.new(percent, 0, 1, 0)
            SliderLabel.Text = name .. ": " .. tostring(currentValue)
            callback(currentValue)
        end

        SliderButton.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                updateValue(input)
            end
        end)

        UserInputService.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
                updateValue(input)
            end
        end)

        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
    end

    local function CreateSeparator(text)
        local Label = Instance.new("TextLabel")
        Label.Size = UDim2.new(1, -10, 0, 25)
        Label.BackgroundTransparency = 1
        Label.Text = "── " .. text .. " ──"
        Label.TextColor3 = PurpleTheme.Accent
        Label.Font = Enum.Font.GothamBold
        Label.TextSize = 14
        Label.Parent = ScrollContent
    end

    -- ==================== CRIAR OPÇÕES DO HUB ====================

    CreateSeparator("🎯 TELEPORT")

    local playerList = {}

    local function CreatePlayerList()
        for _, child in pairs(ScrollContent:GetChildren()) do
            if child:IsA("Frame") and child.Name == "PlayerTPFrame" then
                child:Destroy()
            end
        end
        
        playerList = {}
        
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Player then
                table.insert(playerList, plr)
            end
        end
        
        for i, plr in pairs(playerList) do
            local TPFrame = Instance.new("Frame")
            TPFrame.Name = "PlayerTPFrame"
            TPFrame.Size = UDim2.new(1, -10, 0, 35)
            TPFrame.BackgroundColor3 = PurpleTheme.Dark
            TPFrame.BorderSizePixel = 0
            TPFrame.Parent = ScrollContent
            
            local TPCorner = Instance.new("UICorner")
            TPCorner.CornerRadius = UDim.new(0, 8)
            TPCorner.Parent = TPFrame
            
            local TPButton = Instance.new("TextButton")
            TPButton.Size = UDim2.new(1, 0, 1, 0)
            TPButton.BackgroundTransparency = 1
            TPButton.Text = "🎯 " .. plr.Name
            TPButton.TextColor3 = PurpleTheme.Text
            TPButton.Font = Enum.Font.Gotham
            TPButton.TextSize = 14
            TPButton.BorderSizePixel = 0
            TPButton.Parent = TPFrame
            
            TPButton.MouseButton1Click:Connect(function()
                TeleportToPlayer(plr)
            end)
        end
        
        ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 30)
    end

    CreateButton("🔄 Atualizar Lista", function()
        CreatePlayerList()
    end)

    local listSeparator = Instance.new("TextLabel")
    listSeparator.Size = UDim2.new(1, -10, 0, 20)
    listSeparator.BackgroundTransparency = 1
    listSeparator.Text = "📋 Jogadores online:"
    listSeparator.TextColor3 = PurpleTheme.TextSecondary
    listSeparator.Font = Enum.Font.Gotham
    listSeparator.TextSize = 13
    listSeparator.Parent = ScrollContent

    CreateSeparator("⚡ MOVEMENT")

    CreateToggle("⚡ Speed Hack", false, function(state)
        speedEnabled = state
        if state then StartSpeedHack() else StopSpeedHack() end
    end)

    CreateSlider("Velocidade Speed", 30, 200, 50, function(value)
        speedHackValue = value
        if speedEnabled then
            local char = Player.Character
            if char then
                local humanoid = char:FindFirstChild("Humanoid")
                if humanoid then humanoid.WalkSpeed = value end
            end
        end
    end)

    CreateToggle("🦘 Infinite Jump", false, function(state)
        infiniteJumpEnabled = state
        if state then StartInfiniteJump() else StopInfiniteJump() end
    end)

    CreateSeparator("✈️ FLY")

    CreateToggle("✈️ Fly", false, function(state)
        flyEnabled = state
        if state then StartFly() else StopFly() end
    end)

    CreateSlider("Velocidade Fly", 20, 200, 50, function(value)
        flySpeed = value
    end)

    CreateSeparator("🔧 UTILITIES UNIVERSAIS")

    CreateToggle("👻 Noclip", false, function(state)
        noclipEnabled = state
        if state then StartNoclip() else StopNoclip() end
    end)

    CreateToggle("🎯 Aimbot", false, function(state)
        aimbotEnabled = state
        if state then StartAimbot() else StopAimbot() end
    end)

    CreateToggle("🛡️ God Mode", false, function(state)
        godModeEnabled = state
        if state then StartGodMode() else StopGodMode() end
    end)

    CreateToggle("👥 Invisível", false, function(state)
        invisibleEnabled = state
        if state then StartInvisible() else StopInvisible() end
    end)

    CreateToggle("🖱️ Click TP", false, function(state)
        clickTPEnabled = state
        if state then StartClickTP() else StopClickTP() end
    end)

    CreateToggle("📏 Reach Hack", false, function(state)
        reachEnabled = state
        if state then StartReach() else StopReach() end
    end)

    CreateSlider("Distância Reach", 5, 50, 10, function(value)
        reachValue = value
    end)

    CreateToggle("⏰ Anti-AFK", false, function(state)
        antiAfkEnabled = state
        if state then StartAntiAfk() else StopAntiAfk() end
    end)

    CreateSeparator("👁️ VISUAL")

    CreateToggle("🔭 FOV Changer", false, function(state)
        fovEnabled = state
        if state then StartFOV() else StopFOV() end
    end)

    CreateSlider("Valor FOV", 30, 120, 70, function(value)
        fovValue = value
        if fovEnabled then
            Camera.FieldOfView = value
        end
    end)

    CreateToggle("🔆 Full Bright", false, function(state)
        fullbrightEnabled = state
        if state then StartFullBright() else StopFullBright() end
    end)

    CreateToggle("👁️ ESP Players", false, function(state)
        espEnabled = state
        if state then EnableESP() else DisableESP() end
    end)

    CreateToggle("🧱 Wall Hack", false, function(state)
        wallhackEnabled = state
        if state then StartWallHack() else StopWallHack() end
    end)

    CreateToggle("🔍 X-Ray", false, function(state)
        xrayEnabled = state
        if state then StartXRay() else StopXRay() end
    end)

    CreateButton("🧹 Limpar ESP Mortos", function()
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Player then
                if not plr.Character or not plr.Character:FindFirstChild("HumanoidRootPart") then
                    RemoveESPFromPlayer(plr)
                end
            end
        end
        print("✅ ESP de jogadores mortos removido!")
    end)

    CreateSeparator("🔧 SYSTEM")

    CreateButton("❌ Fechar Hub", function()
        StopAll()
        ScreenGui:Destroy()
    end)

    -- ==================== CRÉDITOS ====================
    local CreditFrame = Instance.new("Frame")
    CreditFrame.Size = UDim2.new(1, -10, 0, 120)
    CreditFrame.BackgroundColor3 = PurpleTheme.Dark
    CreditFrame.BorderSizePixel = 0
    CreditFrame.Parent = ScrollContent

    local CreditCorner = Instance.new("UICorner")
    CreditCorner.CornerRadius = UDim.new(0, 10)
    CreditCorner.Parent = CreditFrame

    local CreditTitle = Instance.new("TextLabel")
    CreditTitle.Size = UDim2.new(1, 0, 0, 25)
    CreditTitle.Position = UDim2.new(0, 0, 0, 10)
    CreditTitle.BackgroundTransparency = 1
    CreditTitle.Text = "🌙 MOON HUB v1.0"
    CreditTitle.TextColor3 = PurpleTheme.Accent
    CreditTitle.Font = Enum.Font.GothamBold
    CreditTitle.TextSize = 18
    CreditTitle.Parent = CreditFrame

    local CreditDev = Instance.new("TextLabel")
    CreditDev.Size = UDim2.new(1, 0, 0, 20)
    CreditDev.Position = UDim2.new(0, 0, 0, 35)
    CreditDev.BackgroundTransparency = 1
    CreditDev.Text = "Desenvolvido por: SILVA"
    CreditDev.TextColor3 = PurpleTheme.Text
    CreditDev.Font = Enum.Font.GothamSemibold
    CreditDev.TextSize = 14
    CreditDev.Parent = CreditFrame

    local CreditOptions = Instance.new("TextLabel")
    CreditOptions.Size = UDim2.new(1, 0, 0, 20)
    CreditOptions.Position = UDim2.new(0, 0, 0, 55)
    CreditOptions.BackgroundTransparency = 1
    CreditOptions.Text = "15 Opções Universais"
    CreditOptions.TextColor3 = PurpleTheme.TextSecondary
    CreditOptions.Font = Enum.Font.Gotham
    CreditOptions.TextSize = 12
    CreditOptions.Parent = CreditFrame

    local CreditDiscord = Instance.new("TextLabel")
    CreditDiscord.Size = UDim2.new(1, 0, 0, 20)
    CreditDiscord.Position = UDim2.new(0, 0, 0, 75)
    CreditDiscord.BackgroundTransparency = 1
    CreditDiscord.Text = "💬 Discord: discord.gg/ZCzTgwdky"
    CreditDiscord.TextColor3 = Color3.fromRGB(100, 150, 255)
    CreditDiscord.Font = Enum.Font.GothamBold
    CreditDiscord.TextSize = 12
    CreditDiscord.Parent = CreditFrame

    local CreditYear = Instance.new("TextLabel")
    CreditYear.Size = UDim2.new(1, 0, 0, 15)
    CreditYear.Position = UDim2.new(0, 0, 0, 98)
    CreditYear.BackgroundTransparency = 1
    CreditYear.Text = "©️ 2024 - Todos os direitos reservados"
    CreditYear.TextColor3 = PurpleTheme.TextSecondary
    CreditYear.Font = Enum.Font.Gotham
    CreditYear.TextSize = 10
    CreditYear.Parent = CreditFrame

    -- Criar lista inicial
    CreatePlayerList()

    -- ==================== FUNÇÕES ====================

    function StopAll()
        StopSpeedHack()
        StopInfiniteJump()
        StopFly()
        StopNoclip()
        StopAimbot()
        StopGodMode()
        StopInvisible()
        StopClickTP()
        StopReach()
        StopAntiAfk()
        StopFOV()
        StopFullBright()
        StopWallHack()
        StopXRay()
        DisableESP()
    end

    function TeleportToPlayer(targetPlayer)
        if not targetPlayer then return end
        local targetChar = targetPlayer.Character
        if not targetChar then return end
        local targetRoot = targetChar:FindFirstChild("HumanoidRootPart")
        if not targetRoot then return end
        local myChar = Player.Character
        if not myChar then return end
        local myRoot = myChar:FindFirstChild("HumanoidRootPart")
        if not myRoot then return end
        
        myRoot.CFrame = targetRoot.CFrame + Vector3.new(0, 3, 0)
        print("✅ Teleportado para " .. targetPlayer.Name)
    end

    function StartSpeedHack()
        local function onCharacterAdded(character)
            local humanoid = character:WaitForChild("Humanoid")
            humanoid.WalkSpeed = speedHackValue
        end
        if Player.Character then
            local humanoid = Player.Character:FindFirstChild("Humanoid")
            if humanoid then humanoid.WalkSpeed = speedHackValue end
        end
        speedConnection = Player.CharacterAdded:Connect(onCharacterAdded)
        spawn(function()
            while speedEnabled do
                wait(0.5)
                if Player.Character then
                    local humanoid = Player.Character:FindFirstChild("Humanoid")
                    if humanoid and humanoid.WalkSpeed ~= speedHackValue then
                        humanoid.WalkSpeed = speedHackValue
                    end
                end
            end
        end)
    end

    function StopSpeedHack()
        speedEnabled = false
        if speedConnection then speedConnection:Disconnect() speedConnection = nil end
        if Player.Character then
            local humanoid = Player.Character:FindFirstChild("Humanoid")
            if humanoid then humanoid.WalkSpeed = 16 end
        end
    end

    function StartInfiniteJump()
        jumpConnection = UserInputService.InputBegan:Connect(function(input)
            if not infiniteJumpEnabled then return end
            if input.UserInputType == Enum.UserInputType.Touch or input.KeyCode == Enum.KeyCode.Space then
                local character = Player.Character
                if character then
                    local humanoid = character:FindFirstChild("Humanoid")
                    if humanoid then humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
                end
            end
        end)
    end

    function StopInfiniteJump()
        infiniteJumpEnabled = false
        if jumpConnection then jumpConnection:Disconnect() jumpConnection = nil end
    end

    function StartFly()
        local character = Player.Character or Player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        local rootPart = character:WaitForChild("HumanoidRootPart")

        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.P = 5000
        bodyVelocity.Name = "FlyVelocity"
        bodyVelocity.Parent = rootPart

        local bodyGyro = Instance.new("BodyGyro")
        bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
        bodyGyro.CFrame = rootPart.CFrame
        bodyGyro.P = 10000
        bodyGyro.D = 100
        bodyGyro.Name = "FlyGyro"
        bodyGyro.Parent = rootPart

        local inputState = {
            forward = false,
            back = false,
            left = false,
            right = false,
            up = false,
            down = false
        }

        local function createFlyButton(text, position, inputKey)
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(0, 55, 0, 55)
            btn.Position = position
            btn.BackgroundColor3 = Color3.fromRGB(130, 70, 190)
            btn.BackgroundTransparency = 0.5
            btn.Text = text
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 22
            btn.BorderSizePixel = 0
            btn.ZIndex = 10
            btn.Parent = ScreenGui
            
            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 12)
            corner.Parent = btn
            
            local stroke = Instance.new("UIStroke")
            stroke.Color = PurpleTheme.Accent
            stroke.Thickness = 2
            stroke.Parent = btn

            btn.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                    inputState[inputKey] = true
                    TweenService:Create(btn, TweenInfo.new(0.1), {BackgroundTransparency = 0.2}):Play()
                end
            end)
            
            btn.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                    inputState[inputKey] = false
                    TweenService:Create(btn, TweenInfo.new(0.1), {BackgroundTransparency = 0.5}):Play()
                end
            end)
            
            table.insert(flyButtons, btn)
            return btn
        end

        createFlyButton("▲", UDim2.new(0.8, -27, 0.35, 0), "forward")
        createFlyButton("▼", UDim2.new(0.8, -27, 0.62, 0), "back")
        createFlyButton("◄", UDim2.new(0.72, -27, 0.485, 0), "left")
        createFlyButton("►", UDim2.new(0.88, -27, 0.485, 0), "right")

        local btnUp = Instance.new("TextButton")
        btnUp.Size = UDim2.new(0, 40, 0, 40)
        btnUp.Position = UDim2.new(0.91, 0, 0.25, 0)
        btnUp.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
        btnUp.BackgroundTransparency = 0.5
        btnUp.Text = "⇧"
        btnUp.TextColor3 = Color3.fromRGB(255, 255, 255)
        btnUp.Font = Enum.Font.GothamBold
        btnUp.TextSize = 20
        btnUp.BorderSizePixel = 0
        btnUp.ZIndex = 10
        btnUp.Parent = ScreenGui
        
        local upCorner = Instance.new("UICorner")
        upCorner.CornerRadius = UDim.new(0, 20)
        upCorner.Parent = btnUp
        
        table.insert(flyButtons, btnUp)

        btnUp.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                inputState["up"] = true
                TweenService:Create(btnUp, TweenInfo.new(0.1), {BackgroundTransparency = 0.2}):Play()
            end
        end)
        
        btnUp.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                inputState["up"] = false
                TweenService:Create(btnUp, TweenInfo.new(0.1), {BackgroundTransparency = 0.5}):Play()
            end
        end)

        local btnDown = Instance.new("TextButton")
        btnDown.Size = UDim2.new(0, 40, 0, 40)
        btnDown.Position = UDim2.new(0.91, 0, 0.65, 0)
        btnDown.BackgroundColor3 = Color3.fromRGB(200, 100, 100)
        btnDown.BackgroundTransparency = 0.5
        btnDown.Text = "⇩"
        btnDown.TextColor3 = Color3.fromRGB(255, 255, 255)
        btnDown.Font = Enum.Font.GothamBold
        btnDown.TextSize = 20
        btnDown.BorderSizePixel = 0
        btnDown.ZIndex = 10
        btnDown.Parent = ScreenGui
        
        local downCorner = Instance.new("UICorner")
        downCorner.CornerRadius = UDim.new(0, 20)
        downCorner.Parent = btnDown
        
        table.insert(flyButtons, btnDown)

        btnDown.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                inputState["down"] = true
                TweenService:Create(btnDown, TweenInfo.new(0.1), {BackgroundTransparency = 0.2}):Play()
            end
        end)
        
        btnDown.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                inputState["down"] = false
                TweenService:Create(btnDown, TweenInfo.new(0.1), {BackgroundTransparency = 0.5}):Play()
            end
        end)

        flyConnection = RunService.Heartbeat:Connect(function()
            if not flyEnabled then return end
            local char = Player.Character
            if not char then return end
            local root = char:FindFirstChild("HumanoidRootPart")
            if not root or not root.Parent then return end
            local hum = char:FindFirstChild("Humanoid")
            local bv = root:FindFirstChild("FlyVelocity")
            local bg = root:FindFirstChild("FlyGyro")
            if not bv or not bg then return end
            
            local camForward = Vector3.new(Camera.CFrame.LookVector.X, 0, Camera.CFrame.LookVector.Z).Unit
            local camRight = Vector3.new(Camera.CFrame.RightVector.X, 0, Camera.CFrame.RightVector.Z).Unit
            local moveDir = Vector3.zero
            
            if inputState.forward then moveDir = moveDir + camForward end
            if inputState.back then moveDir = moveDir - camForward end
            if inputState.left then moveDir = moveDir - camRight end
            if inputState.right then moveDir = moveDir + camRight end
            if inputState.up then moveDir = moveDir + Vector3.new(0, 1, 0) end
            if inputState.down then moveDir = moveDir - Vector3.new(0, 1, 0) end
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + camForward end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - camForward end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - camRight end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + camRight end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, 1, 0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) or UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                moveDir = moveDir - Vector3.new(0, 1, 0)
            end
            
            if moveDir.Magnitude > 0 then
                bv.Velocity = moveDir.Unit * flySpeed
            else
                bv.Velocity = Vector3.zero
            end
            
            bg.CFrame = CFrame.new(root.Position, root.Position + Camera.CFrame.LookVector)
            
            if hum then hum.PlatformStand = true end
        end)
    end

    function StopFly()
        flyEnabled = false
        if flyConnection then flyConnection:Disconnect() flyConnection = nil end
        local char = Player.Character
        if char then
            local root = char:FindFirstChild("HumanoidRootPart")
            if root then
                local bv = root:FindFirstChild("FlyVelocity")
                if bv then bv:Destroy() end
                local bg = root:FindFirstChild("FlyGyro")
                if bg then bg:Destroy() end
            end
            local hum = char:FindFirstChild("Humanoid")
            if hum then hum.PlatformStand = false end
        end
        for _, btn in pairs(flyButtons) do btn:Destroy() end
        flyButtons = {}
    end

    function StartNoclip()
        noclipConnection = RunService.Stepped:Connect(function()
            if not noclipEnabled then return end
            local character = Player.Character
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end

    function StopNoclip()
        noclipEnabled = false
        if noclipConnection then noclipConnection:Disconnect() noclipConnection = nil end
        local character = Player.Character
        if character then
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = true end
            end
        end
    end

    function StartAimbot()
        aimbotConnection = RunService.Heartbeat:Connect(function()
            if not aimbotEnabled then return end
            local myChar = Player.Character
            if not myChar then return end
            local myRoot = myChar:FindFirstChild("HumanoidRootPart")
            if not myRoot then return end
            local closestPlayer = nil
            local closestDistance = math.huge
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local targetRoot = plr.Character:FindFirstChild("HumanoidRootPart")
                    local targetHead = plr.Character:FindFirstChild("Head")
                    if targetRoot and targetHead then
                        local distance = (myRoot.Position - targetRoot.Position).Magnitude
                        if distance < closestDistance then
                            closestDistance = distance
                            closestPlayer = plr
                        end
                    end
                end
            end
            if closestPlayer and closestPlayer.Character then
                local targetHead = closestPlayer.Character:FindFirstChild("Head")
                if targetHead then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, targetHead.Position)
                end
            end
        end)
    end

    function StopAimbot()
        aimbotEnabled = false
        if aimbotConnection then aimbotConnection:Disconnect() aimbotConnection = nil end
    end

    function StartGodMode()
        godModeConnection = RunService.Heartbeat:Connect(function()
            if not godModeEnabled then return end
            local character = Player.Character
            if character then
                local humanoid = character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.Health = humanoid.MaxHealth
                    humanoid.BreakJointsOnDeath = false
                end
            end
        end)
    end

    function StopGodMode()
        godModeEnabled = false
        if godModeConnection then godModeConnection:Disconnect() godModeConnection = nil end
        local character = Player.Character
        if character then
            local humanoid = character:FindFirstChild("Humanoid")
            if humanoid then humanoid.BreakJointsOnDeath = true end
        end
    end

    function StartInvisible()
        invisibleConnection = RunService.Heartbeat:Connect(function()
            if not invisibleEnabled then return end
            local character = Player.Character
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Transparency < 1 then
                        part.Transparency = 0.8
                    end
                end
            end
        end)
    end

    function StopInvisible()
        invisibleEnabled = false
        if invisibleConnection then invisibleConnection:Disconnect() invisibleConnection = nil end
        local character = Player.Character
        if character then
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then part.Transparency = 0 end
            end
        end
    end

    function StartClickTP()
        clickTPConnection = UserInputService.InputBegan:Connect(function(input)
            if not clickTPEnabled then return end
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                local character = Player.Character
                if not character then return end
                local rootPart = character:FindFirstChild("HumanoidRootPart")
                if not rootPart then return end
                
                local mousePosition = UserInputService:GetMouseLocation()
                local ray = Camera:ScreenPointToRay(mousePosition.X, mousePosition.Y)
                local raycastResult = workspace:Raycast(ray.Origin, ray.Direction * 1000)
                
                if raycastResult then
                    rootPart.CFrame = CFrame.new(raycastResult.Position + Vector3.new(0, 3, 0))
                end
            end
        end)
    end

    function StopClickTP()
        clickTPEnabled = false
        if clickTPConnection then clickTPConnection:Disconnect() clickTPConnection = nil end
    end

    function StartReach()
        reachConnection = RunService.Heartbeat:Connect(function()
            if not reachEnabled then return end
            local character = Player.Character
            if character then
                for _, tool in pairs(character:GetChildren()) do
                    if tool:IsA("Tool") then
                        local handle = tool:FindFirstChild("Handle")
                        if handle then
                            handle.Size = Vector3.new(reachValue, reachValue, reachValue)
                            handle.CanCollide = false
                        end
                    end
                end
            end
        end)
    end

    function StopReach()
        reachEnabled = false
        if reachConnection then reachConnection:Disconnect() reachConnection = nil end
    end

    function StartAntiAfk()
        local virtualUser = game:GetService("VirtualUser")
        Player.Idled:Connect(function()
            if antiAfkEnabled then
                virtualUser:CaptureController()
                virtualUser:ClickButton2(Vector2.new())
            end
        end)
        
        antiAfkConnection = RunService.Heartbeat:Connect(function()
            if not antiAfkEnabled then return end
            local character = Player.Character
            if character then
                local humanoid = character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.MoveDirection = Vector3.new(0.01, 0, 0.01)
                end
            end
        end)
    end

    function StopAntiAfk()
        antiAfkEnabled = false
        if antiAfkConnection then antiAfkConnection:Disconnect() antiAfkConnection = nil end
    end

    function StartFOV()
        Camera.FieldOfView = fovValue
        fovConnection = RunService.Heartbeat:Connect(function()
            if not fovEnabled then return end
            if Camera.FieldOfView ~= fovValue then
                Camera.FieldOfView = fovValue
            end
        end)
    end

    function StopFOV()
        fovEnabled = false
        if fovConnection then fovConnection:Disconnect() fovConnection = nil end
        Camera.FieldOfView = 70
    end

    function StartFullBright()
        local lighting = game:GetService("Lighting")
        originalLighting = {
            Brightness = lighting.Brightness,
            ClockTime = lighting.ClockTime,
            FogEnd = lighting.FogEnd,
            GlobalShadows = lighting.GlobalShadows,
            Ambient = lighting.Ambient
        }
        
        lighting.Brightness = 2
        lighting.ClockTime = 14
        lighting.FogEnd = 100000
        lighting.GlobalShadows = false
        lighting.Ambient = Color3.fromRGB(255, 255, 255)
        
        fullbrightConnection = RunService.Heartbeat:Connect(function()
            if not fullbrightEnabled then return end
            if lighting.Brightness ~= 2 then lighting.Brightness = 2 end
            if lighting.ClockTime ~= 14 then lighting.ClockTime = 14 end
            if lighting.FogEnd ~= 100000 then lighting.FogEnd = 100000 end
            if lighting.GlobalShadows ~= false then lighting.GlobalShadows = false end
        end)
    end

    function StopFullBright()
        fullbrightEnabled = false
        if fullbrightConnection then fullbrightConnection:Disconnect() fullbrightConnection = nil end
        local lighting = game:GetService("Lighting")
        lighting.Brightness = originalLighting.Brightness or 1
        lighting.ClockTime = originalLighting.ClockTime or 14
        lighting.FogEnd = originalLighting.FogEnd or 10000
        lighting.GlobalShadows = originalLighting.GlobalShadows or true
        lighting.Ambient = originalLighting.Ambient or Color3.fromRGB(0, 0, 0)
    end

    -- WALL HACK CORRIGIDO
    function StartWallHack()
        wallhackObjects = {}
        for _, object in pairs(workspace:GetDescendants()) do
            if object:IsA("BasePart") and object.Transparency < 0.5 and object ~= workspace.Terrain then
                table.insert(wallhackObjects, {object = object, originalTransparency = object.Transparency})
                object.Transparency = 0.5
            end
        end
    end

    function StopWallHack()
        wallhackEnabled = false
        if wallhackConnection then wallhackConnection:Disconnect() wallhackConnection = nil end
        
        for _, data in pairs(wallhackObjects) do
            if data.object and data.object.Parent then
                data.object.Transparency = data.originalTransparency
            end
        end
        wallhackObjects = {}
    end

    -- X-RAY CORRIGIDO
    function StartXRay()
        originalProperties = {}
        for _, object in pairs(workspace:GetDescendants()) do
            if object:IsA("BasePart") and object.Transparency < 0.8 and object ~= workspace.Terrain then
                table.insert(originalProperties, {object = object, originalTransparency = object.Transparency})
                object.Transparency = 0.8
            end
        end
    end

    function StopXRay()
        xrayEnabled = false
        if xrayConnection then xrayConnection:Disconnect() xrayConnection = nil end
        
        for _, data in pairs(originalProperties) do
            if data.object and data.object.Parent then
                data.object.Transparency = data.originalTransparency
            end
        end
        originalProperties = {}
    end

    function EnableESP()
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Player then SetupESPPlayer(plr) end
        end
        local playerAddedConn = Players.PlayerAdded:Connect(function(plr)
            if plr ~= Player then SetupESPPlayer(plr) end
        end)
        table.insert(espConnections, playerAddedConn)
        local playerRemovingConn = Players.PlayerRemoving:Connect(function(plr)
            RemoveESPFromPlayer(plr)
        end)
        table.insert(espConnections, playerRemovingConn)
    end

    function SetupESPPlayer(target)
        local function onCharacterAdded(character)
            local head = character:WaitForChild("Head", 5)
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart", 5)
            if not head or not humanoidRootPart then return end

            local highlight = Instance.new("Highlight")
            highlight.Name = "MoonHub_ESP"
            highlight.FillColor = Color3.fromRGB(160, 100, 220)
            highlight.FillTransparency = 0.5
            highlight.OutlineColor = PurpleTheme.Accent
            highlight.OutlineTransparency = 0
            highlight.Parent = character

            local billboard = Instance.new("BillboardGui")
            billboard.Name = "MoonHub_NameTag"
            billboard.Size = UDim2.new(0, 150, 0, 40)
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            billboard.AlwaysOnTop = true
            billboard.Parent = head

            local frame = Instance.new("Frame")
            frame.Size = UDim2.new(1, 0, 1, 0)
            frame.BackgroundTransparency = 1
            frame.Parent = billboard

            local nameLabel = Instance.new("TextLabel")
            nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
            nameLabel.BackgroundTransparency = 1
            nameLabel.Text = target.Name
            nameLabel.TextColor3 = PurpleTheme.Accent
            nameLabel.Font = Enum.Font.GothamBold
            nameLabel.TextSize = 14
            nameLabel.TextStrokeTransparency = 0
            nameLabel.Parent = frame

            local distanceLabel = Instance.new("TextLabel")
            distanceLabel.Size = UDim2.new(1, 0, 0.5, 0)
            distanceLabel.Position = UDim2.new(0, 0, 0.5, 0)
            distanceLabel.BackgroundTransparency = 1
            distanceLabel.Text = "0 studs"
            distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            distanceLabel.Font = Enum.Font.Gotham
            distanceLabel.TextSize = 12
            distanceLabel.TextStrokeTransparency = 0.5
            distanceLabel.Parent = frame

            local updateConnection
            updateConnection = RunService.Heartbeat:Connect(function()
                if not character or not character.Parent or not Player.Character or not Player.Character:FindFirstChild("HumanoidRootPart") then
                    if updateConnection then updateConnection:Disconnect() end
                    return
                end
                local dist = (Player.Character.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude
                distanceLabel.Text = math.floor(dist) .. " studs"
            end)
        end

        if target.Character then onCharacterAdded(target.Character) end
        target.CharacterAdded:Connect(onCharacterAdded)
    end

    function RemoveESPFromPlayer(target)
        if target.Character then
            local highlight = target.Character:FindFirstChild("MoonHub_ESP")
            if highlight then highlight:Destroy() end
            local head = target.Character:FindFirstChild("Head")
            if head then
                local billboard = head:FindFirstChild("MoonHub_NameTag")
                if billboard then billboard:Destroy() end
            end
        end
    end

    function DisableESP()
        espEnabled = false
        for _, conn in pairs(espConnections) do
            conn:Disconnect()
        end
        espConnections = {}
        for _, plr in pairs(Players:GetPlayers()) do
            RemoveESPFromPlayer(plr)
        end
    end

    -- Minimizar
    local minimized = false
    MinimizeButton.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            TweenService:Create(MainFrame, TweenInfo.new(0.3), {
                Size = UDim2.new(0, 300, 0, 50)
            }):Play()
            ContentFrame.Visible = false
            MinimizeButton.Text = "☰"
        else
            TweenService:Create(MainFrame, TweenInfo.new(0.3), {
                Size = UDim2.new(0, 300, 0, 400)
            }):Play()
            wait(0.35)
            ContentFrame.Visible = true
            MinimizeButton.Text = "—"
        end
    end)

    -- Arrastar Suave
    local isDragging = false
    local dragStartPos = nil
    local frameStartPos = nil

    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            isDragging = true
            dragStartPos = input.Position
            frameStartPos = MainFrame.Position
        end
    end)

    TitleBar.InputChanged:Connect(function(input)
        if isDragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
            local delta = input.Position - dragStartPos
            local newX = frameStartPos.X.Offset + delta.X
            local newY = frameStartPos.Y.Offset + delta.Y
            local screenSize = Camera.ViewportSize
            local frameSize = MainFrame.AbsoluteSize
            newX = math.clamp(newX, -frameSize.X/2, screenSize.X - frameSize.X/2)
            newY = math.clamp(newY, 0, screenSize.Y - frameSize.Y/2)
            MainFrame.Position = UDim2.new(0, newX, 0, newY)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            isDragging = false
        end
    end)

    MainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            isDragging = true
            dragStartPos = input.Position
            frameStartPos = MainFrame.Position
        end
    end)

    ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 30)

    print("🌙 MOON HUB carregado com sucesso!")
    print("👨‍💻 Feito por: SILVA")
    print("💬 Discord: discord.gg/ZCzTgwdky")
    print("📋 15 opções universais disponíveis!")
    print("✅ X-Ray e Wall Hack: Ativam E Desativam corretamente!")
end

print("🔐 MOON HUB - Tela de Senha")
print("👨‍💻 Criado por: SILVA")
print("💬 Discord: discord.gg/ZCzTgwdky")

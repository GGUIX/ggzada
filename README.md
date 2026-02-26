-- Valentine Hub v2.0
-- Advanced Script Hub with Multiple Features

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local MinimizeButton = Instance.new("TextButton")
local TabContainer = Instance.new("Frame")
local ContentFrame = Instance.new("Frame")
local CreditsLabel = Instance.new("TextLabel")

-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lighting = game:GetService("Lighting")

-- Variables
local player = Players.LocalPlayer
local states = {}
local connections = {}
local minimized = false
local currentTab = "Main"

-- Screen GUI Setup
ScreenGui.Name = "ValentineHub"
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Main Frame
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 500, 0, 450)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.ClipsDescendants = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(255, 50, 150)
MainStroke.Thickness = 2
MainStroke.Parent = MainFrame

-- Title Bar
Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.BorderSizePixel = 0
Title.Size = UDim2.new(1, 0, 0, 45)
Title.Font = Enum.Font.GothamBold
Title.Text = "💝 Valentine Hub v2.0"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 18
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.TextXAlignment = Enum.TextXAlignment.Center

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 12)
TitleCorner.Parent = Title

-- Close Button
CloseButton.Name = "CloseButton"
CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
CloseButton.BorderSizePixel = 0
CloseButton.Position = UDim2.new(1, -40, 0, 7)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 16

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseButton

-- Minimize Button
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Parent = MainFrame
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 200, 50)
MinimizeButton.BorderSizePixel = 0
MinimizeButton.Position = UDim2.new(1, -75, 0, 7)
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 20

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 8)
MinCorner.Parent = MinimizeButton

-- Tab Container
TabContainer.Name = "TabContainer"
TabContainer.Parent = MainFrame
TabContainer.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
TabContainer.BorderSizePixel = 0
TabContainer.Position = UDim2.new(0, 0, 0, 45)
TabContainer.Size = UDim2.new(0, 120, 1, -45)

local TabLayout = Instance.new("UIListLayout")
TabLayout.Parent = TabContainer
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Padding = UDim.new(0, 5)

local TabPadding = Instance.new("UIPadding")
TabPadding.Parent = TabContainer
TabPadding.PaddingTop = UDim.new(0, 10)
TabPadding.PaddingLeft = UDim.new(0, 5)
TabPadding.PaddingRight = UDim.new(0, 5)

-- Content Frame
ContentFrame.Name = "ContentFrame"
ContentFrame.Parent = MainFrame
ContentFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
ContentFrame.BorderSizePixel = 0
ContentFrame.Position = UDim2.new(0, 120, 0, 45)
ContentFrame.Size = UDim2.new(1, -120, 1, -75)

-- Credits Label
CreditsLabel.Name = "Credits"
CreditsLabel.Parent = MainFrame
CreditsLabel.BackgroundTransparency = 1
CreditsLabel.Position = UDim2.new(0, 125, 1, -25)
CreditsLabel.Size = UDim2.new(1, -130, 0, 20)
CreditsLabel.Font = Enum.Font.Gotham
CreditsLabel.Text = "Made with ❤️ by Valentine Team"
CreditsLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
CreditsLabel.TextSize = 12
CreditsLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Functions
local function createTab(name, icon)
    local tab = Instance.new("TextButton")
    tab.Name = name .. "Tab"
    tab.Parent = TabContainer
    tab.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    tab.BorderSizePixel = 0
    tab.Size = UDim2.new(1, 0, 0, 40)
    tab.Font = Enum.Font.GothamBold
    tab.Text = icon .. " " .. name
    tab.TextColor3 = Color3.fromRGB(200, 200, 200)
    tab.TextSize = 14
    tab.TextXAlignment = Enum.TextXAlignment.Left
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 8)
    tabCorner.Parent = tab
    
    local tabPadding = Instance.new("UIPadding")
    tabPadding.PaddingLeft = UDim.new(0, 10)
    tabPadding.Parent = tab
    
    tab.MouseButton1Click:Connect(function()
        currentTab = name
        for _, child in pairs(ContentFrame:GetChildren()) do
            if child:IsA("ScrollingFrame") then
                child.Visible = (child.Name == name .. "Content")
            end
        end
        for _, t in pairs(TabContainer:GetChildren()) do
            if t:IsA("TextButton") then
                t.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
                t.TextColor3 = Color3.fromRGB(200, 200, 200)
            end
        end
        tab.BackgroundColor3 = Color3.fromRGB(255, 50, 150)
        tab.TextColor3 = Color3.fromRGB(255, 255, 255)
    end)
    
    return tab
end

local function createScrollFrame(name)
    local scroll = Instance.new("ScrollingFrame")
    scroll.Name = name .. "Content"
    scroll.Parent = ContentFrame
    scroll.BackgroundTransparency = 1
    scroll.BorderSizePixel = 0
    scroll.Size = UDim2.new(1, 0, 1, 0)
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.ScrollBarThickness = 4
    scroll.ScrollBarImageColor3 = Color3.fromRGB(255, 50, 150)
    scroll.Visible = (name == "Main")
    
    local layout = Instance.new("UIListLayout")
    layout.Parent = scroll
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 8)
    
    local padding = Instance.new("UIPadding")
    padding.Parent = scroll
    padding.PaddingTop = UDim.new(0, 10)
    padding.PaddingLeft = UDim.new(0, 10)
    padding.PaddingRight = UDim.new(0, 10)
    padding.PaddingBottom = UDim.new(0, 10)
    
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 20)
    end)
    
    return scroll
end

local function createButton(parent, name, callback)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = parent
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.BorderSizePixel = 0
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Font = Enum.Font.GothamBold
    btn.Text = name .. ": OFF"
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 14
    btn.AutoButtonColor = false
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = btn
    
    local indicator = Instance.new("Frame")
    indicator.Name = "Indicator"
    indicator.Parent = btn
    indicator.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    indicator.BorderSizePixel = 0
    indicator.Position = UDim2.new(1, -35, 0.5, -10)
    indicator.Size = UDim2.new(0, 20, 0, 20)
    
    local indCorner = Instance.new("UICorner")
    indCorner.CornerRadius = UDim.new(1, 0)
    indCorner.Parent = indicator
    
    states[name] = false
    
    btn.MouseButton1Click:Connect(function()
        states[name] = not states[name]
        btn.Text = name .. ": " .. (states[name] and "ON" or "OFF")
        
        local targetColor = states[name] and Color3.fromRGB(50, 255, 100) or Color3.fromRGB(255, 50, 50)
        TweenService:Create(indicator, TweenInfo.new(0.3), {BackgroundColor3 = targetColor}):Play()
        
        callback(states[name])
    end)
    
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 50)}):Play()
    end)
    
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
    end)
    
    return btn
end

local function createSlider(parent, name, min, max, default, callback)
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Name = name .. "Slider"
    sliderFrame.Parent = parent
    sliderFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    sliderFrame.BorderSizePixel = 0
    sliderFrame.Size = UDim2.new(1, 0, 0, 50)
    
    local frameCorner = Instance.new("UICorner")
    frameCorner.CornerRadius = UDim.new(0, 8)
    frameCorner.Parent = sliderFrame
    
    local label = Instance.new("TextLabel")
    label.Parent = sliderFrame
    label.BackgroundTransparency = 1
    label.Position = UDim2.new(0, 10, 0, 5)
    label.Size = UDim2.new(1, -20, 0, 20)
    label.Font = Enum.Font.GothamBold
    label.Text = name .. ": " .. default
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local sliderBar = Instance.new("Frame")
    sliderBar.Parent = sliderFrame
    sliderBar.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    sliderBar.BorderSizePixel = 0
    sliderBar.Position = UDim2.new(0, 10, 0, 30)
    sliderBar.Size = UDim2.new(1, -20, 0, 8)
    
    local barCorner = Instance.new("UICorner")
    barCorner.CornerRadius = UDim.new(1, 0)
    barCorner.Parent = sliderBar
    
    local sliderFill = Instance.new("Frame")
    sliderFill.Parent = sliderBar
    sliderFill.BackgroundColor3 = Color3.fromRGB(255, 50, 150)
    sliderFill.BorderSizePixel = 0
    sliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = sliderFill
    
    local dragging = false
    
    sliderBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local pos = math.clamp((input.Position.X - sliderBar.AbsolutePosition.X) / sliderBar.AbsoluteSize.X, 0, 1)
            sliderFill.Size = UDim2.new(pos, 0, 1, 0)
            local value = math.floor(min + (max - min) * pos)
            label.Text = name .. ": " .. value
            callback(value)
        end
    end)
end

local function createLabel(parent, text)
    local label = Instance.new("TextLabel")
    label.Parent = parent
    label.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    label.BorderSizePixel = 0
    label.Size = UDim2.new(1, 0, 0, 30)
    label.Font = Enum.Font.GothamBold
    label.Text = text
    label.TextColor3 = Color3.fromRGB(255, 200, 100)
    label.TextSize = 14
    
    local labelCorner = Instance.new("UICorner")
    labelCorner.CornerRadius = UDim.new(0, 8)
    labelCorner.Parent = label
    
    return label
end

-- Character Setup
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local rootPart = char:WaitForChild("HumanoidRootPart")

player.CharacterAdded:Connect(function(newChar)
    char = newChar
    humanoid = char:WaitForChild("Humanoid")
    rootPart = char:WaitForChild("HumanoidRootPart")
end)

-- UI Setup
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
    for _, conn in pairs(connections) do
        if typeof(conn) == "RBXScriptConnection" then
            conn:Disconnect()
        elseif typeof(conn) == "Instance" then
            conn:Destroy()
        end
    end
end)

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    local targetSize = minimized and UDim2.new(0, 500, 0, 45) or UDim2.new(0, 500, 0, 450)
    TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = targetSize}):Play()
    MinimizeButton.Text = minimized and "+" or "-"
end)

-- Create Tabs
createTab("Main", "🏠")
createTab("Player", "👤")
createTab("Combat", "⚔️")
createTab("Visual", "👁️")
createTab("Misc", "⚙️")

-- Create Content Frames
local mainContent = createScrollFrame("Main")
local playerContent = createScrollFrame("Player")
local combatContent = createScrollFrame("Combat")
local visualContent = createScrollFrame("Visual")
local miscContent = createScrollFrame("Misc")

-- Main Tab
createLabel(mainContent, "🌟 Auto Farm Features")

createButton(mainContent, "Auto Farm Valentine", function(enabled)
    if connections.farm then connections.farm:Disconnect() end
    if enabled then
        connections.farm = RunService.Heartbeat:Connect(function()
            pcall(function()
                local args = {[1] = "collect"}
                ReplicatedStorage:WaitForChild("Valentine2026"):WaitForChild("RemoteEvent"):FireServer(unpack(args))
            end)
        end)
    end
end)

createButton(mainContent, "Auto Collect Items", function(enabled)
    if connections.collect then connections.collect:Disconnect() end
    if enabled then
        connections.collect = RunService.Heartbeat:Connect(function()
            for _, item in pairs(workspace:GetDescendants()) do
                if item:IsA("BasePart") and (item.Name:find("Coin") or item.Name:find("Heart")) then
                    pcall(function()
                        item.CFrame = rootPart.CFrame
                    end)
                end
            end
        end)
    end
end)

createButton(mainContent, "Auto Rebirth", function(enabled)
    if connections.rebirth then connections.rebirth:Disconnect() end
    if enabled then
        connections.rebirth = RunService.Heartbeat:Connect(function()
            pcall(function()
                ReplicatedStorage:FindFirstChild("Rebirth"):FireServer()
            end)
        end)
    end
end)

-- Player Tab
createLabel(playerContent, "🏃 Movement Features")

createSlider(playerContent, "Walk Speed", 16, 500, 16, function(value)
    humanoid.WalkSpeed = value
end)

createSlider(playerContent, "Jump Power", 50, 500, 50, function(value)
    humanoid.JumpPower = value
end)

createButton(playerContent, "Fly", function(enabled)
    if enabled then
        local bodyVelocity = Instance.new("BodyVelocity", rootPart)
        bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        connections.fly = bodyVelocity
        
        connections.flyLoop = game:GetService("RunService").Heartbeat:Connect(function()
            local cam = workspace.CurrentCamera
            local direction = Vector3.new(0, 0, 0)
            local userInput = game:GetService("UserInputService")
            
            if userInput:IsKeyDown(Enum.KeyCode.W) then direction = direction + cam.CFrame.LookVector end
            if userInput:IsKeyDown(Enum.KeyCode.S) then direction = direction - cam.CFrame.LookVector end
            if userInput:IsKeyDown(Enum.KeyCode.A) then direction = direction - cam.CFrame.RightVector end
            if userInput:IsKeyDown(Enum.KeyCode.D) then direction = direction + cam.CFrame.RightVector end
            if userInput:IsKeyDown(Enum.KeyCode.Space) then direction = direction + Vector3.new(0, 1, 0) end
            if userInput:IsKeyDown(Enum.KeyCode.LeftShift) then direction = direction - Vector3.new(0, 1, 0) end
            
            bodyVelocity.Velocity = direction * 50
        end)
    else
        if connections.fly then connections.fly:Destroy() end
        if connections.flyLoop then connections.flyLoop:Disconnect() end
    end
end)

createButton(playerContent, "Infinite Jump", function(enabled)
    if connections.jump then connections.jump:Disconnect() end
    if enabled then
        connections.jump = UserInputService.JumpRequest:Connect(function()
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    end
end)

createButton(playerContent, "Noclip", function(enabled)
    if connections.noclip then connections.noclip:Disconnect() end
    if enabled then
        connections.noclip = RunService.Stepped:Connect(function()
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end)
    else
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then part.CanCollide = true end
        end
    end
end)

createButton(playerContent, "God Mode", function(enabled)
    if enabled then
        connections.godmode = humanoid.HealthChanged:Connect(function()
            humanoid.Health = humanoid.MaxHealth
        end)
    else
        if connections.godmode then connections.godmode:Disconnect() end
    end
end)

createButton(playerContent, "Anti-AFK", function(enabled)
    if connections.antiafk then connections.antiafk:Disconnect() end
    if enabled then
        connections.antiafk = RunService.Heartbeat:Connect(function()
            pcall(function()
                game:GetService("VirtualUser"):CaptureController()
                game:GetService("VirtualUser"):ClickButton2(Vector2.new())
            end)
        end)
    end
end)

-- Combat Tab
createLabel(combatContent, "⚔️ Combat Features")

createButton(combatContent, "Hitbox Expander", function(enabled)
    if connections.hitbox then connections.hitbox:Disconnect() end
    if enabled then
        connections.hitbox = RunService.Heartbeat:Connect(function()
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and v.Parent:FindFirstChild("Humanoid") and v.Parent ~= char then
                    pcall(function()
                        v.Size = Vector3.new(20, 20, 20)
                        v.Transparency = 0.7
                        v.CanCollide = false
                        v.Massless = true
                    end)
                end
            end
        end)
    end
end)

createButton(combatContent, "Kill Aura", function(enabled)
    if connections.killaura then connections.killaura:Disconnect() end
    if enabled then
        connections.killaura = RunService.Heartbeat:Connect(function()
            for _, enemy in pairs(workspace:GetDescendants()) do
                if enemy:IsA("Humanoid") and enemy.Parent ~= char and enemy.Health > 0 then
                    local enemyRoot = enemy.Parent:FindFirstChild("HumanoidRootPart")
                    if enemyRoot and (enemyRoot.Position - rootPart.Position).Magnitude <= 20 then
                        pcall(function()
                            ReplicatedStorage:FindFirstChild("Combat"):FireServer(enemy.Parent)
                        end)
                    end
                end
            end
        end)
    end
end)

createButton(combatContent, "Auto Attack", function(enabled)
    if connections.autoattack then connections.autoattack:Disconnect() end
    if enabled then
        connections.autoattack = RunService.Heartbeat:Connect(function()
            pcall(function()
                ReplicatedStorage:FindFirstChild("Attack"):FireServer()
            end)
        end)
    end
end)

createSlider(combatContent, "Attack Range", 5, 100, 5, function(value)
    pcall(function()
        player.Character:FindFirstChild("AttackRange").Value = value
    end)
end)

-- Visual Tab
createLabel(visualContent, "👁️ Visual Features")

createButton(visualContent, "ESP Players", function(enabled)
    if connections.esp then
        for _, v in pairs(connections.esp) do
            v:Destroy()
        end
        connections.esp = {}
    end
    
    if enabled then
        connections.esp = {}
        local function addESP(plr)
            if plr ~= player and plr.Character then
                local highlight = Instance.new("Highlight")
                highlight.Parent = plr.Character
                highlight.FillColor = Color3.fromRGB(255, 50, 150)
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0
                table.insert(connections.esp, highlight)
            end
        end
        
        for _, plr in pairs(Players:GetPlayers()) do
            addESP(plr)
        end
        
        connections.espAdded = Players.PlayerAdded:Connect(addESP)
    else
        if connections.espAdded then connections.espAdded:Disconnect() end
    end
end)

createButton(visualContent, "Fullbright", function(enabled)
    if enabled then
        connections.fullbright = Lighting.LightingChanged:Connect(function()
            Lighting.Brightness = 2
            Lighting.ClockTime = 14
            Lighting.FogEnd = 100000
            Lighting.GlobalShadows = false
            Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        end)
        Lighting.Brightness = 2
        Lighting.ClockTime = 14
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = false
        Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
    else
        if connections.fullbright then connections.fullbright:Disconnect() end
        Lighting.Brightness = 1
        Lighting.ClockTime = 12
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = true
        Lighting.OutdoorAmbient = Color3.fromRGB(70, 70, 70)
    end
end)

createButton(visualContent, "Remove Fog", function(enabled)
    if enabled then
        Lighting.FogEnd = 100000
        connections.fog = Lighting:GetPropertyChangedSignal("FogEnd"):Connect(function()
            Lighting.FogEnd = 100000
        end)
    else
        if connections.fog then connections.fog:Disconnect() end
    end
end)

createSlider(visualContent, "FOV", 70, 120, 70, function(value)
    workspace.CurrentCamera.FieldOfView = value
end)

-- Misc Tab
createLabel(miscContent, "⚙️ Miscellaneous")

createButton(miscContent, "Remove Death Barriers", function(enabled)
    if enabled then
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and (v.Name:lower():find("kill") or v.Name:lower():find("death")) then
                v:Destroy()
            end
        end
    end
end)

createButton(miscContent, "Unlock Gamepasses", function(enabled)
    if enabled then
        pcall(function()
            for _, v in pairs(player:GetChildren()) do
                if v:IsA("BoolValue") and v.Name:find("Pass") then
                    v.Value = true
                end
            end
        end)
    end
end)

createButton(miscContent, "Teleport Spawn", function()
    pcall(function()
        rootPart.CFrame = workspace.SpawnLocation.CFrame + Vector3.new(0, 5, 0)
    end)
end)

createButton(miscContent, "Reset Character", function()
    humanoid.Health = 0
end)

createButton(miscContent, "Rejoin Server", function()
    game:GetService("TeleportService"):Teleport(game.PlaceId, player)
end)

-- Notification
local function notify(title, text, duration)
    game.StarterGui:SetCore("SendNotification", {
        Title = title,
        Text = text,
        Duration = duration or 3
    })
end

notify("💝 Valentine Hub", "Successfully loaded! v2.0", 5)

for _, tab in pairs(TabContainer:GetChildren()) do
    if tab:IsA("TextButton") then
        tab.BackgroundColor3 = Color3.fromRGB(255, 50, 150)
        tab.TextColor3 = Color3.fromRGB(255, 255, 255)
        break
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.RightControl then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

print("Valentine Hub v2.0 loaded | Press Right Control to toggle UI")

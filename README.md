loadstring([[
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove GUI antiga
local oldGui = playerGui:FindFirstChild("Tr1nX_Macabro_GUI")
if oldGui then oldGui:Destroy() end

-- Criar GUI base
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Tr1nX_Macabro_GUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 500, 0, 600) -- maior
frame.Position = UDim2.new(0.25, 0, 0.15, 0)
frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local function addUICorner(obj, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius)
    corner.Parent = obj
end

local function createButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0.08, 0)
    btn.Position = UDim2.new(0.05, 0, posY, 0)
    btn.BackgroundColor3 = Color3.fromRGB(40,0,0)
    btn.TextColor3 = Color3.fromRGB(255,0,0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 26
    btn.Text = text
    btn.BorderSizePixel = 0
    addUICorner(btn, 15)
    btn.Parent = frame
    return btn
end

local title = Instance.new("TextLabel")
title.Size = UDim2.new(0.9, 0, 0.12, 0)
title.Position = UDim2.new(0.05, 0, 0.02, 0)
title.BackgroundTransparency = 0.5
title.BackgroundColor3 = Color3.fromRGB(0,0,0)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBlack
title.TextSize = 30
title.Text = "DEV BY Tr1nX_777 | MACABRO"
title.BorderSizePixel = 0
title.Parent = frame
addUICorner(title, 15)

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(0.9, 0, 0.07, 0)
statusLabel.Position = UDim2.new(0.05, 0, 0.88, 0)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextSize = 22
statusLabel.Text = "Status: Aguardando ação..."
statusLabel.Parent = frame

-- Botões principais
local btnDupe = createButton("💀 DUPLICAR ITEM NA MÃO", 0.16)
local btnESP = createButton("🧟 ESP: OFF", 0.28)
local btnFly = createButton("🦅 VOAR: OFF", 0.40)
local btnInvis = createButton("🕵️ INVISIBILIDADE: OFF", 0.52)

-- Input para Teleport
local tpBox = Instance.new("TextBox")
tpBox.Size = UDim2.new(0.6,0,0.07,0)
tpBox.Position = UDim2.new(0.05,0,0.64,0)
tpBox.PlaceholderText = "Nome do jogador para TP"
tpBox.BackgroundColor3 = Color3.fromRGB(20,20,20)
tpBox.TextColor3 = Color3.fromRGB(255,255,255)
tpBox.Font = Enum.Font.Gotham
tpBox.TextSize = 22
tpBox.BorderSizePixel = 0
addUICorner(tpBox, 15)
tpBox.Parent = frame

local tpBtn = Instance.new("TextButton")
tpBtn.Size = UDim2.new(0.3,0,0.07,0)
tpBtn.Position = UDim2.new(0.67,0,0.64,0)
tpBtn.BackgroundColor3 = Color3.fromRGB(40,0,0)
tpBtn.TextColor3 = Color3.fromRGB(255,0,0)
tpBtn.Font = Enum.Font.GothamBold
tpBtn.TextSize = 22
tpBtn.Text = "📍 TELEPORTAR"
tpBtn.BorderSizePixel = 0
addUICorner(tpBtn, 15)
tpBtn.Parent = frame

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255,0,0)
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.Text = "—"
minimizeBtn.BorderSizePixel = 0
addUICorner(minimizeBtn, 7)
minimizeBtn.Parent = frame

-- GUI Minimizada (quadradinho)
local miniFrame = Instance.new("Frame")
miniFrame.Size = UDim2.new(0, 100, 0, 40)
miniFrame.Position = UDim2.new(0.01, 0, 0.9, 0)
miniFrame.BackgroundColor3 = Color3.fromRGB(15,15,15)
miniFrame.BorderSizePixel = 0
miniFrame.Visible = false
miniFrame.Parent = screenGui
addUICorner(miniFrame, 10)

local miniLabel = Instance.new("TextLabel")
miniLabel.Size = UDim2.new(1, 0, 1, 0)
miniLabel.BackgroundTransparency = 1
miniLabel.TextColor3 = Color3.fromRGB(255,0,0)
miniLabel.Font = Enum.Font.GothamBlack
miniLabel.TextSize = 22
miniLabel.Text = "Th1x"
miniLabel.Parent = miniFrame

-- Função Minimizar e Restaurar
minimizeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
    miniFrame.Visible = true
end)

miniFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        frame.Visible = true
        miniFrame.Visible = false
    end
end)

-- Função DUPLICAR ITEM
btnDupe.MouseButton1Click:Connect(function()
    local character = player.Character
    if not character then
        statusLabel.Text = "Personagem não encontrado!"
        return
    end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local tool = character:FindFirstChildOfClass("Tool")
    if not tool then
        statusLabel.Text = "Sem item na mão!"
        return
    end

    local clone = tool:Clone()
    clone.Parent = character
    humanoid:EquipTool(clone)
    statusLabel.Text = "Item duplicado!"
end)

-- ESP
local espEnabled = false
local espHighlights = {}

local function removeESP()
    for plr, hl in pairs(espHighlights) do
        if hl then hl:Destroy() end
    end
    espHighlights = {}
end

local function updateESP()
    removeESP()
    if espEnabled then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local hl = Instance.new("Highlight")
                hl.Adornee = plr.Character
                hl.FillColor = Color3.fromRGB(255, 0, 0)
                hl.OutlineColor = Color3.fromRGB(255, 0, 0)
                hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                hl.Parent = playerGui
                espHighlights[plr] = hl
            end
        end
    end
end

btnESP.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if espEnabled then
        btnESP.Text = "🧟 ESP: ON"
        btnESP.TextColor3 = Color3.fromRGB(0,255,0)
        statusLabel.Text = "ESP ativado"
        updateESP()
    else
        btnESP.Text = "🧟 ESP: OFF"
        btnESP.TextColor3 = Color3.fromRGB(255,0,0)
        statusLabel.Text = "ESP desativado"
        removeESP()
    end
end)

Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function()
        if espEnabled then
            task.wait(1)
            updateESP()
        end
    end)
end)

-- Voo
local flyEnabled = false
local flying = false
local bodyVelocity, bodyGyro
local speed = 60

local function startFly()
    local char = player.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if not hrp or not humanoid then return end

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5)
    bodyVelocity.Velocity = Vector3.new(0,0,0)
    bodyVelocity.Parent = hrp

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(1e5,1e5,1e5)
    bodyGyro.CFrame = hrp.CFrame
    bodyGyro.Parent = hrp

    humanoid.PlatformStand = true
    flying = true

    statusLabel.Text = "Voando ativado"
end

local function stopFly()
    local char = player.Character
    if not char then return end
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
    if bodyGyro then bodyGyro:Destroy() bodyGyro = nil end
    if humanoid then humanoid.PlatformStand = false end
    flying = false
    statusLabel.Text = "Voando desativado"
end

RunService.Heartbeat:Connect(function()
    if flying and bodyVelocity and bodyGyro then
        local cam = workspace.CurrentCamera
        local direction = Vector3.new()
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            direction = direction + cam.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            direction = direction - cam.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            direction = direction - cam.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            direction = direction + cam.CFrame.RightVector
        end
        direction = direction.Unit
        if direction.Magnitude > 0 then
            bodyVelocity.Velocity = direction * speed
            bodyGyro.CFrame = cam.CFrame
        else
            bodyVelocity.Velocity = Vector3.new(0,0,0)
        end
    end
end)

btnFly.MouseButton1Click:Connect(function()
    flyEnabled = not flyEnabled
    if flyEnabled then
        startFly()
        btnFly.Text = "🦅 VOAR: ON"
        btnFly.TextColor3 = Color3.fromRGB(0,255,0)
    else
        stopFly()
        btnFly.Text = "🦅 VOAR: OFF"
        btnFly.TextColor3 = Color3.fromRGB(255,0,0)
    end
end)

-- Invisibilidade
local invisEnabled = false

btnInvis.MouseButton1Click:Connect(function()
    local char = player.Character
    if not char then
        statusLabel.Text = "Personagem não encontrado!"
        return
    end

    invisEnabled = not invisEnabled

    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") or part:IsA("Decal") or part:IsA("Accessory") then
            if invisEnabled then
                if part:IsA("BasePart") then
                    part.Transparency = 1
                elseif part:IsA("Decal") then
                    part.Transparency = 1
                elseif part:IsA("Accessory") then
                    if part:FindFirstChild("Handle") then
                        part.Handle.Transparency = 1
                    end
                end
            else
                if part:IsA("BasePart") then
                    part.Transparency = 0
                elseif part:IsA("Decal") then
                    part.Transparency = 0
                elseif part:IsA("Accessory") then
                    if part:FindFirstChild("Handle") then
                        part.Handle.Transparency = 0
                    end
                end
            end
        end
    end

    if invisEnabled then
        btnInvis.Text = "🕵️ INVISIBILIDADE: ON"
        btnInvis.TextColor3 = Color3.fromRGB(0,255,0)
        statusLabel.Text = "Invisibilidade ativada"
    else
        btnInvis.Text = "🕵️ INVISIBILIDADE: OFF"
        btnInvis.TextColor3 = Color3.fromRGB(255,0,0)
        statusLabel.Text = "Invisibilidade desativada"
    end
end)

-- Teleport
tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpBox.Text
    if targetName == "" then
        statusLabel.Text = "Digite o nome do jogador para teleportar!"
        return
    end

    local targetPlayer = Players:FindFirstChild(targetName)
    if not targetPlayer then
        statusLabel.Text = "Jogador não encontrado!"
        return
    end
    if not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        statusLabel.Text = "Jogador não está no jogo ou sem personagem!"
        return
    end

    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then
        statusLabel.Text = "Seu personagem não está pronto!"
        return
    end

    char.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,3,0)
    statusLabel.Text = "Teleportado para "..targetName
end)

-- Foco no TextBox quando clicar
tpBox.Focused:Connect(function()
    tpBox.Text = ""
end)
]])()

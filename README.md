# LucasHub
Script Roblox
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local ContentProvider = game:GetService("ContentProvider")
local player = Players.LocalPlayer

------------------------------------------------------------------
-- CONFIGURAÇÃO DOS ASSETS DO MEME 67
------------------------------------------------------------------
local ID_AUDIO_67 = "rbxassetid://9114223177" -- ID de som (Substitua se tiver outro ID do 67)
local ID_IMAGEM_67 = "rbxassetid://10700813292" -- Decal/Imagem do 67

-- Interface Principal
local gui = Instance.new("ScreenGui")
gui.Name = "LucasHub"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Instância de Áudio Global para o Meme
local sound67 = Instance.new("Sound")
sound67.SoundId = ID_AUDIO_67
sound67.Volume = 1
sound67.Parent = gui

------------------------------------------------------------------
-- ANIMAÇÃO DE ENTRADA (MEME 67 INTRO)
------------------------------------------------------------------
local introOverlay = Instance.new("Frame")
introOverlay.Size = UDim2.new(1, 0, 1, 0)
introOverlay.BackgroundColor3 = Color3.fromRGB(5, 5, 8)
introOverlay.BorderSizePixel = 0
introOverlay.ZIndex = 200
introOverlay.Parent = gui

local memeImage = Instance.new("ImageLabel")
memeImage.Size = UDim2.fromOffset(0, 0)
memeImage.Position = UDim2.new(0.5, 0, 0.4, 0)
memeImage.AnchorPoint = Vector2.new(0.5, 0.5)
memeImage.BackgroundTransparency = 1
memeImage.Image = ID_IMAGEM_67
memeImage.ZIndex = 201
memeImage.Parent = introOverlay

local introText = Instance.new("TextLabel")
introText.Size = UDim2.new(1, 0, 0, 60)
introText.Position = UDim2.new(0.5, 0, 0.75, 0)
introText.AnchorPoint = Vector2.new(0.5, 0.5)
introText.BackgroundTransparency = 1
introText.Text = "67!!!"
introText.TextColor3 = Color3.fromRGB(220, 80, 255)
introText.TextSize = 0
introText.Font = Enum.Font.GothamBlack
introText.ZIndex = 201
introText.Parent = introOverlay

------------------------------------------------------------------
-- JANELA PRINCIPAL
------------------------------------------------------------------
local main = Instance.new("Frame")
main.Size = UDim2.fromOffset(0, 0)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.BackgroundColor3 = Color3.fromRGB(12, 10, 18)
main.BorderSizePixel = 0
main.ClipsDescendants = true
main.Visible = false
main.Parent = gui

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 12)

local border = Instance.new("UIStroke")
border.Color = Color3.fromRGB(170, 50, 255)
border.Thickness = 2.5
border.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
border.Parent = main

-- Animação do Pulso Neon
task.spawn(function()
    local tweenInfo = TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    local tweenGlow = TweenService:Create(border, tweenInfo, {
        Color = Color3.fromRGB(220, 100, 255),
        Thickness = 3.5
    })
    tweenGlow:Play()
end)

------------------------------------------------------------------
-- EXECUTAR A ANIMAÇÃO DO 67
------------------------------------------------------------------
task.spawn(function()
    sound67:Play()

    -- Zoom e Impacto da Imagem e Texto do 67
    TweenService:Create(memeImage, TweenInfo.new(0.4, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out), {
        Size = UDim2.fromOffset(180, 180)
    }):Play()

    TweenService:Create(introText, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        TextSize = 48
    }):Play()

    task.wait(1.5)

    -- Sumir com o Meme
    TweenService:Create(memeImage, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
        Size = UDim2.fromOffset(0, 0)
    }):Play()
    
    TweenService:Create(introText, TweenInfo.new(0.3), {
        TextSize = 0
    }):Play()

    TweenService:Create(introOverlay, TweenInfo.new(0.4), {
        BackgroundTransparency = 1
    }):Play()

    task.wait(0.4)
    introOverlay:Destroy()

    -- ABRIR LUCAS HUB
    main.Visible = true
    TweenService:Create(main, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.fromOffset(270, 330),
        Position = UDim2.new(0.5, -135, 0.5, -165)
    }):Play()
end)

------------------------------------------------------------------
-- SISTEMA DE ARRASTAR / MOVER
------------------------------------------------------------------
local dragging, dragInput, dragStart, startPos

local function update(input)
    local delta = input.Position - dragStart
    main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

main.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = main.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

main.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

------------------------------------------------------------------
-- TOPO / TÍTULO E BOTÃO FECHAR
------------------------------------------------------------------
local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 40)
header.BackgroundTransparency = 1
header.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 1, 0)
title.Position = UDim2.fromOffset(12, 0)
title.BackgroundTransparency = 1
title.Text = "⚡ LUCAS HUB 67"
title.TextColor3 = Color3.fromRGB(200, 110, 255)
title.TextSize = 15
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = header

local close = Instance.new("TextButton")
close.Size = UDim2.fromOffset(26, 26)
close.Position = UDim2.new(1, -32, 0.5, -13)
close.BackgroundColor3 = Color3.fromRGB(35, 20, 50)
close.Text = "✕"
close.TextColor3 = Color3.fromRGB(255, 100, 120)
close.TextSize = 13
close.Font = Enum.Font.GothamBold
close.Parent = header

Instance.new("UICorner", close).CornerRadius = UDim.new(0, 6)

local closeBorder = Instance.new("UIStroke")
closeBorder.Color = Color3.fromRGB(170, 50, 255)
closeBorder.Thickness = 1
closeBorder.Parent = close

-- Linha Divisória
local line = Instance.new("Frame")
line.Size = UDim2.new(1, -20, 0, 1)
line.Position = UDim2.fromOffset(10, 40)
line.BackgroundColor3 = Color3.fromRGB(140, 50, 220)
line.BorderSizePixel = 0
line.Parent = main

-- Lista de Rolagem
local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, -16, 1, -50)
scroll.Position = UDim2.fromOffset(8, 45)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0
scroll.ScrollBarThickness = 3
scroll.ScrollBarImageColor3 = Color3.fromRGB(180, 80, 255)
scroll.Parent = main

local listLayout = Instance.new("UIListLayout")
listLayout.Padding = UDim.new(0, 6)
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
listLayout.Parent = scroll

listLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.fromOffset(0, listLayout.AbsoluteContentSize.Y + 8)
end)

------------------------------------------------------------------
-- POPUP DE CONFIRMAÇÃO DE FECHAMENTO
------------------------------------------------------------------
local confirmOverlay = Instance.new("Frame")
confirmOverlay.Size = UDim2.new(1, 0, 1, 0)
confirmOverlay.BackgroundColor3 = Color3.fromRGB(5, 5, 10)
confirmOverlay.BackgroundTransparency = 1
confirmOverlay.Visible = false
confirmOverlay.ZIndex = 10
confirmOverlay.Parent = main

local confirmFrame = Instance.new("Frame")
confirmFrame.Size = UDim2.fromOffset(210, 125)
confirmFrame.Position = UDim2.new(0.5, -105, 0.5, -62)
confirmFrame.BackgroundColor3 = Color3.fromRGB(18, 14, 28)
confirmFrame.BorderSizePixel = 0
confirmFrame.ZIndex = 11
confirmFrame.Parent = confirmOverlay

Instance.new("UICorner", confirmFrame).CornerRadius = UDim.new(0, 10)

local confirmStroke = Instance.new("UIStroke")
confirmStroke.Color = Color3.fromRGB(180, 70, 255)
confirmStroke.Thickness = 2
confirmStroke.Parent = confirmFrame

local confirmTitle = Instance.new("TextLabel")
confirmTitle.Size = UDim2.new(1, -10, 0, 45)
confirmTitle.Position = UDim2.fromOffset(5, 8)
confirmTitle.BackgroundTransparency = 1
confirmTitle.Text = "Deseja fechar o\nLucas Hub?"
confirmTitle.TextColor3 = Color3.fromRGB(240, 230, 255)
confirmTitle.TextSize = 13
confirmTitle.Font = Enum.Font.GothamBold
confirmTitle.ZIndex = 12
confirmTitle.Parent = confirmFrame

local btnConfirm = Instance.new("TextButton")
btnConfirm.Size = UDim2.fromOffset(85, 30)
btnConfirm.Position = UDim2.fromOffset(14, 75)
btnConfirm.BackgroundColor3 = Color3.fromRGB(150, 40, 230)
btnConfirm.Text = "Confirmar"
btnConfirm.TextColor3 = Color3.fromRGB(255, 255, 255)
btnConfirm.TextSize = 11
btnConfirm.Font = Enum.Font.GothamBold
btnConfirm.ZIndex = 12
btnConfirm.Parent = confirmFrame

Instance.new("UICorner", btnConfirm).CornerRadius = UDim.new(0, 6)

local btnCancel = Instance.new("TextButton")
btnCancel.Size = UDim2.fromOffset(85, 30)
btnCancel.Position = UDim2.new(1, -99, 0, 75)
btnCancel.BackgroundColor3 = Color3.fromRGB(35, 28, 48)
btnCancel.Text = "Cancelar"
btnCancel.TextColor3 = Color3.fromRGB(200, 190, 220)
btnCancel.TextSize = 11
btnCancel.Font = Enum.Font.GothamBold
btnCancel.ZIndex = 12
btnCancel.Parent = confirmFrame

Instance.new("UICorner", btnCancel).CornerRadius = UDim.new(0, 6)

close.MouseButton1Click:Connect(function()
    sound67:Play()
    TweenService:Create(close, TweenInfo.new(0.1), {Size = UDim2.fromOffset(20, 20)}):Play()
    task.wait(0.1)
    TweenService:Create(close, TweenInfo.new(0.1), {Size = UDim2.fromOffset(26, 26)}):Play()

    confirmOverlay.Visible = true
    TweenService:Create(confirmOverlay, TweenInfo.new(0.2), {BackgroundTransparency = 0.3}):Play()
end)

btnCancel.MouseButton1Click:Connect(function()
    TweenService:Create(confirmOverlay, TweenInfo.new(0.15), {BackgroundTransparency = 1}):Play()
    task.wait(0.15)
    confirmOverlay.Visible = false
end)

btnConfirm.MouseButton1Click:Connect(function()
    TweenService:Create(main, TweenInfo.new(0.25, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
        Size = UDim2.fromOffset(0, 0),
        Position = UDim2.new(main.Position.X.Scale, main.Position.X.Offset + 135, main.Position.Y.Scale, main.Position.Y.Offset + 165)
    }):Play()
    task.wait(0.25)
    gui:Destroy()
end)

------------------------------------------------------------------
-- FUNÇÃO DE CRIAR BOTÕES
------------------------------------------------------------------
local function CriarBotaoScript(nomeScript, linkUrl)
    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, -6, 0, 38)
    card.BackgroundColor3 = Color3.fromRGB(22, 18, 32)
    card.BorderSizePixel = 0
    card.Parent = scroll

    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 6)

    local cardStroke = Instance.new("UIStroke")
    cardStroke.Color = Color3.fromRGB(65, 45, 90)
    cardStroke.Thickness = 1
    cardStroke.Parent = card

    local txtName = Instance.new("TextLabel")
    txtName.Size = UDim2.new(1, -90, 1, 0)
    txtName.Position = UDim2.fromOffset(8, 0)
    txtName.BackgroundTransparency = 1
    txtName.Text = nomeScript
    txtName.TextColor3 = Color3.fromRGB(240, 235, 250)
    txtName.TextSize = 11
    txtName.Font = Enum.Font.GothamMedium
    txtName.TextXAlignment = Enum.TextXAlignment.Left
    txtName.Parent = card

    local btnExec = Instance.new("TextButton")
    btnExec.Size = UDim2.fromOffset(75, 24)
    btnExec.Position = UDim2.new(1, -80, 0.5, -12)
    btnExec.BackgroundColor3 = Color3.fromRGB(140, 45, 220)
    btnExec.Text = "► Executar"
    btnExec.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnExec.TextSize = 10
    btnExec.Font = Enum.Font.GothamBold
    btnExec.Parent = card

    Instance.new("UICorner", btnExec).CornerRadius = UDim.new(0, 5)

    btnExec.MouseButton1Click:Connect(function()
        sound67:Play()
        btnExec.Text = "Carregando..."
        task.spawn(function()
            local success, err = pcall(function()
                loadstring(game:HttpGet(linkUrl))()
            end)
            if not success then
                warn("Erro ao carregar o script:", err)
            end
            btnExec.Text = "► Executar"
        end)
    end)
end

------------------------------------------------------------------
-- SCRIPTS DO LUCAS HUB
------------------------------------------------------------------

CriarBotaoScript("Miranda Afk", "https://api.luarmor.net/files/v4/loaders/6b07a458832f08b2314f706f14723212.lua")
CriarBotaoScript("Servidor Privado", "https://raw.githubusercontent.com/kittylol-hub/Kittylol/refs/heads/main/main.lua")
CriarBotaoScript("Lennon Atualizado V3", "https://raw.githubusercontent.com/lennonxscripts/lennonhubv3/refs/heads/main/stealanegg.lua")
CriarBotaoScript("Miranda Steal Atualizado", "https://raw.githubusercontent.com/miirandahub/loader/refs/heads/main/stealaeggs")
CriarBotaoScript("Lennon Afk Novo", "https://raw.githubusercontent.com/lennonxscripts/lennonfarm/refs/heads/main/farmv1.lua")
CriarBotaoScript("Chilli Hub", "https://raw.githubusercontent.com/tienkhanh1/Chilli-Hub-Script/refs/heads/main/StealAnEgg")
CriarBotaoScript("Fly Community", "https://raw.githubusercontent.com/napun87/stealanegg/refs/heads/main/fly.lua")

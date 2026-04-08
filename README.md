-- =========================
-- Drakion Hub - Blox Fruit
-- =========================
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- GUI PRINCIPAL
local gui = Instance.new("ScreenGui")
gui.Name = "DrakionHub"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local mainFrame = Instance.new("Frame", gui)
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 600, 0, 350)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
-- começa escondido; será mostrado pelo botão de toggle
mainFrame.Visible = false
mainFrame.BorderSizePixel = 0
mainFrame.BackgroundTransparency = 0.15
mainFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)

Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 5)

local stroke1 = Instance.new("UIStroke", mainFrame)
stroke1.Color = Color3.fromRGB(0,0,0)
stroke1.Thickness = 4
stroke1.Transparency = 0

-- =========================
-- DRAG UI
-- =========================
do
    local dragging, dragStart, startPos
    mainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = mainFrame.Position
        end
    end)
    mainFrame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end
-- =========================
-- SIDEBAR ESQUERDO
-- =========================
local sidebar = Instance.new("Frame", mainFrame)
sidebar.Name = "Sidebar"
sidebar.Size = UDim2.new(0, 185, 0, 296.5)
sidebar.Position = UDim2.new(0, 0, 0, 54)
sidebar.BackgroundColor3 = Color3.fromRGB(3,0,75)
sidebar.BorderSizePixel = 0
sidebar.BackgroundTransparency = 1

local stroke = Instance.new("UIStroke", sidebar)
stroke.Color = Color3.fromRGB(0,0,0)
stroke.Thickness = 4
stroke.Transparency = 0

Instance.new("UICorner", sidebar).CornerRadius = UDim.new(0, 5)

local headerFrame = Instance.new("Frame", mainFrame)
headerFrame.Name = "HeaderFrame"
headerFrame.Size = UDim2.new(1, 0, 0, 50)
headerFrame.Position = UDim2.new(0, 0, 0, 0)
headerFrame.BackgroundColor3 = Color3.fromRGB(3,0, 75)
headerFrame.BackgroundTransparency = 1
headerFrame.BorderSizePixel = 0

-- Logo
local logo = Instance.new("ImageLabel", headerFrame)
logo.Name = "Logo"
logo.Size = UDim2.new(0, 100, 0, 100)
logo.Position = UDim2.new(0, 0, 0, -30)
logo.BackgroundTransparency = 1
logo.Image = "rbxassetid://129327299482883" -- troque pelo ID do seu arquivo
logo.ScaleType = Enum.ScaleType.Fit
logo.ZIndex = 3

-- Título ao lado da logo
local sidebarHeader = Instance.new("TextLabel", headerFrame)
sidebarHeader.Name = "SidebarHeader"
sidebarHeader.Size = UDim2.new(1, -72, 0, 64)
sidebarHeader.Position = UDim2.new(0, 140, 0, -6)
sidebarHeader.BackgroundTransparency = 1
sidebarHeader.TextColor3 = Color3.fromRGB(255,255,255)
sidebarHeader.TextScaled = false
sidebarHeader.Text = "Drakion Hub"
sidebarHeader.BorderSizePixel = 0
sidebarHeader.TextSize = 35
sidebarHeader.TextXAlignment = Enum.TextXAlignment.Left
sidebarHeader.TextYAlignment = Enum.TextYAlignment.Center
sidebarHeader.FontFace = Font.new(
    "rbxasset://fonts/families/BuilderSans.json",
    Enum.FontWeight.ExtraBold,
    Enum.FontStyle.Normal
)

-- gradiente dourado:
local grad = Instance.new("UIGradient", sidebarHeader)
grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}


local stroke = Instance.new("UIStroke", sidebarHeader)
stroke.Color = Color3.fromRGB(0,0,0)
stroke.Thickness = 2

-- Menu Items
local menuItems = {
    "Discord",
    "Main Farm",
    "Itens Quest",
	"Farm Outher",
    "Dungeon and Raid",
    "Teleport",
    "Fruits",
    "Shop",
    "Setting",
}

local scrollFrame = Instance.new("ScrollingFrame", sidebar)
scrollFrame.Name = "MenuScroll"
scrollFrame.Size = UDim2.new(1, 0, 1, -15)
scrollFrame.Position = UDim2.new(0, 0, 0, 10)
scrollFrame.BackgroundTransparency = 1
scrollFrame.BorderSizePixel = 0
scrollFrame.ScrollBarThickness = 6
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, #menuItems * 40)
Instance.new("UICorner", scrollFrame).CornerRadius = UDim.new(0, 5)

--sinalizador de menu selecionado
local selectedIndicator = Instance.new("Frame", scrollFrame)
selectedIndicator.Name = "Indicator"
selectedIndicator.Size = UDim2.new(0, 5, 0, 25)
selectedIndicator.Position = UDim2.new(0,10,0.15, 0) -- posição inicial, será movida para o botão selecionado
selectedIndicator.BackgroundColor3 = Color3.fromRGB(255,255,255)
selectedIndicator.BorderSizePixel = 0
selectedIndicator.Visible = false -- começa invisível, só mostra quando um menu é selecionado
selectedIndicator.BackgroundTransparency = 0
Instance.new("UICorner", selectedIndicator).CornerRadius = UDim.new(0, 4)
selectedIndicator.ZIndex = 3

for i, itemName in ipairs(menuItems) do
    local menuButton = Instance.new("TextButton", scrollFrame)
    menuButton.Name = itemName
    menuButton.Size = UDim2.new(0.9, 0, 0, 35)
    menuButton.Position = UDim2.new(0.05, 0, 0, (i - 1) * 40)
    menuButton.BackgroundColor3 = Color3.fromRGB(212,12,12)
	menuButton.BackgroundTransparency = 0.3
    menuButton.TextColor3 =
    Color3.fromRGB(255,255,255)
    menuButton.TextScaled = true
    menuButton.Text = itemName
    menuButton.BorderSizePixel = 0
    Instance.new("UICorner", menuButton).CornerRadius = UDim.new(0, 4)
    menuButton.TextScaled = false
    menuButton.TextSize = 20
    menuButton.Localize = false
    menuButton.ZIndex = 2
    menuButton.FontFace = Font.new(
    "rbxasset://fonts/families/BuilderSans.json",
    Enum.FontWeight.ExtraBold,
    Enum.FontStyle.Normal
)
    stroke = Instance.new("UIStroke", menuButton)
    stroke.Thickness = 2
    stroke.Transparency = 0

-- gradiente dourado:
local grad = Instance.new("UIGradient", menuButton)
grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}


    -- escurece ligeiramente ao passar o mouse
    menuButton.MouseEnter:Connect(function()
        menuButton.BackgroundTransparency = 0.7
    end)
    menuButton.MouseLeave:Connect(function()
        menuButton.BackgroundTransparency = 0.3
    end)

    -- move indicator when button is clicked
    menuButton.MouseButton1Click:Connect(function()
        selectedIndicator.Position = UDim2.new(0, 10, 0.015, (i - 1) * 40)
        selectedIndicator.Visible = true
    end)
end

-- gradiente dourado:
local grad = Instance.new("UIGradient", selectedIndicator)
grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}


	grad.Rotation = 90

-- start indicator on Credits menu (first item)
selectedIndicator.Position = UDim2.new(0, 10, 0.015, 0)
selectedIndicator.Visible = true
 
-- =========================
-- ÁREA PRINCIPAL (DIREITA)
-- =========================
local contentArea = Instance.new("Frame", mainFrame)
contentArea.Name = "ContentArea"
contentArea.Size = UDim2.new(1, -190, 1, -55)
contentArea.Position = UDim2.new(0, 190, 0, 55)
contentArea.BackgroundColor3 = Color3.fromRGB(3,0, 75)
contentArea.BorderSizePixel = 0
contentArea.BackgroundTransparency = 1
Instance.new("UICorner", contentArea).CornerRadius = UDim.new(0, 5)

local stroke2 = Instance.new("UIStroke", contentArea)
stroke2.Color = Color3.fromRGB(0,0,0)
stroke2.Thickness = 4
stroke2.Transparency = 0

-- Header da página
local pageHeader = Instance.new("TextLabel", contentArea)
pageHeader.Name = "PageHeader"
pageHeader.Size = UDim2.new(1,-50, 0, 50)
pageHeader.Position = UDim2.new(0,70,0,-53)
pageHeader.TextColor3 = Color3.fromRGB(255, 255, 255)
pageHeader.BackgroundColor3 = Color3.fromRGB(3,0, 75)
pageHeader.BackgroundTransparency = 1
pageHeader.Text = " discord.gg/Nfh5Sczyqz"
pageHeader.BorderSizePixel = 0
pageHeader.TextScaled = false
pageHeader.TextSize = 15
pageHeader.Font = Enum.Font.LuckiestGuy
pageHeader.TextXAlignment = Enum.TextXAlignment.Center
pageHeader.TextYAlignment = Enum.TextYAlignment.Center
pageHeader.TextTransparency = 0
Instance.new("UICorner", pageHeader).CornerRadius = UDim.new(0, 5)

-- gradiente dourado:
local grad = Instance.new("UIGradient", pageHeader)
grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}


local stroke = Instance.new("UIStroke", pageHeader)
stroke.Color = Color3.fromRGB(0,0,0)
stroke.Thickness = 2

 -- white underline below PageHeader
local headerUnderline = Instance.new("Frame", pageHeader)
headerUnderline.Name = "HeaderUnderline"
headerUnderline.Size = UDim2.new(1.68, 0, 0, 4)
headerUnderline.Position = UDim2.new(0, -263, 0, 48.8) -- right under headerFrame height
headerUnderline.BackgroundColor3 = Color3.fromRGB(0,0,0)
headerUnderline.BorderSizePixel = 0
headerUnderline.ZIndex = 2

-- Botão X (Fechar)
local closeBtn = Instance.new("TextButton", pageHeader)
closeBtn.Name = "CloseButton"
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -65, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(212,12,12)
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.TextScaled = true
closeBtn.Text = "×"
closeBtn.BorderSizePixel = 0
closeBtn.BackgroundTransparency = 0
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 4)

local stroke3 = Instance.new("UIStroke", closeBtn)
stroke3.Color = Color3.fromRGB(255,255,255)
stroke3.Thickness = 0.5
stroke3.Transparency = 0

closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- botão simples para abrir/fechar a interface principal
local toggleBtn = Instance.new("ImageButton", gui)
toggleBtn.Name = "ToggleButton"
toggleBtn.Size = UDim2.new(0,115,0,110.5)
toggleBtn.Position = UDim2.new(0,7,0,620)
toggleBtn.Image = "rbxassetid://129327299482883" -- troque pelo ID do seu arquivo (pode ser o mesmo da logo)
toggleBtn.BackgroundTransparency = 1
toggleBtn.BorderSizePixel = 0
toggleBtn.ZIndex = 2
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0, 10)

toggleBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)
 -- =========================
 -- =========================
-- DRAG UI botão da interface
-- =========================
do
    local dragging, dragStart, startPos
    toggleBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = toggleBtn.Position
        end
    end)
    toggleBtn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            toggleBtn.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end

-- references to menu buttons for later use
local discordBtn = scrollFrame:FindFirstChild("Discord")
local mainFarmBtn = scrollFrame:FindFirstChild("Main Farm")
local itensQuestBtn = scrollFrame:FindFirstChild("Itens Quest")
local FarmOutherBtn = scrollFrame:FindFirstChild("Farm Outher")
local dungeonRaidBtn = scrollFrame:FindFirstChild("Dungeon and Raid")
local teleportBtn = scrollFrame:FindFirstChild("Teleport")
local fruitsBtn = scrollFrame:FindFirstChild("Fruits")
local shopBtn = scrollFrame:FindFirstChild("Shop")
local settingBtn = scrollFrame:FindFirstChild("Setting")

-- table holding each page frame for easy hiding/showing
local pages = {}

local function showOnly(frame)
    for _, f in pairs(pages) do
        f.Visible = (f == frame)
    end
    -- update header text if you want
    if frame and frame.Name then
        pageHeader.Text = frame.Name:gsub("([A-Z])"," %1"):gsub("^%s+","")
        pageHeader.Localize = false
    end
end

---- Titulo das funções
local function createPageTitle(text, y, y2, yp, posicao)
    local TituloEntreAsFuncoes = Instance.new("TextLabel", posicao)
    TituloEntreAsFuncoes.Name = text
    TituloEntreAsFuncoes.Size = UDim2.new(0.9, 0, 0.1, 0)
    TituloEntreAsFuncoes.Position = UDim2.new(0.05, 0, y, 0)
    TituloEntreAsFuncoes.BackgroundTransparency = 1
    TituloEntreAsFuncoes.TextColor3 = Color3.fromRGB(255,255,255)
    TituloEntreAsFuncoes.Text = text
    TituloEntreAsFuncoes.TextScaled = false
    TituloEntreAsFuncoes.BorderSizePixel = 0
    TituloEntreAsFuncoes.TextTransparency = 0
    Instance.new("UICorner", TituloEntreAsFuncoes).CornerRadius = UDim.new(0, 4)
    TituloEntreAsFuncoes.FontFace = Font.new(
        "rbxasset://fonts/families/BuilderSans.json",
        Enum.FontWeight.ExtraBold,
        Enum.FontStyle.Normal
    )
    TituloEntreAsFuncoes.Localize = false
    TituloEntreAsFuncoes.ZIndex = 2
    TituloEntreAsFuncoes.TextSize = 20

    local stroke3 = Instance.new("UIStroke", TituloEntreAsFuncoes)
    stroke3.Thickness = 2
    stroke3.Transparency = 0

    -- gradiente dourado:
    local grad = Instance.new("UIGradient", TituloEntreAsFuncoes)
    grad.Color = ColorSequence.new{
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

    local UnderLineTtulo = Instance.new("Frame", TituloEntreAsFuncoes)
    UnderLineTtulo.Name = "UnderLineAutoStatus"
    UnderLineTtulo.Size = UDim2.new(0.9, 0, y2, 0)
    UnderLineTtulo.Position = UDim2.new(0.05, 0, yp, 0)
    UnderLineTtulo.BackgroundTransparency = 0
    UnderLineTtulo.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    UnderLineTtulo.BorderSizePixel = 0
    Instance.new("UICorner", UnderLineTtulo).CornerRadius = UDim.new(0, 4)

    -- gradiente dourado:
    local grad = Instance.new("UIGradient", UnderLineTtulo)
    grad.Color = ColorSequence.new{
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(212, 12, 12)),
    	ColorSequenceKeypoint.new(0.4, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.7, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(212, 12, 12)),
    	ColorSequenceKeypoint.new(1, Color3.fromRGB(212, 12, 12))
    }

    grad.Transparency = NumberSequence.new{
        NumberSequenceKeypoint.new(0, 1),
       NumberSequenceKeypoint.new(0.4, 0),
       NumberSequenceKeypoint.new(0.7, 0),
      NumberSequenceKeypoint.new(1, 1),
    }
    return TituloEntreAsFuncoes 
end

--- Rodapé das Tabs
local function createFooter(y1, y2, y3, yt, posicao)
    local UnderLineRodape = Instance.new("Frame", posicao)
    UnderLineRodape.Name = "UnderLineRodape"
    UnderLineRodape.Size = UDim2.new(1, 0, yt, 0)
    UnderLineRodape.Position = UDim2.new(0 , 0, y1, 0)
    UnderLineRodape.BackgroundTransparency = 0
    UnderLineRodape.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    UnderLineRodape.BorderSizePixel = 0
    Instance.new("UICorner", UnderLineRodape).CornerRadius = UDim.new(0, 4)

    -- gradiente dourado:
    local grad = Instance.new("UIGradient", UnderLineRodape)
    grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(212, 12, 12)),
	ColorSequenceKeypoint.new(0.4, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.7, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(212, 12, 12)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(212, 12, 12))
    }

    grad.Transparency = NumberSequence.new{
        NumberSequenceKeypoint.new(0, 1),
        NumberSequenceKeypoint.new(0.5, 0),
       NumberSequenceKeypoint.new(0.6, 0),
       NumberSequenceKeypoint.new(1, 1),
    }

    local LogoRodape = Instance.new("ImageLabel", posicao)
    LogoRodape.Size = UDim2.new(0, 35, 0, 35)
    LogoRodape.Position = UDim2.new(0.08,0, y2,0)
    LogoRodape.BackgroundTransparency = 1
    LogoRodape.Image = "rbxassetid://129327299482883"

    local TextRodape = Instance.new("TextLabel", posicao)
    TextRodape.Size = UDim2.new(0, 100, 0, 20)
    TextRodape.Position = UDim2.new(0.4, 0, y3, 0)
    TextRodape.BackgroundTransparency = 1
    TextRodape.TextColor3 = Color3.fromRGB(255, 255, 255)
    TextRodape.Text = "Drakion Hub  -  BY  龙'¨'珠- The Black -龙'¨'珠"
    TextRodape.TextScaled = false
    TextRodape.BorderSizePixel = 0
    TextRodape.TextTransparency = 0
    TextRodape.FontFace = Font.new(
     "rbxasset://fonts/families/GrenzeGotisch.json",
      Enum.FontWeight.ExtraBold,
      Enum.FontStyle.Normal
    )
    TextRodape.Localize = false
    TextRodape.ZIndex = 2
    TextRodape.TextSize = 20

     local grad = Instance.new("UIGradient", TextRodape)
     grad.Color = ColorSequence.new{
       ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
       ColorSequenceKeypoint.new(0.1, Color3.fromRGB(160, 120, 30)),
       ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 218, 27))}
        return UnderLineRodape, LogoRodape, TextRodape
     end

--- Botão das funções
local function createButton(text, y, posicao)
    local Botaoprincipal = Instance.new("Frame", posicao)
    Botaoprincipal.Name = text
    Botaoprincipal.Size = UDim2.new(0.9, 0, 0.04, 0)
    Botaoprincipal.Position = UDim2.new(0.05, 0, y, 0)
    Botaoprincipal.BackgroundTransparency = 0
    Botaoprincipal.BackgroundColor3 = Color3.fromRGB(0,0,0)
    Botaoprincipal.BorderSizePixel = 0
    Instance.new("UICorner", Botaoprincipal).CornerRadius = UDim.new(0, 4)
    Botaoprincipal.Localize = false

    local BotaoprincipalText = Instance.new("TextLabel", Botaoprincipal)
    BotaoprincipalText.Name = text.."Text"
    BotaoprincipalText.Size = UDim2.new(1, 0, 1, 0)
    BotaoprincipalText.Position = UDim2.new(0.05, 0, 0, 0)
    BotaoprincipalText.BackgroundTransparency = 1
    BotaoprincipalText.TextColor3 = Color3.fromRGB(255, 255, 255)
    BotaoprincipalText.Text = text
    BotaoprincipalText.TextScaled = false
    BotaoprincipalText.BorderSizePixel = 0
    BotaoprincipalText.TextTransparency = 0
    BotaoprincipalText.FontFace = Font.new(
        "rbxasset://fonts/families/BuilderSans.json",
        Enum.FontWeight.ExtraBold,
       Enum.FontStyle.Normal
    )
    BotaoprincipalText.TextXAlignment = Enum.TextXAlignment.Left
    BotaoprincipalText.TextYAlignment = Enum.TextYAlignment.Center
    BotaoprincipalText.Localize = false
    BotaoprincipalText.ZIndex = 2
    BotaoprincipalText.TextSize = 18

    local grad = Instance.new("UIGradient", BotaoprincipalText)
    grad.Color = ColorSequence.new{
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

    local stroke3 = Instance.new("UIStroke", BotaoprincipalText)
    stroke3.Thickness = 2

    ----botão do button
    local ButtonOfTheBotao = Instance.new("ImageButton", Botaoprincipal)
    ButtonOfTheBotao.Name = text.."Button"
    ButtonOfTheBotao.Size = UDim2.new(0.04, 0, 0.35, 0)
    ButtonOfTheBotao.Position = UDim2.new(0.9, 0, 0.33, 0)
    ButtonOfTheBotao.BackgroundTransparency = 0
    ButtonOfTheBotao.BackgroundColor3 = Color3.fromRGB(212,12,12)
    ButtonOfTheBotao.ImageTransparency = 1
    ButtonOfTheBotao.BorderSizePixel = 0
    Instance.new("UICorner", ButtonOfTheBotao).CornerRadius = UDim.new(0, 2)
    ButtonOfTheBotao.Localize = false

    local outerStroke = Instance.new("UIStroke", ButtonOfTheBotao)
    outerStroke.Color = Color3.fromRGB(0,0,0)
    outerStroke.Thickness = 2

    local inner = Instance.new("Frame", ButtonOfTheBotao)
    inner.Size = UDim2.new(1.5, 0, 1.5, 0)
    inner.Position = UDim2.new(-0.255, 0, -0.25, 0)
    inner.BackgroundTransparency = 1
    inner.BorderSizePixel = 0
    Instance.new("UICorner", inner).CornerRadius = UDim.new(0, 4)

    local innerStroke = Instance.new("UIStroke", inner)
    innerStroke.Color = Color3.fromRGB(212,12,12)
    innerStroke.Thickness = 2
    return Botaoprincipal
end

----notificações

local function showNotification(yt, text)
 
    -- Criar mini GUI de notificação
    local notificationGui = Instance.new("ScreenGui")
     notificationGui.Name = "NotificationGui"
     notificationGui.ResetOnSpawn = false
     notificationGui.Parent = player:WaitForChild("PlayerGui")
     
     local notificationFrame = Instance.new("Frame", notificationGui)
     notificationFrame.Name = "NotificationFrame"
     notificationFrame.Size = UDim2.new(0, 320, 0, yt)
     notificationFrame.AnchorPoint = Vector2.new(1, 1)
     notificationFrame.Position = UDim2.new(1, -10, 1, -10)
     notificationFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
     notificationFrame.BackgroundTransparency = 0.1
     notificationFrame.BorderSizePixel = 0
     Instance.new("UICorner", notificationFrame).CornerRadius = UDim.new(0, 5)
     
     local stroke3 = Instance.new("UIStroke", notificationFrame)
     stroke3.Color = Color3.fromRGB(212, 12, 12)
     stroke3.Thickness = 3
     stroke3.Transparency = 0

    local notificationText = Instance.new("TextLabel", notificationFrame)
    notificationText.Name = "NotificationText"
    notificationText.Size = UDim2.new(0, 250, 0, 40)
    notificationText.Position = UDim2.new(0, 60, 0, 9)
    notificationText.BackgroundTransparency = 1
    notificationText.TextColor3 = Color3.fromRGB(212, 12, 12)
    notificationText.Text = text
    notificationText.TextScaled = true
    notificationText.Font = Enum.Font.SourceSansBold
    notificationText.TextXAlignment = Enum.TextXAlignment.Center
    notificationText.TextYAlignment = Enum.TextYAlignment.Center

    local notificationicon = Instance.new("ImageLabel", notificationFrame)
    notificationicon.Name = "NotificationIcon"
    notificationicon.Size = UDim2.new(0, 50, 0, 50)
    notificationicon.Position = UDim2.new(0, 7, 0, 5)
    notificationicon.BackgroundTransparency = 1
    notificationicon.Image = "rbxassetid://129327299482883"
    notificationicon.BorderSizePixel = 0
    Instance.new("UICorner", notificationicon).CornerRadius = UDim.new(0, 10)
    notificationicon.ZIndex = 3


    wait(3)
    notificationGui:Destroy()
end

------ função dos botões de selecionar
local function Selectbutton(posicao, textLabel, yoptions, yposition, options)
     
    -----botão de selecionar
    local backgroundselect = Instance.new("Frame", posicao)
    backgroundselect.Size = UDim2.new(0.9, 0, 0.04, 0)
    backgroundselect.Position = UDim2.new(0.05, 0, yposition, 0)
    backgroundselect.BackgroundColor3 = Color3.fromRGB(0,0,0)
    backgroundselect.BorderSizePixel = 0
    Instance.new("UICorner", backgroundselect).CornerRadius = UDim.new(0, 4)
    backgroundselect.Localize = false


    local selectedLabel = Instance.new("TextLabel", backgroundselect)
    selectedLabel.Size = UDim2.new(1,-40,1,0)
    selectedLabel.Position = UDim2.new(0,0,0,0)
    selectedLabel.BackgroundTransparency = 1
    selectedLabel.Text = textLabel
    selectedLabel.TextColor3 = Color3.fromRGB(255,255,255)
    selectedLabel.TextXAlignment = Enum.TextXAlignment.Left
    selectedLabel.TextYAlignment = Enum.TextYAlignment.Center
    selectedLabel.BorderSizePixel = 0
    selectedLabel.FontFace = Font.new(
       "rbxasset://fonts/families/BuilderSans.json",
       Enum.FontWeight.ExtraBold,
       Enum.FontStyle.Normal
    )
    selectedLabel.Localize = false
    selectedLabel.ZIndex = 2
    selectedLabel.TextSize = 18

    local grad = Instance.new("UIGradient", selectedLabel)
    grad.Color = ColorSequence.new{
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

    local selectedLabelPadding = Instance.new("UIPadding", selectedLabel)
    selectedLabelPadding.PaddingLeft = UDim.new(0, 10)

    local Botaoselect = Instance.new("TextButton", backgroundselect)
    Botaoselect.Size = UDim2.new(0,40,1,0)
    Botaoselect.Position = UDim2.new(1,-40,0,0)
    Botaoselect.Text = "▼"
    Botaoselect.BackgroundColor3 = Color3.fromRGB(212, 12, 12)
    Botaoselect.TextColor3 = Color3.fromRGB(255,255,255)
    Botaoselect.BorderSizePixel = 0
    Instance.new("UICorner", Botaoselect).CornerRadius = UDim.new(0, 4)

    local grad = Instance.new("UIGradient", Botaoselect)
    grad.Color = ColorSequence.new{
      ColorSequenceKeypoint.new(0, Color3.fromRGB(212, 12, 12)),
      ColorSequenceKeypoint.new(0.4, Color3.fromRGB(255, 218, 27)),
      ColorSequenceKeypoint.new(0.7, Color3.fromRGB(255, 218, 27)),
      ColorSequenceKeypoint.new(0.9, Color3.fromRGB(212, 12, 12)),
       ColorSequenceKeypoint.new(1, Color3.fromRGB(212, 12, 12))
    }

    local backgroundoptions = Instance.new("ScrollingFrame", posicao)
    backgroundoptions.Size = UDim2.new(0.9, 0, yoptions, 0)
    backgroundoptions.Position = UDim2.new(0.05, 0, yposition + 0.05, 0)
    backgroundoptions.BackgroundColor3 = Color3.fromRGB(30,30,30)
    backgroundoptions.BorderSizePixel = 0
    Instance.new("UICorner", backgroundoptions).CornerRadius = UDim.new(0, 4)
    backgroundoptions.ClipsDescendants = true
    backgroundoptions.Visible = false
    backgroundoptions.ScrollBarThickness = 6
    backgroundoptions.ZIndex = 3

    local stroke = Instance.new("UIStroke", backgroundoptions)
    stroke.Color = Color3.fromRGB(255,255,255)
    stroke.Thickness = 2
    local grad = Instance.new("UIGradient", stroke)
    grad.Color = ColorSequence.new{
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
    	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

    local layout = Instance.new("UIListLayout", backgroundoptions)
    layout.SortOrder = Enum.SortOrder.LayoutOrder

    local selectedOption

    local function updateSelection(button)
        selectedOption = button.Text
        selectedLabel.Text = selectedOption
     for _, child in pairs(backgroundoptions:GetChildren()) do
         if child:IsA("TextButton") then
                child.TextColor3 = Color3.fromRGB(255,255,255)
                local grad = Instance.new("UIGradient", child)
                  grad.Color = ColorSequence.new{
                      ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
                      ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
                      ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
                      ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
                      ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
                      ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}
                child.BackgroundColor3 = Color3.fromRGB(0,0,0)
                child.Localize = false
                child.BackgroundTransparency = 0.2
                child.FontFace = Font.new(
                "rbxasset://fonts/families/BuilderSans.json",
                Enum.FontWeight.ExtraBold,
                Enum.FontStyle.Normal
                )
                child.TextSize = 18
                child.TextXAlignment = Enum.TextXAlignment.Center
                Instance.new("UICorner", child).CornerRadius = UDim.new(0, 4)
            end
        end
     button.TextColor3 = Color3.fromRGB(0,0,0)
     button.BackgroundColor3 = Color3.fromRGB(255,255,255)
      local grad = Instance.new("UIGradient", button)
      grad.Color = ColorSequence.new{ 
         ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
         ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
         ColorSequenceKeypoint.new(0.4, Color3.fromRGB(160, 120, 30)),
         ColorSequenceKeypoint.new(0.8, Color3.fromRGB(255, 218, 27)),
         ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
         ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 218, 27))}
      backgroundoptions.BackgroundTransparency = 0
      backgroundoptions.Visible = false
      button.Localize = false
      Instance.new("UICorner", button).CornerRadius = UDim.new(0, 4)
    end

    for i, name in ipairs(options) do
      local optionBtn = Instance.new("TextButton", backgroundoptions)
      optionBtn.Size = UDim2.new(1,0,0,30)
      optionBtn.Position = UDim2.new(0.1, 0, 0, 0)
     optionBtn.BackgroundColor3 = Color3.fromRGB(0,0,0)
     optionBtn.BorderSizePixel = 0
     optionBtn.TextColor3 = Color3.fromRGB(255,255,255)
      optionBtn.FontFace = Font.new(
         "rbxasset://fonts/families/BuilderSans.json",
         Enum.FontWeight.ExtraBold,
         Enum.FontStyle.Normal
      )
      optionBtn.TextSize = 18
     optionBtn.Text = name
     optionBtn.Localize = false
     Instance.new("UICorner", optionBtn).CornerRadius = UDim.new(0, 4)
     optionBtn.TextXAlignment = Enum.TextXAlignment.Center
     optionBtn.TextYAlignment = Enum.TextYAlignment.Center
     optionBtn.Position = UDim2.new(0,10,0,0)
     optionBtn.Name = "Option"..i
     local grad = Instance.new("UIGradient", optionBtn)
     grad.Color = ColorSequence.new{
         ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
         ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
         ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
         ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
         ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
         ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}
     optionBtn.ZIndex = 3

     optionBtn.MouseButton1Click:Connect(function()
         updateSelection(optionBtn)
      end)
    end

    Botaoselect.MouseButton1Click:Connect(function()
       backgroundoptions.Visible = not backgroundoptions.Visible
      backgroundoptions.CanvasSize = UDim2.new(0, 0, 0, #options * 30)
      if backgroundoptions.Visible then
          backgroundoptions.CanvasPosition = Vector2.new(0, 0)
        end
    end)
    return backgroundselect, backgroundoptions, selectedLabel
end

-- Area onde ficam as Tabs
local contend = Instance.new("Frame", contentArea)
contend.Name = "Content"
contend.Size = UDim2.new(1, 0, 1, 0)
contend.BackgroundTransparency = 1
contend.BorderSizePixel = 0

------ Scroll Das Tabs
local function createScrollArea(text, Canvasy)
    local ScrollTabs = Instance.new("ScrollingFrame", contend)
    ScrollTabs.Name = text
    ScrollTabs.Size = UDim2.new(0.9, 0, 0.9, 0)
    ScrollTabs.Position = UDim2.new(0.05, 0, 0.05, 0)
    ScrollTabs.BackgroundColor3 = Color3.fromRGB(212,12,12)
    ScrollTabs.BackgroundTransparency = 0.4
    ScrollTabs.BorderSizePixel = 0
    ScrollTabs.ZIndex = 1
    Instance.new("UICorner", ScrollTabs).CornerRadius = UDim.new(0, 4)
    ScrollTabs.ScrollBarThickness = 8
    ScrollTabs.CanvasSize = UDim2.new(0, 0, 0, Canvasy)
    ScrollTabs.Visible = false
    return ScrollTabs
end

----- Scroll da Tab Discord
local ScrollDiscord = createScrollArea("Discord", 0)

-- titulo do menu discord
local discordTitle = createPageTitle("Discord", 0.05, 0.2, 1, ScrollDiscord)

---menu discord
local link = Instance.new("Frame", ScrollDiscord)
link.Name = "Link"
link.Size = UDim2.new(0, 330, 0, 162)
link.Position = UDim2.new(0.05, 0, 0.2, 0)
link.BackgroundTransparency = 0
link.BackgroundColor3 = Color3.fromRGB(0,0,0)
link.BorderSizePixel = 0
Instance.new("UICorner", link).CornerRadius = UDim.new(0, 4)
link.Localize = false
link.ZIndex = 2

local discordLink = Instance.new("TextButton", link)
discordLink.Name = "DiscordLink"
discordLink.Size = UDim2.new(0, 270, 0, 30)
discordLink.BackgroundTransparency = 1
discordLink.TextColor3 = Color3.fromRGB(212, 12, 12)
discordLink.Text = "https://discord.gg/Nfh5Sczyqz"
discordLink.TextScaled = false
discordLink.BorderSizePixel = 0
discordLink.TextTransparency = 0
Instance.new("UICorner", discordLink).CornerRadius = UDim.new(0, 4)
discordLink.FontFace = Font.new(
    "rbxasset://fonts/families/BuilderSans.json",
    Enum.FontWeight.ExtraBold,
    Enum.FontStyle.Normal
)
discordLink.Localize = false
discordLink.ZIndex = 3
discordLink.TextSize = 20

-----imagem do servidor
local imagelink = Instance.new("ImageButton", link)
imagelink.Name = "ToggleButton"
imagelink.Size = UDim2.new(0,75,0,75)
imagelink.Position = UDim2.new(0,10,0,30)
imagelink.Image = "rbxassetid://129327299482883" -- troque pelo ID do seu arquivo (pode ser o mesmo da logo)
imagelink.BackgroundTransparency = 1
imagelink.BorderSizePixel = 0
Instance.new("UICorner", imagelink).CornerRadius = UDim.new(0, 10)
imagelink.ZIndex = 3

---nome do servidor
local serverName = Instance.new("TextLabel", link)
serverName.Name = "ServerName"
serverName.Size = UDim2.new(0, 270, 0, 30)
serverName.Position = UDim2.new(0,70,0,53)
serverName.BackgroundTransparency = 1
serverName.TextColor3 = Color3.fromRGB(255, 255, 255)
serverName.Text = "Drakion hub ┃ community"
serverName.TextScaled = false
serverName.BorderSizePixel = 0
serverName.TextTransparency = 0
Instance.new("UICorner", serverName).CornerRadius = UDim.new(0, 4)
serverName.FontFace = Font.new(
    "rbxasset://fonts/families/BuilderSans.json",
    Enum.FontWeight.ExtraBold,
    Enum.FontStyle.Normal
)
serverName.Localize = false
serverName.ZIndex = 3
serverName.TextSize = 20

local stroke3 = Instance.new("UIStroke", serverName)
stroke3.Thickness = 3
stroke3.Transparency = 0

-- gradiente dourado:
local grad = Instance.new("UIGradient", serverName)
grad.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
	ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

--- botão de abrir o discord
local openDiscordBtn = Instance.new("TextButton", link)
openDiscordBtn.Name = "OpenDiscord"
openDiscordBtn.Size = UDim2.new(0, 290, 0, 37)
openDiscordBtn.Position = UDim2.new(0, 20, 0, 110)
openDiscordBtn.BackgroundTransparency = 0
openDiscordBtn.BackgroundColor3 = Color3.fromRGB(212,12,12)
openDiscordBtn.BorderSizePixel = 0
openDiscordBtn.TextColor3 = Color3.fromRGB(255,255,255)
openDiscordBtn.Text = "Join Discord"
Instance.new("UICorner", openDiscordBtn).CornerRadius = UDim.new(0, 4)
openDiscordBtn.Localize = false
openDiscordBtn.ZIndex = 3
openDiscordBtn.FontFace = Font.new(
    "rbxasset://fonts/families/BuilderSans.json",
    Enum.FontWeight.ExtraBold,
    Enum.FontStyle.Normal
)
openDiscordBtn.TextSize = 20

 openDiscordBtn.MouseButton1Click:Connect(function()
    setclipboard("https://discord.gg/Nfh5Sczyqz")
    showNotification(60, "Text copied to clipboard!")
 end)
 discordLink.MouseButton1Click:Connect(function()
    setclipboard("https://discord.gg/Nfh5Sczyqz")
    showNotification(60, "Text copied to clipboard!")
 end)

----Rodapé
local RodapeDiscord = createFooter(0.84, 0.865, 0.89, 0.02, ScrollDiscord)

-- register page
pages["Discord"] = ScrollDiscord
if discordBtn then
    discordBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollDiscord)
        pageHeader.Text = "discord.gg/Nfh5Sczyqz"
        pageHeader.FontFace = Font.new(
         "rbxasset://fonts/families/BuilderSans.json",
          Enum.FontWeight.ExtraBold,
          Enum.FontStyle.Normal
          )
        pageHeader.TextSize = 20
        pageHeader.Localize = false
    end)
end

----scroll area main farm
local ScrollFarm = createScrollArea("Main Farm", 1000)

-- titulo do menu farm
local MainFarmTitle = createPageTitle("Main Farm", -0.02, 0.05, 0.65, ScrollFarm)
---- menu Farm

---Farm Level
 local FarmLevel = createButton("Farm Level", 0.065, ScrollFarm)
--- farm bones
local FarmBones = createButton("Farm Bones", 0.11, ScrollFarm)
----farm katakuri
local FarmKatakuri = createButton("Farm Katakuri", 0.155, ScrollFarm)
----farm Tyrant of the Skies
local FarmTyrant = createButton("Farm Tyrant of the Skies", 0.2, ScrollFarm)
----farm Nearest
local FarmNearest = createButton("Farm Nearest", 0.245, ScrollFarm)

----Titulo do menu materiais
local MaterialsTitle = createPageTitle("Materials", 0.260, 0.05, 0.65, ScrollFarm)
-----menu materiais
------select materials
local SelectMaterials = Selectbutton(ScrollFarm, "Select Material...",  0.13, 0.345, {"Vampire Fang","Fish Tail","Gunpowder","Mystic Droplet","Conjured Cocoa","Dragon Scale","Leather","Ectoplasm","Mini Tusk","Magma Ore","Scrap Metal","Angel Wings","Radioactive Material","Demonic Wisp"})

----button active Auto Materials
local AutoMaterials = createButton("Auto Materials", 0.395, ScrollFarm)

--- titulo do menu maestria
local MaestriaTitle = createPageTitle("Maestria", 0.41, 0.05, 0.65, ScrollFarm)
---- menu maestria

-----Select Method Maestria
local MethodMaestria = Selectbutton(ScrollFarm, "Method Maestria...", 0.09, 0.5, {"Sword","Blox Fruit","Gun"})

----Rolagem de porcentagem de vida para Farm maestria

local ScrollMaestria = Instance.new("Frame", ScrollFarm)
ScrollMaestria.Size = UDim2.new(0.9, 0, 0.1, 0)
ScrollMaestria.Position = UDim2.new(0.05, 0, 0, 550)
ScrollMaestria.BackgroundColor3 = Color3.fromRGB(0,0,0)
ScrollMaestria.BackgroundTransparency = 0
ScrollMaestria.BorderSizePixel = 0
Instance.new("UICorner", ScrollMaestria).CornerRadius = UDim.new(0, 4)
ScrollMaestria.Localize = false

local FrameTextHealth = Instance.new("Frame", ScrollMaestria)
FrameTextHealth.Size = UDim2.new(0.4, 0, 0.4, 0)
FrameTextHealth.Position = UDim2.new(0.03, 0, 0, 10)
FrameTextHealth.BackgroundColor3 = Color3.fromRGB(20,20,20)
FrameTextHealth.BackgroundTransparency = 0
FrameTextHealth.BorderSizePixel = 0
Instance.new("UICorner", FrameTextHealth).CornerRadius = UDim.new(0, 4)

local TextHealth = Instance.new("TextLabel", FrameTextHealth)
TextHealth.Size = UDim2.new(0, 100, 0, 100)
TextHealth.Position = UDim2.new(0, -20, 0, -30)
TextHealth.BackgroundTransparency = 1
TextHealth.Text = "Health:"
TextHealth.TextColor3 = Color3.fromRGB(255, 255, 255)
TextHealth.TextSize = 18
TextHealth.Font = Enum.Font.SourceSansBold
TextHealth.TextYAlignment = Enum.TextYAlignment.Center

local grad = Instance.new("UIGradient", TextHealth)
grad.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 244, 200)),
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 244, 200))}

local backgroundscrolling = Instance.new("Frame", ScrollFarm)
backgroundscrolling.Size = UDim2.new(0.85, 0, 0, 10)
backgroundscrolling.Position = UDim2.new(0, 27.5, 0, 610)
backgroundscrolling.BackgroundColor3 = Color3.new(0.2,0.2,0.2)
backgroundscrolling.BorderSizePixel = 0
backgroundscrolling.ClipsDescendants = true
backgroundscrolling.Active = true
Instance.new("UICorner", backgroundscrolling).CornerRadius = UDim.new(0, 4)

local frontscroll = Instance.new("Frame", backgroundscrolling)
frontscroll.Size = UDim2.new(0, 0, 1, 0)
frontscroll.BackgroundColor3 = Color3.fromRGB(212,12,12)
frontscroll.BorderSizePixel = 0
Instance.new("UICorner", frontscroll).CornerRadius = UDim.new(0, 4)

local buttonScroll = Instance.new("ImageButton", backgroundscrolling)
buttonScroll.Size = UDim2.new(0, 10, 0, 10)
buttonScroll.Position = UDim2.new(0, 0, 0, 0)
buttonScroll.BackgroundColor3 = Color3.fromRGB(255,255,255)
buttonScroll.BorderSizePixel = 0
buttonScroll.ZIndex = 2
buttonScroll.AutoButtonColor = false
Instance.new("UICorner", buttonScroll).CornerRadius = UDim.new(0, 8)

local valuescroll = Instance.new("TextLabel", ScrollFarm)
valuescroll.Position = UDim2.new(0, 100, 0, 565)
valuescroll.Size = UDim2.new(0, 40, 0, 31)
valuescroll.Text = "0%"
valuescroll.TextColor3 = Color3.fromRGB(255,255,255)
valuescroll.BackgroundTransparency = 1
valuescroll.Font = Enum.Font.SourceSansBold
valuescroll.TextSize = 20

local grad = Instance.new("UIGradient", valuescroll)
grad.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 218, 27)),
    ColorSequenceKeypoint.new(0.05, Color3.fromRGB(160, 120, 30)),
    ColorSequenceKeypoint.new(0.2, Color3.fromRGB(160, 120, 30)),
    ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 218, 27)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 218, 27))}

local minValue, maxValue = 0, 1036
local dragging = false

local function setSliderFromX(px)
    local left = backgroundscrolling.AbsolutePosition.X
    local width = backgroundscrolling.AbsoluteSize.X
    local ratio = math.clamp((px - left) / width, 0, 0.965)
    frontscroll.Size = UDim2.new(ratio + 0.01, 0, 1, 0)
    buttonScroll.Position = UDim2.new(ratio, 0, 0, 0)
    local value = math.floor(minValue + ratio * (maxValue - minValue) + 0.5)
    valuescroll.Text = tostring(value / 10) .. "%"
    return value
end

buttonScroll.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)

buttonScroll.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

backgroundscrolling.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        setSliderFromX(input.Position.X)
    end
end)

backgroundscrolling.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        setSliderFromX(input.Position.X)
    end
end)

----Auto Maestria
local AutoMaestria = createButton("Auto Maestria", 0.66, ScrollFarm)

--- Titulo do menu status
local StatusTitle = createPageTitle("Status", 0.675, 0.05, 0.65, ScrollFarm)
----menu status

-----Select Method Status
local MethodStatus = Selectbutton(ScrollFarm, "Method Status...", 0.15, 0.765, {"Melee","Defense","Sword","Gun","Blox Fruit"})

----Auto Status
local AutoStatusBtn = createButton("Auto Status", 0.815, ScrollFarm)

----Rodapé
local Rodapemainfarm = createFooter(0.96, 0.964, 0.97, 0.005, ScrollFarm)

-- register page and setup click to show it
pages["Main Farm"] = ScrollFarm
if mainFarmBtn then
    mainFarmBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollFarm)
        pageHeader.Text = "Main Farm"
    end)
end

----scroll area Itens Quest
local ScrollItensQuest = createScrollArea("Itens Quest", 1000)

-- titulo do menu itens quest
local itensQuestTitle = createPageTitle("Itens Quest", 0.05, 0.05, 0.65, ScrollItensQuest)

-- register page
pages["ItensQuest"] = ScrollItensQuest
if itensQuestBtn then
    itensQuestBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollItensQuest)
    end)
end

---- scroll area Farm Outher
local ScrollFarmOuther = createScrollArea("Farm Outher", 1000)

-- Titulo do menu Farm Outher
local FarmOutherTitle = createPageTitle("Farm Outher", 0.05, 0.05, 0.65, ScrollFarmOuther)

-- register page
pages["FarmOuther"] = ScrollFarmOuther
if FarmOutherBtn then
    FarmOutherBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollFarmOuther)
    end)
end

---- scroll area Dungeon and Raid
local ScrollDungeonRaid = createScrollArea("Dungeon and Raid", 1000)

-- Menu Dungeon and Raid
local dungeonRaidTitle = createPageTitle("Dungeon and Raid", 0.05, 0.05, 0.65, ScrollDungeonRaid)

-- register page
pages["DungeonRaid"] = ScrollDungeonRaid

if dungeonRaidBtn then
    dungeonRaidBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollDungeonRaid)
    end)
end

---- scroll area Teleport
local ScrollTeleport = createScrollArea("Teleport", 1000)

-- Menu Teleport
local teleportTitle = createPageTitle("Teleport", 0.05, 0.05, 0.65, ScrollTeleport)

-- register page
pages["Teleport"] = ScrollTeleport
if teleportBtn then
    teleportBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollTeleport)
    end)
end

---- scroll area Fruits
local ScrollFruits = createScrollArea("Fruits", 1000)

-- Menu Fruits
local fruitsTitle = createPageTitle("Fruits", 0.05, 0.05, 0.65, ScrollFruits)

-- register page
pages["Fruits"] = ScrollFruits
if fruitsBtn then
    fruitsBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollFruits)
    end)
end

---- scroll area Shop
local ScrollShop = createScrollArea("Shop", 1000)

----Menu Shop
local shopTitle = createPageTitle("Shop", 0.05, 0.05, 0.65, ScrollShop)

-- register page
pages["Shop"] = ScrollShop
if shopBtn then
    shopBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollShop)
    end)
end

---- scroll area Setting
local ScrollSetting = createScrollArea("Setting", 1000)

-- Menu Setting
local settingTitle = createPageTitle("Setting", 0.05, 0.05, 0.65, ScrollSetting)

-- register page
pages["Setting"] = ScrollSetting
if settingBtn then
    settingBtn.MouseButton1Click:Connect(function()
        showOnly(ScrollSetting)
    end)
end

-- start with no page shown
for _, v in pairs(pages) do
    if v then
        v.Visible = false
    end
end
showOnly(ScrollDiscord)
pageHeader.Text = "discord.gg/Nfh5Sczyqz"
        pageHeader.FontFace = Font.new(
         "rbxasset://fonts/families/BuilderSans.json",
          Enum.FontWeight.ExtraBold,
          Enum.FontStyle.Normal
          )
        pageHeader.TextSize = 20
        pageHeader.Localize = false

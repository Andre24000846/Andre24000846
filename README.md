-- Cria uma tela GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Cria um frame para o menu
local MenuFrame = Instance.new("Frame")
MenuFrame.Size = UDim2.new(0.5, 0, 0.5, 0) -- Tamanho 50% da tela
MenuFrame.Position = UDim2.new(0.25, 0, 0.25, 0) -- Centraliza o menu na tela
MenuFrame.BackgroundColor3 = Color3.new(0, 0, 0) -- Cor preta
MenuFrame.BackgroundTransparency = 0.6 -- 60% transparente
MenuFrame.Parent = ScreenGui

-- Função para tornar o frame arrastável
local function makeDraggable(frame)
    local dragging
    local dragInput
    local dragStart
    local startPos

    local function update(input)
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position

            -- Desativa o controle da câmera
            game.Players.LocalPlayer.PlayerScripts.CameraScript.Disabled = true

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                    -- Reativa o controle da câmera
                    game.Players.LocalPlayer.PlayerScripts.CameraScript.Disabled = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end

-- Torna o menu arrastável
makeDraggable(MenuFrame)

-- Cria um botão para fechar o menu
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 50, 0, 50)
CloseButton.Position = UDim2.new(1, -50, 0, 0)
CloseButton.Text = "X"
CloseButton.Parent = MenuFrame

-- Cria o ícone "👀" que aparece quando o menu é fechado
local EyeIcon = Instance.new("TextButton")
EyeIcon.Size = UDim2.new(0, 70, 0, 70) -- Aumentado o tamanho do ícone
EyeIcon.Position = UDim2.new(0, 0, 0, 0)
EyeIcon.Text = "👀"
EyeIcon.Visible = false
EyeIcon.Parent = ScreenGui

-- Função para animar a reaparição do menu
local function animateMenu()
    MenuFrame:TweenPosition(UDim2.new(0.25, 0, 0.25, 0), "Out", "Quad", 0.5, true)
end

-- Função para fechar o menu
CloseButton.MouseButton1Click:Connect(function()
    MenuFrame.Visible = false
    EyeIcon.Visible = true
end)

-- Função para reabrir o menu
EyeIcon.MouseButton1Click:Connect(function()
    MenuFrame.Visible = true
    EyeIcon.Visible = false
    animateMenu()
end)

-- Torna o ícone "👀" arrastável
makeDraggable(EyeIcon)

-- Adiciona o texto "☔andre hub☔" no canto superior esquerdo do menu
local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(0, 200, 0, 50)
TitleText.Position = UDim2.new(0, 10, 0, 10)
TitleText.Text = "☔andre hub☔"
TitleText.TextColor3 = Color3.new(1, 1, 1)
TitleText.BackgroundTransparency = 1
TitleText.Parent = MenuFrame

-- Cria um frame brilhante ao redor do texto
local BorderFrame = Instance.new("Frame")
BorderFrame.Size = UDim2.new(1, 0, 1, 0) -- Ajustado para o tamanho do texto
BorderFrame.Position = UDim2.new(0, 0, 0, 0)
BorderFrame.BackgroundTransparency = 0 -- Sem transparência
BorderFrame.Parent = TitleText

-- Função para mudar a cor do frame brilhante
local function changeBorderColor()
    while true do
        BorderFrame.BackgroundColor3 = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
        wait(0.5)
    end
end

-- Inicia a mudança de cor do frame brilhante
spawn(changeBorderColor)

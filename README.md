-- Definir o serviço de Players
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Função para criar uma etiqueta de nome para o jogador
local function createESP(player)
    -- Verifica se o personagem do jogador existe
    local character = player.Character
    if not character then return end

    -- Cria o BillboardGui (GUI de rótulo) para o nome
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = character:WaitForChild("Head") -- Adiciona o nome sobre a cabeça do jogador
    billboardGui.Size = UDim2.new(0, 100, 0, 50) -- Define o tamanho do rótulo
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)  -- Posiciona o nome um pouco acima da cabeça

    -- Cria o TextLabel para exibir o nome
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = billboardGui
    textLabel.Text = player.Name -- Define o texto como o nome do jogador
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)  -- Cor branca
    textLabel.TextSize = 20  -- Tamanho do texto
    textLabel.BackgroundTransparency = 1  -- Torna o fundo transparente
    textLabel.TextStrokeTransparency = 0.5  -- Adiciona uma borda suave ao texto

    -- Adiciona a GUI ao CoreGui para garantir que ela será exibida na interface
    billboardGui.Parent = game.CoreGui
end

-- Função para atualizar os ESPs enquanto o jogo está em andamento
RunService.Heartbeat:Connect(function()
    for _, player in ipairs(Players:GetPlayers()) do
        -- Evita que a ESP seja criada para o próprio jogador
        if player ~= Players.LocalPlayer then
            -- Verifica se o jogador já possui o ESP (marcado com um BoolValue)
            if not player:FindFirstChild("ESP") then
                -- Cria o ESP para o jogador
                createESP(player)
                
                -- Marca o jogador para saber que o ESP foi criado
                local espTag = Instance.new("BoolValue")
                espTag.Name = "ESP"
                espTag.Parent = player
            end
        end
    end
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = game.Workspace.CurrentCamera

-- Função para criar ESP
local function createESP(character, color)
    local highlight = Instance.new("Highlight")
    highlight.Parent = character
    highlight.FillColor = color
    highlight.OutlineColor = Color3.new(0, 0, 0) -- Cor da borda
    highlight.OutlineTransparency = 0.5
end

-- Função para atualizar ESP dos jogadores
local function updateESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local color = (player.Team == LocalPlayer.Team) and Color3.new(0, 0, 1) or Color3.new(1, 0, 0)
            createESP(player.Character, color)
        end
    end
end

-- Conectar eventos
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        updateESP()
    end)
end)

Players.PlayerRemoving:Connect(function(player)
    if player.Character then
        local highlight = player.Character:FindFirstChildOfClass("Highlight")
        if highlight then
            highlight:Destroy()
        end
    end
end)

-- Inicializar o ESP para jogadores existentes
updateESP()

-- Loop para atualizar o ESP continuamente
while true do
    updateESP()
    wait(1) -- Atualiza a cada segundo
end

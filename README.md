local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local espEnabled = false

-- Função para ativar/desativar o ESP
local function toggleESP()
    espEnabled = true  -- Ativar o ESP automaticamente

    -- Caso o ESP esteja ativado
    if espEnabled then
        for _, otherPlayer in pairs(game.Players:GetPlayers()) do
            if otherPlayer ~= player and otherPlayer.Character then
                local otherCharacter = otherPlayer.Character
                local humanoidRootPart = otherCharacter:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    -- Criar uma linha que conecta o jogador com o outro
                    local line = Instance.new("LineHandleAdornment")
                    line.Parent = otherCharacter
                    line.Adornee = humanoidRootPart
                    line.Color3 = Color3.fromRGB(255, 0, 0) -- Cor da linha
                    line.Thickness = 0.2 -- Espessura da linha
                    line.ZIndex = 10 -- Garante que a linha fique acima do personagem
                    line.Visible = true
                    line.Name = "ESP_Line"
                    otherCharacter:SetAttribute("ESP_Line", line) -- Armazenar a linha no atributo
                end
            end
        end
    end
end

-- Função para ativar a modificação das hitboxes
local function modifyHitboxes()
    -- Aumentar a hitbox do próprio jogador para 1 e deixá-la visível
    for _, v in pairs(character:GetChildren()) do
        if v:IsA("BasePart") then
            v.Size = Vector3.new(1, 1, 1)
            v.Transparency = 0 -- Torna a hitbox visível para o próprio jogador
        end
    end

    -- Definir hitbox dos outros jogadores como 1000 e invisível para eles
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player then
            local otherCharacter = otherPlayer.Character
            if otherCharacter then
                for _, v in pairs(otherCharacter:GetChildren()) do
                    if v:IsA("BasePart") then
                        v.Size = Vector3.new(1000, 1000, 1000)
                        v.Transparency = 1 -- Torna a hitbox invisível para os outros jogadores
                    end
                end
            end
        end
    end
end

-- Função para remover cooldown das habilidades
local function removeCooldown()
    for _, v in pairs(getgc(true)) do
        if type(v) == "function" and islclosure(v) then
            local constants = debug.getconstants(v)
            for _, c in pairs(constants) do
                if type(c) == "number" and c > 0 then -- Verifica se há tempos de recarga
                    debug.setupvalue(v, 1, 0) -- Define o cooldown como zero
                end
            end
        end
    end
end

-- Função para alterar a velocidade
local function changeSpeed()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 65 -- Define a velocidade de movimento para 65
    end
end

-- Função para alterar a vida do jogador para 1000000
local function setHealthToMax()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.Health = 1000000 -- Define a vida para 1.000.000
    end
end

-- Função para dar 100 de dano em todos os outros jogadores
local function dealDamageToAll()
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local otherCharacter = otherPlayer.Character
            local humanoid = otherCharacter:FindFirstChild("Humanoid")
            if humanoid then
                humanoid:TakeDamage(100) -- Dá 100 de dano
            end
        end
    end
end

-- Função para mostrar a notificação
local function showNotification()
    game.StarterGui:SetCore("SendNotification", {
        Title = "Aviso";
        Text = "Se inscreva no canal do @pedroanjoyt!";
        Duration = 60; -- Duração da notificação em segundos
    })
end

-- Executando as funções automaticamente

-- 1. Modificar a hitbox
modifyHitboxes()

-- 2. Remover cooldowns
removeCooldown()

-- 3. Alterar a velocidade para 65
changeSpeed()

-- 4. Alterar a vida do jogador para 1000000
setHealthToMax()

-- 5. Dar 100 de dano em todos os outros jogadores
dealDamageToAll()

-- 6. Ativar ESP (visibilidade dos jogadores)
toggleESP()

-- 7. Exibir a notificação
showNotification()

-- Mensagem inicial no console
print("Funções ativadas automaticamente.")

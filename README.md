--[[ Script Hub em Português - Blox Fruits ]]--

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Verifica se as variáveis globais estão definidas, se não, define padrão
if getgenv().TimeEscolhido == nil then getgenv().TimeEscolhido = "Piratas" end
if getgenv().AutoTeleport == nil then getgenv().AutoTeleport = true end
if getgenv().AutoFarm == nil then getgenv().AutoFarm = true end

print("Iniciando Script Hub - Blox Fruits")
print("Time escolhido: " .. getgenv().TimeEscolhido)
print("Auto Teleporte: " .. tostring(getgenv().AutoTeleport))
print("Auto Farm: " .. tostring(getgenv().AutoFarm))

-- Trocar de time automaticamente
if getgenv().TimeEscolhido == "Piratas" then
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetTeam", "Pirates")
    print("Você entrou para o time Piratas!")
elseif getgenv().TimeEscolhido == "Marinha" then
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetTeam", "Marines")
    print("Você entrou para o time Marinha!")
else
    print("Time inválido! Use 'Piratas' ou 'Marinha'.")
end

-- Função para teleportar para uma ilha
local function TeleportarPara(IlhaNome)
    local locais = {
        ["Início"] = CFrame.new(2067, 38, 902),
        ["Selva"] = CFrame.new(-1615, 36, 152),
        ["Deserto"] = CFrame.new(977, 7, 4400),
        ["Gelo"] = CFrame.new(4933, 16, -440),
        ["Ferro"] = CFrame.new(755, 7, 3011)
    }
    
    if locais[IlhaNome] then
        player.Character.HumanoidRootPart.CFrame = locais[IlhaNome]
        print("Teleportado para a ilha: " .. IlhaNome)
    else
        print("Ilha não encontrada: " .. IlhaNome)
    end
end

-- Teleporte automático para Selva (padrão)
if getgenv().AutoTeleport then
    wait(2)
    TeleportarPara("Selva")
end

-- Farm automático de NPCs
if getgenv().AutoFarm then
    print("Iniciando farm automático...")
    while true do
        wait(0.5)
        for _, v in pairs(game.Workspace.Enemies:GetChildren()) do
            if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
                pcall(function()
                    v.HumanoidRootPart.CanCollide = false
                    v.HumanoidRootPart.Size = Vector3.new(60,60,60)
                    player.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
                    game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                end)
            end
        end
    end
end

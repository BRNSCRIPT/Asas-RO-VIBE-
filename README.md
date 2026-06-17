local player = game.Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")

ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

ToggleButton.Name = "AsasToggle"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ToggleButton.BackgroundTransparency = 0.2
ToggleButton.Position = UDim2.new(0.05, 0, 0.4, 0)
ToggleButton.Size = UDim2.new(0, 140, 0, 50)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Text = "ASAS: ATIVO"
ToggleButton.TextColor3 = Color3.fromRGB(0, 255, 100)
ToggleButton.TextSize = 18.000

UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = ToggleButton

-- FunÃ§Ã£o para achar o RemoteEvent e avisar o servidor
local function avisarServidor(status)
    -- Procura o evento no Workspace baseado na estrutura que vocÃª passou
    local charFolder = game.Workspace:FindFirstChild("Carlostfra112", true)
    if charFolder then
        local flyFolder = charFolder:FindFirstChild("Fly", true)
        if flyFolder then
            local events = flyFolder:FindFirstChild("Events")
            if events then
                local activateFlight = events:FindFirstChild("ActivateFlight")
                if activateFlight then
                    activateFlight:FireServer(status) -- Aqui Ã© onde avisamos o servidor
                end
            end
        end
    end
end

local vooAtivado = true

ToggleButton.MouseButton1Click:Connect(function()
    vooAtivado = not vooAtivado
    
    if vooAtivado then
        -- Ativa novamente
        avisarServidor(true)
        ToggleButton.Text = "ASAS: ATIVO"
        ToggleButton.TextColor3 = Color3.fromRGB(0, 255, 100)
    else
        -- Desativa e avisa o servidor que Ã© para parar (false)
        avisarServidor(false)
        
        -- Reset local para garantir que vocÃª possa andar
        local hum = player.Character and player.Character:FindFirstChild("Humanoid")
        if hum then
            hum.Sit = false
            hum:ChangeState(Enum.HumanoidStateType.Running)
        end
        
        ToggleButton.Text = "ASAS: ANDAR"
        ToggleButton.TextColor3 = Color3.fromRGB(255, 50, 50)
    end
end)

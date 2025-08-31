local Players = game:GetService("Players") local UserInputService = game:GetService("UserInputService") local TweenService = game:GetService("TweenService") local player = Players.LocalPlayer local character = player.Character or player.CharacterAdded:Wait() local humanoid = character:WaitForChild("Humanoid")

local gui = Instance.new("ScreenGui") gui.Name = "BeastHub" gui.Parent = player:WaitForChild("PlayerGui")

local floatButton = Instance.new("TextButton") floatButton.Size = UDim2.new(0, 50, 0, 50) floatButton.Position = UDim2.new(0.05, 0, 0.4, 0) floatButton.Text = "≡" floatButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0) floatButton.TextScaled = true floatButton.Draggable = true floatButton.Parent = gui

local panel = Instance.new("Frame") panel.Size = UDim2.new(0, 250, 0, 300) panel.Position = UDim2.new(0.1, 0, 0.2, 0) panel.BackgroundColor3 = Color3.fromRGB(20, 20, 20) panel.Visible = false panel.Parent = gui

local title = Instance.new("TextLabel") title.Text = "Beast Hub" title.Size = UDim2.new(1, 0, 0, 40) title.BackgroundColor3 = Color3.fromRGB(0, 100, 0) title.TextColor3 = Color3.fromRGB(255, 255, 255) title.TextScaled = true title.Parent = panel

local flying = false local teleport = false

local flyButton = Instance.new("TextButton") flyButton.Size = UDim2.new(0.9, 0, 0, 40) flyButton.Position = UDim2.new(0.05, 0, 0.2, 0) flyButton.Text = "Ativar Voo" flyButton.BackgroundColor3 = Color3.fromRGB(0, 150, 0) flyButton.TextColor3 = Color3.fromRGB(255, 255, 255) flyButton.Parent = panel

local speedLabel = Instance.new("TextLabel") speedLabel.Size = UDim2.new(0.9, 0, 0, 20) speedLabel.Position = UDim2.new(0.05, 0, 0.35, 0) speedLabel.Text = "Velocidade" speedLabel.TextColor3 = Color3.fromRGB(255,255,255) speedLabel.BackgroundTransparency = 1 speedLabel.Parent = panel

local speedSlider = Instance.new("TextBox") speedSlider.Size = UDim2.new(0.9, 0, 0, 30) speedSlider.Position = UDim2.new(0.05, 0, 0.4, 0) speedSlider.PlaceholderText = "Digite até 16000" speedSlider.Text = "" speedSlider.Parent = panel

local jumpLabel = Instance.new("TextLabel") jumpLabel.Size = UDim2.new(0.9, 0, 0, 20) jumpLabel.Position = UDim2.new(0.05, 0, 0.55, 0) jumpLabel.Text = "Super Pulo" jumpLabel.TextColor3 = Color3.fromRGB(255,255,255) jumpLabel.BackgroundTransparency = 1 jumpLabel.Parent = panel

local jumpSlider = Instance.new("TextBox") jumpSlider.Size = UDim2.new(0.9, 0, 0, 30) jumpSlider.Position = UDim2.new(0.05, 0, 0.6, 0) jumpSlider.PlaceholderText = "Digite até 16000" jumpSlider.Text = "" jumpSlider.Parent = panel

local tpButton = Instance.new("TextButton") tpButton.Size = UDim2.new(0.9, 0, 0, 40) tpButton.Position = UDim2.new(0.05, 0, 0.75, 0) tpButton.Text = "Teleporte Click" tpButton.BackgroundColor3 = Color3.fromRGB(0, 150, 0) tpButton.TextColor3 = Color3.fromRGB(255, 255, 255) tpButton.Parent = panel

floatButton.MouseButton1Click:Connect(function() panel.Visible = not panel.Visible end)

local bodyVel, bodyGyro flyButton.MouseButton1Click:Connect(function() flying = not flying if flying then flyButton.Text = "Desativar Voo" bodyVel = Instance.new("BodyVelocity") bodyVel.MaxForce = Vector3.new(4000,4000,4000) bodyVel.Velocity = Vector3.new(0,0,0) bodyVel.Parent = character.PrimaryPart

bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(4000,4000,4000)
    bodyGyro.CFrame = character.PrimaryPart.CFrame
    bodyGyro.Parent = character.PrimaryPart

    game:GetService("RunService").RenderStepped:Connect(function()
        if flying then
            local move = Vector3.new()
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                move = move + (workspace.CurrentCamera.CFrame.LookVector)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                move = move - (workspace.CurrentCamera.CFrame.LookVector)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                move = move - (workspace.CurrentCamera.CFrame.RightVector)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                move = move + (workspace.CurrentCamera.CFrame.RightVector)
            end
            bodyVel.Velocity = move * 100
            bodyGyro.CFrame = workspace.CurrentCamera.CFrame
        end
    end)
else
    flyButton.Text = "Ativar Voo"
    if bodyVel then bodyVel:Destroy() end
    if bodyGyro then bodyGyro:Destroy() end
end

end)

speedSlider.FocusLost:Connect(function() local val = tonumber(speedSlider.Text) if val and val > 0 and val <= 16000 then humanoid.WalkSpeed = val end end)

jumpSlider.FocusLost:Connect(function() local val = tonumber(jumpSlider.Text) if val and val > 0 and val <= 16000 then humanoid.UseJumpPower = true humanoid.JumpPower = val end end)

local mouse = player:GetMouse() tpButton.MouseButton1Click:Connect(function() teleport = not teleport tpButton.Text = teleport and "Teleporte: ON" or "Teleporte: OFF" end)

mouse.Button1Down:Connect(function() if teleport then if mouse.Target then character:MoveTo(mouse.Hit.p + Vector3.new(0,3,0)) end end end)

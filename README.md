local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local Workspace = game:GetService("Workspace")

-- Certifique-se que o local de spawn tem o nome certo
local spawnLocation = Workspace:FindFirstChild("SpawnLocation") or Workspace:FindFirstChild("Spawn")

-- Garante que existe um ScreenGui
local gui = player:WaitForChild("PlayerGui"):FindFirstChild("FlyGui")
if not gui then
	gui = Instance.new("ScreenGui")
	gui.Name = "FlyGui"
	gui.ResetOnSpawn = false
	gui.Parent = player:WaitForChild("PlayerGui")
end

-- Cria o botão
local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0, 120, 0, 50)
flyButton.Position = UDim2.new(1, -130, 0.5, -25) -- Lateral direita
flyButton.Text = "Voar ao Spawn"
flyButton.BackgroundColor3 = Color3.fromRGB(30, 144, 255)
flyButton.TextColor3 = Color3.new(1, 1, 1)
flyButton.Font = Enum.Font.SourceSansBold
flyButton.TextSize = 20
flyButton.Parent = gui

-- Lógica de voo
local flying = false
flyButton.MouseButton1Click:Connect(function()
	if flying then return end
	if not spawnLocation then
		warn("Spawn não encontrado!")
		return
	end

	flying = true

	local goalPos = spawnLocation.Position + Vector3.new(0, 60, 0)
	local direction = (goalPos - hrp.Position).Unit
	local speed = 60
	local distance = (goalPos - hrp.Position).Magnitude
	local duration = distance / speed

	local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Linear)
	local tween = TweenService:Create(hrp, tweenInfo, {CFrame = CFrame.new(goalPos)})
	tween:Play()

	tween.Completed:Connect(function()
		flying = false
	end)
end)

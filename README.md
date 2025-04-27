local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local camera = Workspace.CurrentCamera

local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "ParadoxianLagGui"

local center = UDim2.new(0.5, 0, 0.5, 0)
local radius = 150
local imageId = "rbxassetid://73869234666153"
local crossId = "rbxassetid://6031068431"

local imageLabels = {}
local numImages = 10

-- Imagens em círculo
for i = 1, numImages do
	local image = Instance.new("ImageLabel")
	image.Image = imageId
	image.Size = UDim2.new(0, 50, 0, 50)
	image.BackgroundTransparency = 1
	image.Parent = screenGui
	table.insert(imageLabels, image)
end

-- Cruz girando
local cross = Instance.new("ImageLabel")
cross.Image = crossId
cross.Size = UDim2.new(0, 100, 0, 100)
cross.Position = center
cross.AnchorPoint = Vector2.new(0.5, 0.5)
cross.BackgroundTransparency = 1
cross.Rotation = 0
cross.Parent = screenGui

-- Texto espiral original
for i = 1, 100 do
	local label = Instance.new("TextLabel")
	label.Text = "PARADOXIAN.EXE"
	label.Font = Enum.Font.Arcade
	label.TextColor3 = Color3.fromRGB(255, 0, 255)
	label.BackgroundTransparency = 1
	label.TextSize = 14
	label.Size = UDim2.new(0, 150, 0, 20)
	label.Position = UDim2.new(0.5, math.cos(i/5)*i*3, 0.5, math.sin(i/5)*i*3)
	label.Parent = screenGui
end

-- Texto duplicador insano
local function insaneSpam()
	for i = 1, 1000 do
		local label = Instance.new("TextLabel")
		label.Text = "PARADOXIAN.EXE"
		label.Font = Enum.Font.Arcade
		label.TextColor3 = Color3.fromRGB(math.random(255), 0, math.random(255))
		label.BackgroundTransparency = 1
		label.TextSize = 16
		label.Size = UDim2.new(0, 200, 0, 25)
		label.Position = UDim2.new(0, math.random(0, screenGui.AbsoluteSize.X), 0, math.random(0, screenGui.AbsoluteSize.Y))
		label.Rotation = math.random(0, 360)
		label.ZIndex = 10
		label.Parent = screenGui
	end
end

-- Câmera tremendo
local function cameraShake()
	local offset = Vector3.new(math.random(-1,1), math.random(-1,1), math.random(-1,1)) * 0.5
	camera.CFrame = camera.CFrame * CFrame.new(offset)
end

-- Loop visual caótico
local angle = 0
RunService.RenderStepped:Connect(function()
	angle = angle + math.rad(2)

	-- Câmera tremor
	cameraShake()

	-- Cruz girando
	cross.Rotation = cross.Rotation + 5

	-- Atualizar círculo
	for i = 1, numImages do
		local theta = angle + (2 * math.pi * i / numImages)
		local x = math.cos(theta) * radius
		local y = math.sin(theta) * radius
		imageLabels[i].Position = center + UDim2.new(0, x, 0, y)
	end

	-- Duplicar texto loucamente
	insaneSpam()
end)

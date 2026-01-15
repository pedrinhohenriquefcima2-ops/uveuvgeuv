local HUB_NAME = "Pedro Scripts"

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")
local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

local LP = Players.LocalPlayer
local SaveEvent = ReplicatedStorage:WaitForChild("PedroScripts_Save")

_G.AimbotEnabled = _G.AimbotEnabled or false
_G.ESPEnabled = _G.ESPEnabled or false
_G.TeamCheck = _G.TeamCheck ~= false

local blur = Instance.new("BlurEffect")
blur.Size = 0
blur.Parent = Lighting

local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://9118823101"
clickSound.Volume = 0.6
clickSound.Parent = SoundService

local gui = Instance.new("ScreenGui")
gui.Name = "PedroScriptsHub"
gui.ResetOnSpawn = false
gui.Parent = LP:WaitForChild("PlayerGui")

local main = Instance.new("Frame", gui)
main.Size = UDim2.fromScale(0,0)
main.Position = UDim2.fromScale(0.5,0.5)
main.BackgroundColor3 = Color3.fromRGB(18,18,22)
main.BorderSizePixel = 0

TweenService:Create(main, TweenInfo.new(0.4), {
	Size = UDim2.fromScale(0.6,0.64),
	Position = UDim2.fromScale(0.2,0.18)
}):Play()

TweenService:Create(blur, TweenInfo.new(0.3), {Size = 16}):Play()

Instance.new("UICorner", main).CornerRadius = UDim.new(0,22)

local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(220,60,60)
stroke.Thickness = 3

local header = Instance.new("TextLabel", main)
header.Size = UDim2.fromScale(1,0.12)
header.BackgroundTransparency = 1
header.Text = HUB_NAME
header.Font = Enum.Font.GothamBlack
header.TextScaled = true
header.TextColor3 = Color3.fromRGB(255,80,80)

local btnClose = Instance.new("TextButton", main)
btnClose.Text = "✕"
btnClose.Size = UDim2.fromScale(0.06,0.07)
btnClose.Position = UDim2.fromScale(0.93,0.02)
btnClose.BackgroundColor3 = Color3.fromRGB(200,40,40)
btnClose.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", btnClose)

btnClose.MouseButton1Click:Connect(function()
	TweenService:Create(main, TweenInfo.new(0.25), {
		Size = UDim2.fromScale(0,0),
		Position = UDim2.fromScale(0.5,0.5)
	}):Play()
	TweenService:Create(blur, TweenInfo.new(0.25), {Size = 0}):Play()
	task.delay(0.3,function()
		gui.Enabled = false
	end)
end)

UIS.InputBegan:Connect(function(input,gp)
	if gp then return end
	if input.KeyCode == Enum.KeyCode.H then
		gui.Enabled = not gui.Enabled
		TweenService:Create(blur, TweenInfo.new(0.25), {
			Size = gui.Enabled and 16 or 0
		}):Play()
	end
	if input.KeyCode == Enum.KeyCode.Q then
		_G.AimbotEnabled = not _G.AimbotEnabled
		SaveEvent:FireServer(_G.AimbotEnabled,_G.ESPEnabled,_G.TeamCheck)
	end
	if input.KeyCode == Enum.KeyCode.F then
		_G.ESPEnabled = not _G.ESPEnabled
		SaveEvent:FireServer(_G.AimbotEnabled,_G.ESPEnabled,_G.TeamCheck)
	end
end)

local fps = Instance.new("TextLabel", gui)
fps.Size = UDim2.fromScale(0.15,0.05)
fps.Position = UDim2.fromScale(0.83,0.93)
fps.BackgroundTransparency = 1
fps.TextColor3 = Color3.new(1,1,1)
fps.Font = Enum.Font.GothamBold
fps.TextScaled = true

local frames = 0
local last = os.clock()

RunService.RenderStepped:Connect(function()
	frames += 1
	if os.clock() - last >= 1 then
		fps.Text = "FPS: "..frames
		frames = 0
		last = os.clock()
	end
end)

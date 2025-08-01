1# Hitbox

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()
local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local hitboxButton = Instance.new("TextButton")

screenGui.Parent = player:WaitForChild("PlayerGui")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.5, -100, 0.5, -50)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Parent = screenGui

hitboxButton.Size = UDim2.new(1, 0, 1, 0)
hitboxButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
hitboxButton.Text = "Activar Hitbox"
hitboxButton.Parent = frame

local function createHitbox()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")

    local hitbox = Instance.new("Part")
    hitbox.Size = Vector3.new(2, humanoid.HipWidth, humanoid.HipHeight)
    hitbox.Position = character.PrimaryPart.Position
    hitbox.Anchored = true
    hitbox.Transparency = 0.5
    hitbox.BrickColor = BrickColor.new("Bright red")
    hitbox.Parent = workspace

    local function updateHitbox()
        if character and character.PrimaryPart then
            hitbox.Position = character.PrimaryPart.Position
        end
    end

    local connection
    connection = game:GetService("RunService").RenderStepped:Connect(updateHitbox)

    character.AncestryChanged:Connect(function(_, parent)
        if not parent then
            connection:Disconnect()
            hitbox:Destroy()
        end
    end)
end

hitboxButton.MouseButton1Click:Connect(createHitbox)

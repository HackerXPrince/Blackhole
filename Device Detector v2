local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local lockedDevice = {} -- Stores the final result so it doesn't change

local function getFinalDevice(player)
    -- If already locked, just return the result
    if lockedDevice[player] then return lockedDevice[player] end

    local char = player.Character
    local hum = char and char:FindFirstChild("Humanoid")
    if not hum or hum.MoveDirection.Magnitude == 0 then 
        return "Analyzing..." 
    end

    local moveDir = hum.MoveDirection
    local x, z = math.abs(moveDir.X), math.abs(moveDir.Z)

    -- PC Check: Perfect WASD (Straight lines or 45-degree diagonals)
    local isWASD = (x == 1 and z == 0) or (x == 0 and z == 1) or (math.abs(x - 0.707) < 0.02)
    
    if isWASD and hum.MoveDirection.Magnitude > 0.98 then
        lockedDevice[player] = "PC / Laptop"
    else
        -- Mobile/Pad Check: Analog movement (Weird angles or walking slow)
        lockedDevice[player] = "Mobile / Pad"
    end

    return lockedDevice[player]
end

local function addESP(player)
    if player == Players.LocalPlayer then return end
    
    local text = Drawing.new("Text")
    text.Visible = false
    text.Size = 18
    text.Center = true
    text.Outline = true
    text.Font = 2

    RunService.RenderStepped:Connect(function()
        local char = player.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            local label = getFinalDevice(player)
            local pos, onScreen = Camera:WorldToViewportPoint(char.HumanoidRootPart.Position + Vector3.new(0, 3, 0))
            
            if onScreen then
                text.Position = Vector2.new(pos.X, pos.Y)
                text.Text = player.Name .. "\n[" .. label .. "]"
                
                -- Colors (PC = Black, Others = White)
                if label == "PC / Laptop" then
                    text.Color = Color3.fromRGB(0, 0, 0)
                else
                    text.Color = Color3.fromRGB(255, 255, 255)
                end
                
                text.Visible = true
            else 
                text.Visible = false 
            end
        else 
            text.Visible = false 
            if not player.Parent then text:Remove() end
        end
    end)
end

for _, p in Players:GetPlayers() do addESP(p) end
Players.PlayerAdded:Connect(addESP)

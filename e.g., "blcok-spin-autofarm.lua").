-- Block Spin Autofarm Script
-- Key: 1718171hsks
-- Load with: loadstring(game:HttpGet('YOUR_RAW_URL'))()

local required_key = "1718171hsks"
local user_input = input("Enter script key: ")

if user_input ~= required_key then
    error("Invalid key! Join our Discord for valid keys")
end

local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Root = Character:WaitForChild("HumanoidRootPart")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera

-- Anti-detection flags
setfflag("HumanoidParallelRemoveNoPhysics", "False")
setfflag("HumanoidParallelRemoveNoPhysicsNoSimulate2", "False")

-- Improved safe wait with jitter
local function SafeWait(t)
    local jitter = math.random(80, 120)/100
    wait(t * jitter)
end

-- Better block detection
local function FindNearestBlock()
    local nearestBlock = nil
    local minDistance = math.huge
    
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name:lower():find("block") and obj.Transparency < 0.5 then
            local distance = (Root.Position - obj.Position).Magnitude
            if distance < minDistance then
                minDistance = distance
                nearestBlock = obj
            end
        end
    end
    
    return nearestBlock
end

-- Improved smooth movement
local function SmoothMove(target)
    local startPos = Root.Position
    local distance = (startPos - target).Magnitude
    local steps = math.floor(distance / 3) + math.random(3, 6)
    local heightOffset = math.random(2, 4)
    
    for i = 1, steps do
        if not Root or not Root.Parent then break end
        
        local t = i / steps
        local offset = Vector3.new(
            math.random(-100, 100)/100,
            math.random(0, 50)/100,
            math.random(-100, 100)/100
        )
        
        -- Parabolic arc movement
        local height = math.sin(t * math.pi) * heightOffset
        local newPos = startPos:Lerp(target, t) + 
                       Vector3.new(0, height, 0) + 
                       offset * 0.5
        
        Root.CFrame = CFrame.new(newPos, target)
        SafeWait(0.07)
    end
end

-- Anti-AFK system
local VirtualInput = game:GetService("VirtualInputManager")
local function AntiAFK()
    coroutine.wrap(function()
        while true do
            VirtualInput:SendKeyEvent(true, Enum.KeyCode.W, false, nil)
            SafeWait(0.1)
            VirtualInput:SendKeyEvent(false, Enum.KeyCode.W, false, nil)
            SafeWait(math.random(30, 50))
        end
    end)()
end

-- Main farming loop
AntiAFK()

coroutine.wrap(function()
    while true do
        SafeWait(0.7)
        
        -- Re-acquire character references in case of respawn
        if not Character or not Character.Parent then
            Character = Player.Character or Player.CharacterAdded:Wait()
            Humanoid = Character:WaitForChild("Humanoid")
            Root = Character:WaitForChild("HumanoidRootPart")
        end
        
        local targetBlock = FindNearestBlock()
        if targetBlock then
            -- Move to a position near the block
            local targetPos = targetBlock.Position + Vector3.new(0, 3, 0)
            SmoothMove(targetPos)
            
            -- Random human-like actions
            if math.random(1, 10) > 7 then
                Humanoid.Jump = true
                SafeWait(0.2)
                
                -- Random camera movement
                local look = Root.CFrame * CFrame.Angles(
                    math.rad(math.random(-10, 10)),
                    math.rad(math.random(-30, 30)),
                    0
                )
                Root.CFrame = look
                SafeWait(0.3)
            end
            
            -- Collect block with touch simulation
            firetouchinterest(Root, targetBlock, 0)
            SafeWait(0.1)
            firetouchinterest(Root, targetBlock, 1)
            
            -- Random post-collection delay
            if math.random(1, 10) > 8 then
                SafeWait(math.random(0.5, 1.2))
            end
        else
            -- If no blocks found, move randomly
            local randomPos = Root.Position + Vector3.new(
                math.random(-50, 50),
                0,
                math.random(-50, 50)
            )
            SmoothMove(randomPos)
        end
        
        -- Random pause pattern
        if math.random(1, 15) == 1 then
            SafeWait(math.random(1.0, 2.5))
        end
    end
end)()

print("Autofarm activated! Working perfectly...")

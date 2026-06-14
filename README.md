-- Roblox Universal Autofarm Concept
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")

_G.AutoFarm = true -- Set to false to stop the loop

-- Function to safely move the character to the target
local function teleportTo(targetCFrame)
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = targetCFrame * CFrame.new(0, -5, 0) -- Positions you slightly below the mob to avoid hits
    end
end

-- Main Farming Loop
task.spawn(function()
    while _G.AutoFarm and task.wait(0.5) do
        pcall(function()
            -- Replace "Workspace.Mobs" with the directory where enemies spawn in your specific game
            for _, mob in pairs(workspace.Mobs:GetChildren()) do
                local humanoid = mob:FindFirstChildOfClass("Humanoid")
                local rootPart = mob:FindFirstChild("HumanoidRootPart")
                
                if humanoid and humanoid.Health > 0 and rootPart then
                    -- Keep teleporting to the mob until it dies
                    while _G.AutoFarm and humanoid.Health > 0 and task.wait(0.1) do
                        teleportTo(rootPart.CFrame)
                        
                        -- Simulates standard combat tool activation (e.g., punch, sword)
                        local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
                        if tool then
                            tool:Activate()
                        end
                    end
                end
            end
        end)
    end
end)

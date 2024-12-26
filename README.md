if game.PlaceId == 16732694052 then

    -- Load
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

    -- Main
    local Window = OrionLib:MakeWindow({
        Name = "FEZZER HUB",
        HidePremium = false,
        SaveConfig = true,
        ConfigFolder = "FEZZERHubConfig",
        IntroText = "by Vitin Crezy e IT4LaC"
    })

    -- Global settings
    _G.AutoCast = false
    _G.AutoLancar = false
    _G.InstatanicReel = false
    _G.FixMapBeta = false
    _G.RepairMapBeta = false
    _G.AutoShake = false

    -- Function for AutoCast
    local function AutoCast()
        while _G.AutoCast do
            local args = {
                [1] = game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flimsy Rod")
            }
            game:GetService("Players").LocalPlayer.PlayerGui.hud.safezone.backpack.events.equip:FireServer(unpack(args))
            print("Auto-equipping Flimsy Rod.")
            wait(5)  -- Adjust the wait time as needed
        end
    end

    -- Function for AutoLancar
    local function AutoLancar()
        while _G.AutoLancar do
            local args = {
                [1] = 94.6,  -- Valor ajustado para ponto flutuante
                [2] = 1
            }
            local rod = game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flimsy Rod")
            if rod then
                rod.events.cast:FireServer(unpack(args))
                print("Auto-casting...")
            else
                print("Flimsy Rod not found!")
            end
            wait(10)  -- Adjust the wait time as needed
        end
    end

    -- Function for Instatanic Reel
    local function InstatanicReel()
        while _G.InstatanicReel do
            local args = {
                [1] = 100,
                [2] = true
            }
            game:GetService("ReplicatedStorage").events.reelfinished:FireServer(unpack(args))
            print("Instatanic Reel activated.")
            wait(1)  -- Adjust the frequency as needed
        end
    end

    -- Function for AutoShake
    local function AutoShake()
        while _G.AutoShake do
            task.wait()
            xpcall(function()
                local PlayerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
                local GuiService = game:GetService("GuiService")
                local VirtualInputManager = game:GetService("VirtualInputManager")
                
                local shakeui = PlayerGui:FindFirstChild("shakeui")
                if not shakeui then return end
                local safezone = shakeui:FindFirstChild("safezone")
                local button = safezone and safezone:FindFirstChild("button")
                task.wait(0.2)
                GuiService.SelectedObject = button
                if GuiService.SelectedObject == button then
                    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Return, false, game)
                    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Return, false, game)
                end
                task.wait(0.1)
                GuiService.SelectedObject = nil
            end, function(err)
                warn(err)
            end)
        end
    end

    -- Function for Fix Map Beta
    local function FixMapBeta()
        while _G.FixMapBeta do
            local args = {
                [1] = game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Treasure Map")
            }
            game:GetService("Players").LocalPlayer.PlayerGui.hud.safezone.backpack.events.equip:FireServer(unpack(args))
            print("Equipping Treasure Map.")
            wait(5)  -- Adjust the wait time as needed
        end
    end

    -- Function for Repair Map Beta
    local function RepairMapBeta()
        while _G.RepairMapBeta do
            workspace.world.npcs:FindFirstChild("Jack Marrow").treasure.repairmap:InvokeServer()
            print("Repairing Treasure Map.")
            wait(5)  -- Adjust the wait time as needed
        end
    end

    -- Function to teleport to specific islands
    local function TeleportToIsland(islandPosition)
        local player = game.Players.LocalPlayer
        if player and player.Character then
            local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                humanoidRootPart.CFrame = CFrame.new(islandPosition + Vector3.new(0, 100, 0))  -- Teleport to 100 units above the island
                print("Teleporting to island.")
            end
        end
    end

    -- Teleport to Players
    local function TeleportToPlayer(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player and player.Character then
            local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = humanoidRootPart.CFrame
                print("Teleported to player: " .. playerName)
            end
        else
            warn("Player not found: " .. playerName)
        end
    end

    -- Rejoin Game
    local function RejoinGame()
        game:GetService("TeleportService"):Teleport(game.PlaceId, game.Players.LocalPlayer)
    end

    -- Hop Server
    local function HopServer()
        local TeleportService = game:GetService("TeleportService")
        local http = game:GetService("HttpService")
        local servers = http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"))

        for _, server in pairs(servers.data) do
            if server.playing < server.maxPlayers then
                TeleportService:TeleportToPlaceInstance(game.PlaceId, server.id, game.Players.LocalPlayer)
                return
            end
        end
        print("No available servers found.")
    end

    -- Hop Server Low
    local function HopServerLow()
        local TeleportService = game:GetService("TeleportService")
        local http = game:GetService("HttpService")
        local servers = http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"))

        local lowestServer
        for _, server in pairs(servers.data) do
            if not lowestServer or server.playing < lowestServer.playing then
                lowestServer = server
            end
        end

        if lowestServer then
            TeleportService:TeleportToPlaceInstance(game.PlaceId, lowestServer.id, game.Players.LocalPlayer)
            print("Hopped to server with lowest number of players.")
        else
            print("No servers found.")
        end
    end

    -- Create Tabs and Toggles
    local MainTab = Window:MakeTab({
        Name = "Main",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local AutoCastSection = MainTab:AddSection({
        Name = "Auto-Cast"
    })
    
    MainTab:AddToggle({
        Name = "Auto-Cast",
        Default = false,
        Callback = function(Value)
            _G.AutoCast = Value
            if _G.AutoCast then
                AutoCast()
            end
        end   
    })

    local AutoLancarSection = MainTab:AddSection({
        Name = "Auto-Lancar"
    })
    
    MainTab:AddToggle({
        Name = "Auto-Lancar",
        Default = false,
        Callback = function(Value)
            _G.AutoLancar = Value
            if _G.AutoLancar then
                AutoLancar()
            end
        end    
    })

    local InstatanicReelSection = MainTab:AddSection({
        Name = "Instatanic Reel"
    })
    
    MainTab:AddToggle({
        Name = "Instatanic Reel",
        Default = false,
        Callback = function(Value)
            _G.InstatanicReel = Value
            if _G.InstatanicReel then
                InstatanicReel()
            end
        end
    })

    local AutoShakeSection = MainTab:AddSection({
        Name = "Auto Shake"
    })

    MainTab:AddToggle({
        Name = "Auto Shake",
        Default = false,
        Callback = function(Value)
            _G.AutoShake = Value
            if _G.AutoShake then
                AutoShake()
            end
        end
    })

    -- Create Fix Map Tab
    local FixMapTab = Window:MakeTab({
        Name = "Fix Map Beta",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local FixMapSection = FixMapTab:AddSection({
        Name = "Fix Map Beta"
    })
    
    FixMapSection:AddToggle({
        Name = "Enable Fix Map Beta",
        Default = false,
        Callback = function(Value)
            _G.FixMapBeta = Value
            if _G.FixMapBeta then
                FixMapBeta()
            end
        end
    })

    local RepairMapSection = FixMapTab:AddSection({
        Name = "Repair Map Beta"
    })
    
    RepairMapSection:AddToggle({
        Name = "Enable Repair Map Beta",
        Default = false,
        Callback = function(Value)
            _G.RepairMapBeta = Value
            if _G.RepairMapBeta then
                RepairMapBeta()
            end
        end
    })

    -- Create Teleport Tab and Buttons for Islands
    local TeleportTab = Window:MakeTab({
        Name = "Teleport",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local IslandSection = TeleportTab:AddSection({
        Name = "Islands"
    })
    
    IslandSection:AddButton({
        Name = "Sunstone",
        Callback = function()
            local islandPosition = Vector3.new(-932, 131, -1109)
            TeleportToIsland(islandPosition)
        end   
    })
    
    IslandSection:AddButton({
        Name = "Merlin",
        Callback = function()
            local islandPosition = Vector3.new(-1223, 195, -1044)
            TeleportToIsland(islandPosition)
        end   
    })
    
    IslandSection:AddButton({
        Name = "Rod Of The Depths",
        Callback = function()
            local islandPosition = Vector3.new(1689.9, -902.4, 1437.7)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Moosewood Dock",
        Callback = function()
            local islandPosition = Vector3.new(360, 133, 264)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Terrapin Island Dock",
        Callback = function()
            local islandPosition = Vector3.new(-196, 133, 1945)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Forsaken Shores Dock",
        Callback = function()
            local islandPosition = Vector3.new(-2485, 133, 1562)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Jack Marrow and Sunken Rod",
        Callback = function()
            local islandPosition = Vector3.new(-2828, 214, 1519)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Roslit Bay Dock",
        Callback = function()
            local islandPosition = Vector3.new(-1462, 132, 717)
            TeleportToIsland(islandPosition)
        end   
    })

    IslandSection:AddButton({
        Name = "Kings Rod",
        Callback = function()
            local islandPosition = Vector3.new(1377, -811, -297)
            TeleportToIsland(islandPosition)
        end   
    })

    -- Create Teleport to Players Tab
    local TeleportPlayersTab = Window:MakeTab({
        Name = "Teleport to Players",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    TeleportPlayersTab:AddTextbox({
        Name = "Teleport to Player",
        Default = "",
        TextDisappear = true,
        Callback = function(playerName)
            TeleportToPlayer(playerName)
        end
    })

    -- Create Server Options Tab
    local ServerTab = Window:MakeTab({
        Name = "Server Options",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    ServerTab:AddButton({
        Name = "Rejoin",
        Callback = function()
            RejoinGame()
        end
    })

    ServerTab:AddButton({
        Name = "Hop Server",
        Callback = function()
            HopServer()
        end
    })

    ServerTab:AddButton({
        Name = "Hop Server Low",
        Callback = function()
            HopServerLow()
        end
    })

end

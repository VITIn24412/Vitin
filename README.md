if game.PlaceId == 17524285289 then
    -- Load
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

    -- Main
    local Window = OrionLib:MakeWindow({Name = "Zark Hub", HidePremium = false, SaveConfig = true, ConfigFolder = "Zark Hub"})

    -- Create Auto Farm Tab
    local AutoFarmTab = Window:MakeTab({
        Name = "Auto Farm",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local AutoFarmSection = AutoFarmTab:AddSection({
        Name = "Auto Farm"
    })

    -- Variable to control Auto Farm state
    local autoFarmEnabled = false

    -- Function to simulate a click in the middle of the screen
    local function FireClick()
        local VirtualUser = game:GetService("VirtualUser")
        VirtualUser:CaptureController()
        while autoFarmEnabled do
            VirtualUser:ClickButton1(Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2))
            wait(0)  -- Definido para 0 para a velocidade máxima de clique
        end
    end

    -- Add checkbox to toggle Auto Farm
    AutoFarmSection:AddToggle({
        Name = "Auto Farm",
        Default = false,
        Callback = function(value)
            autoFarmEnabled = value
            if autoFarmEnabled then
                FireClick()
            end
        end
    })

    -- Create Auto Egg Tab
    local AutoEggTab = Window:MakeTab({
        Name = "Auto Egg",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local AutoEggSection = AutoEggTab:AddSection({
        Name = "Auto Egg"
    })

    -- Variable to control Auto Egg state
    local autoEggEnabled = false

    -- Variable to store selected egg
    local selectedEgg = "Halloween Egg"

    -- List of eggs
    local eggs = {
        "Halloween Egg",
        "Alien Egg",
        "Starter Egg",
        "Farm Egg",
        "Grave Egg"
    }

    -- Function to map egg names to corresponding IDs
    local eggMapping = {
        ["Halloween Egg"] = "Egg1",
        ["Alien Egg"] = "Egg2",
        ["Starter Egg"] = "Egg3",
        ["Farm Egg"] = "Egg4",
        ["Grave Egg"] = "Egg5"
    }

    -- Function to hatch selected egg
    local function HatchEgg()
        while autoEggEnabled do
            local args = {
                [1] = eggMapping[selectedEgg],
                [2] = 1,
                [3] = true
            }
            game:GetService("ReplicatedStorage").HatchPet:FireServer(unpack(args))
            wait(1)  -- Ajuste o tempo de espera conforme necessário
        end
    end

    -- Add dropdown to select egg
    AutoEggSection:AddDropdown({
        Name = "Select Egg",
        Default = "Halloween Egg",
        Options = eggs,
        Callback = function(value)
            selectedEgg = value
        end
    })

    -- Add checkbox to toggle Auto Egg
    AutoEggSection:AddToggle({
        Name = "Auto Egg",
        Default = false,
        Callback = function(value)
            autoEggEnabled = value
            if autoEggEnabled then
                HatchEgg()
            end
        end
    })
end

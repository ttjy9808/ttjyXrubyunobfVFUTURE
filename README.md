local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "TTJY X RUBY HUB", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

local info = Window:MakeTab({
	Name = "info/credit",
	Icon = "rbxassetid://13257297610",
	PremiumOnly = false
})
local Section = info:AddSection({
	Name = "script update everyday,you can report bug or suggest in discord server"
})
local Section = info:AddSection({
	Name = "script made by ttjy#9808 and ! Deni210#8309"
})
local Section = info:AddSection({
	Name = "update:"
})
local Section = info:AddSection({
    Name = "- Big Update Part 2:\n" ..
           "- Remake auto rob club hop\n" ..
           "- Remake auto rob club\n" ..
           "- Remake auto rob pyramid lv1 hop and no hop\n" ..
           "- Remake auto rob pyramid lv1 instance E hop and no hop\n" ..
           "- Added Ship to heist status/level check\n" ..
           "- Added Plane to heist status/level check\n" ..
           "- Fix bank remove laser\n" ..
           "- Anti police\n" ..
           "- Anti Heros\n" ..
           "- HP CHECK"
})
local Section = info:AddSection({
	Name = "-Change from teleport to small rob 4 to tp to gun store 2"
})
local Section = info:AddSection({
	Name = "-Release small auto rob serverhop with instance E"
})
local Section = info:AddSection({
	Name = "Fix teleport to small rob 1 and 4"
})
local Section = info:AddSection({
	Name = "-Remake small auto rob"
})
local Section = info:AddSection({
	Name = "-Fix teleport to prison"
})
local Section = info:AddSection({
	Name = "-Remake small auto rob instance E"
})
local Section = info:AddSection({
	Name = "-Remake small auto rob serverhop"
})
local Section = info:AddSection({
	Name = "-Added Esp phone,laptop,cash register"
})
local Section = info:AddSection({
	Name = "-Fix Auto rob pyramid level2"
})






local Main = Window:MakeTab({
	Name = "Main",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

Main:AddButton({
	Name = "Instance E",
	Callback = function()
	for i,v in next, getgc(true) do
                if type(v) == "table" and rawget(v, "ID") and rawget(v, "Seconds") then
                    if typeof(v.Seconds) == "number" then
                        rawset(v, "Seconds", 0.001) -- setting it to 0 will not work because the game checks that.
                    end
                end
            end
end
})

Main:AddButton({
	Name = "Server Hop",
	Callback = function()
	local cmdlp = game.Players.LocalPlayer
rejoining = true
            local Decision = "any"
            local GUIDs = {}
            local maxPlayers = 0
            local pagesToSearch = 100
            if Decision == "fast" then pagesToSearch = 5 end
            local Http = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100&cursor="))
            for i = 1,pagesToSearch do
                for i,v in pairs(Http.data) do
                    if v.playing ~= v.maxPlayers and v.id ~= game.JobId then
                        maxPlayers = v.maxPlayers
                        table.insert(GUIDs, {id = v.id, users = v.playing})
                    end
                end
                if Http.nextPageCursor ~= null then Http = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100&cursor="..Http.nextPageCursor)) else break end
            end
            if Decision == "any" or Decision == "fast" then
                game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, GUIDs[math.random(1,#GUIDs)].id, cmdlp)
            elseif Decision == "smallest" then
                game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, GUIDs[#GUIDs].id, cmdlp)
            elseif Decision == "largest" then
                game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, GUIDs[1].id, cmdlp)
            else
                print("")
            end
            wait(3)
            rejoining = false
end
})

-- Define the team name to check
local teamName = "Police"

-- Variable to control loop execution
local value = false -- Set to false to turn off the loop
local hb -- Heartbeat connection

-- Function to check if a player with the specified team is near you
local function checkTeamProximity()
    while value do
        local players = game:GetService("Players"):GetPlayers()
        local playerNearby = false

        for _, player in ipairs(players) do
            if player.Team and player.Team.Name == teamName then
                local playerPosition = player.Character and player.Character.PrimaryPart and player.Character.PrimaryPart.Position

                if playerPosition then
                    local yourPosition = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character.PrimaryPart and game.Players.LocalPlayer.Character.PrimaryPart.Position
                    local distance = (playerPosition - yourPosition).Magnitude

                    if distance < 10 then -- Adjust the distance threshold as needed
                        playerNearby = true
                        break
                    end
                end
            end
        end

        if playerNearby and value then
            if not hb then
                hb = game:GetService("RunService").Heartbeat:Connect(function()
                    if value and playerNearby then
                        if not success then
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(46.720882415771484, 25.5849609375, -61.242427825927734)
                            keypress(0x45) -- Replace with the appropriate function for key press
                            task.wait(0.01)
                            keyrelease(0x45) -- Replace with the appropriate function for key release
                        end
                    else
                        success = true
                    end
                end)
            end
        elseif hb then
            hb:Disconnect()
            hb = nil
        end

        wait(1) -- Adjust the delay between checks as desired
    end
end

-- Function to handle the toggle callback
local function handleToggle(newValue)
    value = newValue -- Update the value based on the toggle state

    if value then
        checkTeamProximity() -- Perform the proximity check if value is true
    else
        if hb then
            hb:Disconnect()
            hb = nil
        end
    end
end

-- Connect the toggle callback
Main:AddToggle({
    Name = "Anti Police",
    Default = false,
    Callback = handleToggle
})

-- Define the team name to check
local heroesTeamName = "Heroes"

-- Variable to control loop execution
local heroesValue = false -- Set to false to turn off the loop
local heroesHB -- Heartbeat connection

-- Function to check if a player with the specified team is near you
local function checkHeroesProximity()
    while heroesValue do
        local players = game:GetService("Players"):GetPlayers()
        local heroesPlayerNearby = false

        for _, player in ipairs(players) do
            if player.Team and player.Team.Name == heroesTeamName then
                local playerPosition = player.Character and player.Character.PrimaryPart and player.Character.PrimaryPart.Position

                if playerPosition then
                    local yourPosition = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character.PrimaryPart and game.Players.LocalPlayer.Character.PrimaryPart.Position
                    local distance = (playerPosition - yourPosition).Magnitude

                    if distance < 10 then -- Adjust the distance threshold as needed
                        heroesPlayerNearby = true
                        break
                    end
                end
            end
        end

        if heroesPlayerNearby and heroesValue then
            if not heroesHB then
                heroesHB = game:GetService("RunService").Heartbeat:Connect(function()
                    if heroesValue and heroesPlayerNearby then
                        if not heroesSuccess then
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(46.720882415771484, 25.5849609375, -61.242427825927734)
                            keypress(0x45) -- Replace with the appropriate function for key press
                            task.wait(0.01)
                            keyrelease(0x45) -- Replace with the appropriate function for key release
                        end
                    else
                        heroesSuccess = true
                    end
                end)
            end
        elseif heroesHB then
            heroesHB:Disconnect()
            heroesHB = nil
        end

        wait(1) -- Adjust the delay between checks as desired
    end
end

-- Function to handle the toggle callback
local function handleHeroesToggle(newValue)
    heroesValue = newValue -- Update the value based on the toggle state

    if heroesValue then
        checkHeroesProximity() -- Perform the proximity check if value is true
    else
        if heroesHB then
            heroesHB:Disconnect()
            heroesHB = nil
        end
    end
end

-- Connect the toggle callback
Main:AddToggle({
    Name = "Anti Heroes",
    Default = false,
    Callback = handleHeroesToggle
})

-- Variable to control loop execution
local value = false -- Set to false to turn off the loop
local hb -- Heartbeat connection

-- Constants for rapid HP decrease check
local RAPID_HP_DECREASE_AMOUNT = 50 -- Adjust as needed
local RAPID_HP_DECREASE_TIME_THRESHOLD = 3 -- Adjust the range as needed
 -- Adjust as needed

-- Function to check the humanoid HP and rapid HP decrease
local function checkHP()
    while value do
        local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            local hp = humanoid.Health
            local currentTime = tick()

            if hp <= 25 and humanoid.LastHealthChange and humanoid.LastHealthChange >= RAPID_HP_DECREASE_AMOUNT and (not humanoid.LastHealthChangeTime or currentTime - humanoid.LastHealthChangeTime <= RAPID_HP_DECREASE_TIME_THRESHOLD) then
                if not hb then
                    hb = game:GetService("RunService").Heartbeat:Connect(function()
                        if value and humanoid.Health <= 25 and humanoid.LastHealthChange and humanoid.LastHealthChange >= RAPID_HP_DECREASE_AMOUNT and (not humanoid.LastHealthChangeTime or currentTime - humanoid.LastHealthChangeTime <= RAPID_HP_DECREASE_TIME_THRESHOLD) then
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(46.720882415771484, 25.5849609375, -61.242427825927734)
                            keypress(0x45) -- Replace with the appropriate function for key press
                            task.wait(0.01)
                            keyrelease(0x45) -- Replace with the appropriate function for key release
                        end
                    end)
                end
            elseif hb then
                hb:Disconnect()
                hb = nil
            end

            humanoid.LastHealthChange = hp - humanoid.Health
            humanoid.LastHealthChangeTime = currentTime
        end

        wait(1) -- Adjust the delay between checks as desired
    end
end

-- Function to handle the toggle callback
local function handleToggle(newValue)
    value = newValue -- Update the value based on the toggle state

    if value then
        checkHP() -- Perform the HP check if value is true
    else
        if hb then
            hb:Disconnect()
            hb = nil
        end
    end
end

-- Connect the toggle callback
Main:AddToggle({
    Name = "HP Check",
    Default = false,
    Callback = handleToggle
})


















local heist = Window:MakeTab({
	Name = "heist status/level",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

heist:AddButton({
	Name = "Pyramid",
	Callback = function()
	if workspace.Heists.Pyramid:FindFirstChild("Level1") then
  OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 1 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
elseif workspace.Heists.Pyramid:FindFirstChild("Level2") then
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 2 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

heist:AddButton({
	Name = "Club",
	Callback = function()
	if workspace.Heists.Club:FindFirstChild("Level1") then
  OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 1 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
elseif workspace.Heists.Club:FindFirstChild("Level2") then
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 2 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
elseif workspace.Heists.Club:FindFirstChild("Level3") then
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 3 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

heist:AddButton({
	Name = "Jewelry",
	Callback = function()
	if workspace.Heists["Jewelry Store"].Items.Diamonds.Containers.Glass:FindFirstChild("CanBeDamaged") or workspace.Heists["Jewelry Store"].Items.Vent.MainVent:FindFirstChild("TouchInterest") then
  OrionLib:MakeNotification({
	Name = "Jewerly",
	Content = "Jewerly is open",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Jewerly",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

heist:AddButton({
	Name = "Bank",
	Callback = function()
	if workspace.Heists.Bank.Items.Chunk:FindFirstChild("Deposit") then
  OrionLib:MakeNotification({
	Name = "Bank",
	Content = "Bank is open",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Bank",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

heist:AddButton({
	Name = "Ship",
	Callback = function()
	if workspace.Heists.Ship:FindFirstChild("Ship") or workspace.Heists.Ship:FindFirstChild("CollectMoney") then
  OrionLib:MakeNotification({
	Name = "Ship",
	Content = "Ship is open",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Ship",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

heist:AddButton({
	Name = "Plane",
	Callback = function()
	if workspace.Heists.Plane:FindFirstChild("Plane") then
  OrionLib:MakeNotification({
	Name = "Plane",
	Content = "Ship is open",
	Image = "rbxassetid://4483345998",
	Time = 5
})
else
    OrionLib:MakeNotification({
	Name = "Plane",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})
























local ESP = Window:MakeTab({
	Name = "ESP",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

ESP:AddButton({
	Name = "Phone ESP",
	Callback = function()
      		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "Phone_600_5" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})

ESP:AddButton({
	Name = "Laptop ESP",
	Callback = function()
      		      		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "Laptop_1000_6" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})

ESP:AddButton({
	Name = "Cash Register ESP",
	Callback = function()
      		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "Cash Register_600_5" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})

ESP:AddButton({
	Name = "EGG ESP",
	Callback = function()
	OrionLib:MakeNotification({
	Name = "EGG ESP SYSTEM",
	Content = "Can't close lol",
	Image = "rbxassetid://4483345998",
	Time = 5
})
      		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "RedEggCreate" or v.Name == "GreenEggCreate" or v.Name == "BlueEggCreate" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})

ESP:AddButton({
	Name = "ATM ESP",
	Callback = function()
	OrionLib:MakeNotification({
	Name = "ATM ESP",
	Content = "Can't close lol",
	Image = "rbxassetid://4483345998",
	Time = 5
})
 		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "ATM_600_5" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})

ESP:AddButton({
	Name = "Luggage ESP",
	Callback = function()
	OrionLib:MakeNotification({
	Name = "Luggage ESP",
	Content = "Can't close lol",
	Image = "rbxassetid://4483345998",
	Time = 5
})
 		for _,v in pairs(game.Workspace:GetDescendants()) do
        if v.Name == "Luggage_500_5" then
            local a = Instance.new("BillboardGui", v)
            a.Size = UDim2.new(1, 0, 1, 0)
            a.Name = "ESP"
            a.AlwaysOnTop = true
            local b = Instance.new("Frame", a)
            b.Size = UDim2.new(1, 0, 1, 0)
            b.BackgroundTransparency = 1
            b.BorderSizePixel = 0
            local c = Instance.new("TextLabel", b)
            c.Text = v.Name
            c.Size = UDim2.new(1, 0, 1, 0)
            c.BackgroundTransparency = 1
            c.BorderSizePixel = 0
            c.TextColor3 = Color3.new(0, 1, 0)
            c.TextSize = 20
            
                    local highlight = Instance.new("SelectionBox")
        highlight.Color3 = Color3.new(0, 1, 0)
        highlight.LineThickness = 0.1
        highlight.Adornee = v
        highlight.Parent = v
        highlight.Visible = true
        end
    end
  	end    
})



ESP:AddButton({
    Name = "RIP ESP",
    Callback = function()
        for _,v in pairs(game.Workspace:GetDescendants()) do
            if v.Name == "ESP" then
                v:Destroy()
            end
        end
    end    
})














local RemoveLaser = Window:MakeTab({
	Name = "RemoveLaser",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})


RemoveLaser:AddButton({
	Name = "ALL LASER",
	Callback = function()
	for i, v in pairs(game.Workspace:GetDescendants()) do
                if v.Name == "MovingLasers" or v.Name == "Laser" or v.Name == "GreenLaser" or v.Name == "GreenLasers" or v.Name == "Lasers" or v.Name == "VaultLasers" or v.Name == "NightLaser" or v.Name == "LaserDoor" or v.Name == "Lava" or v.Name == "Spotlight" or v.Name == "Flamethrowers" or v.Name == "Saws" or v.Name == "SpikeTraps" or v.Name == "Balls" or v.Name == "PressurePlates" or v.Name == "LaserWalls" or v.Name == "WallBeam" or v.Name == "Detectors" or v.Name == "Turrets" then
                    v:Destroy()
                end
            end
end
})

RemoveLaser:AddButton({
	Name = "Jewerly",
	Callback = function()
      	for i,v in pairs(game:GetService("Workspace").Heists["Jewelry Store"].Items:GetChildren()) do 
   if v.Name == "Spotlight" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

for i,v in pairs(game:GetService("Workspace").Heists["Jewelry Store"].Items:GetChildren()) do 
   if v.Name == "Laser" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

for i,v in pairs(game:GetService("Workspace").Heists["Jewelry Store"].Items:GetChildren()) do 
   if v.Name == "Flamethrowers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
	OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove laser and fire",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end    
})

RemoveLaser:AddButton({
	Name = "Apple Store",
	Callback = function()
      	for i,v in pairs(game:GetService("Workspace"):GetChildren()) do 
   if v.Name == "MovingLasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
	OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove laser from apple store",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end    
})

RemoveLaser:AddButton({
	Name = "Pyramid level1",
	Callback = function()
			      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level1:GetChildren()) do 
   if v.Name == "Lava" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

			      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level1:GetChildren()) do 
   if v.Name == "Saws" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

			      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level1:GetChildren()) do 
   if v.Name == "SpikeTraps" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

			      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level1:GetChildren()) do 
   if v.Name == "Flamethrowers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end

			      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level1:GetChildren()) do 
   if v.Name == "Balls" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Trap and Fire Ball and Lava",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end
})

RemoveLaser:AddButton({
	Name = "Pyramid level2",
	Callback = function()
		      	for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level2:GetChildren()) do 
   if v.Name == "Flamethrowers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level2:GetChildren()) do 
   if v.Name == "Lava" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Pyramid.Level2:GetChildren()) do 
   if v.Name == "PressurePlates" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Trap and Lava",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end 
})
RemoveLaser:AddButton({
	Name = "Club level1",
	Callback = function()
		for i,v in pairs(game:GetService("Workspace").Heists.Club.Level1.Items:GetChildren()) do 
   if v.Name == "Lasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Laser",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end 
})

RemoveLaser:AddButton({
	Name = "Club level2",
	Callback = function()
		for i,v in pairs(game:GetService("Workspace").Heists.Club.Level2.Items:GetChildren()) do 
   if v.Name == "LaserWalls" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Laser",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end 
})

RemoveLaser:AddButton({
	Name = "Bank",
	Callback = function()
		for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Function:GetChildren()) do 
   if v.Name == "Lasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
		for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Function:GetChildren()) do 
   if v.Name == "LaserRotate" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(Workspace.Heists.Bank.Items.Chunk.Deposit.BottomSection.Function:GetChildren()) do 
   if v.Name == "BankLasers2" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.LiftHallway.Function:GetChildren()) do 
   if v.Name == "Lasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Function:GetChildren()) do 
   if v.Name == "Root" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(Workspace.Heists.Bank.Items.Chunk.Deposit:GetChildren()) do 
   if v.Name == "CameraLasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Fucntion.BankLasers2:GetChildren()) do 
   if v.Name == "LaserMiddle" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Fucntion.BankLasers2:GetChildren()) do 
   if v.Name == "LaserFront" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Fucntion.BankLasers2:GetChildren()) do 
   if v.Name == "LaserBack" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
for i,v in pairs(game:GetService("Workspace").Heists.Bank.Items.Chunk.Deposit.BottomSection.Fucntion.BankLasers2:GetChildren()) do 
   if v.Name == "Root" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end
  	OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Laser",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end 
})

RemoveLaser:AddButton({
	Name = "Casino",
	Callback = function()
			for i,v in pairs(game:GetService("Workspace").Heists.Casino.Stealthy.Items:GetChildren()) do 
   if v.Name == "Detectors" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end 
			for i,v in pairs(game:GetService("Workspace").Heists.Casino.Stealthy.Items:GetChildren()) do 
   if v.Name == "MovingLasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end 
			for i,v in pairs(game:GetService("Workspace").Heists.Casino.Items:GetChildren()) do 
   if v.Name == "Lasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end 
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Laser",
	Image = "rbxassetid://4483345998",
	Time = 5
})
  	end 
})

RemoveLaser:AddButton({
	Name = "Plane",
	Callback = function()
				for i,v in pairs(game:GetService("Workspace").Heists.Plane.Plane:GetChildren()) do 
   if v.Name == "Lasers" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end 
  		OrionLib:MakeNotification({
	Name = "Remove L from your account",
	Content = "Done! Remove Laser",
	Image = "rbxassetid://4483345998",
	Time = 5
})
	  	end 
})

RemoveLaser:AddButton({
	Name = "Ship",
	Callback = function()
				for i,v in pairs(game:GetService("Workspace").Heists.Ship.Ship:GetChildren()) do 
   if v.Name == "Turrets" then
       for a,b in pairs(v:GetChildren()) do
           b:Destroy()
       end
   end
end 
	  	end 
})



local Teleport = Window:MakeTab({
	Name = "Teleport",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

Teleport:AddButton({
	Name = "Criminal Base",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-criminal-base/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Prison",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-prison/main/README.md'))()
	end 
})


Teleport:AddButton({
	Name = "Jewerly",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/teleport-to-jew/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Bank",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-bank/main/README.md'))()
	end 
}) 

Teleport:AddButton({
	Name = "Casino",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-casino/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Club",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-club/main/README.md'))()
	end 
})




Teleport:AddButton({
	Name = "Pyramid",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-pyramid/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Gun store",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-tp-gun-store/main/README.md'))()
	end 
})
Teleport:AddButton({
	Name = "Gun store 2",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob4/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Airport",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-airport/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Apple Store",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/apple-store/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Small rob1",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob1/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Small rob2",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob2/main/README.md'))()
	end 
})

Teleport:AddButton({
	Name = "Small rob3",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob3/main/README.md'))()
	end 
})














local Rubyhub = Window:MakeTab({
	Name = "Ruby Hub",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

local rubysection = Rubyhub:AddSection({
	Name = "After enter key the script will load"
})

_G.Key = "TTJYWITHRUBYSOMEHOWIDK!(I#)(@($($(@()!@(((#***JFEHIJWQ"
_G.KeyInput = "string"




Rubyhub:AddTextbox({
	Name = "Key",
	Default = "",
	TextDisappear = true,
	Callback = function(Value)
	_G.KeyInput = Value
	end	  
})

Rubyhub:AddButton({
Name = "Check Key",
Callback = function()
if _G.KeyInput == _G.Key then
OrionLib:MakeNotification({
Name = "Key System",
Content = "Key Correct",
Image = "rbxassetid://4483345998",
Time = 5
})
task.wait(2)
loadstring(game:HttpGet("https://raw.githubusercontent.com/Deni210/madcity/main/Ruby%20Hub%20Chapter%202", true))()
else
OrionLib:MakeNotification({
Name = "Key System",
Content = "Wrong Key",
Image = "rbxassetid://4483345998",
Time = 5
})
end
end    
})
















local autorob = Window:MakeTab({
	Name = "Auto Rob",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

local creaditautorob = autorob:AddSection({
	Name = "Auto Rob made my ttjy#9808 improved by ! Deni210#8309"
})

autorob:AddButton({
	Name = "small rob",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/manual-small-rob-un-obf/main/README.md'))()
	end 
})

autorob:AddButton({
	Name = "small rob (instance E)",
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/small-auto-rob-no-hop-unobf-but-instance-E/main/README.md'))()
	end 
})

autorob:AddButton({
	Name = "Pyramid",
	Callback = function()
	if workspace.Heists.Pyramid:FindFirstChild("Level1") then
  OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 1 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/autorobpyramidmanualnohopunobf/main/README.md'))()
elseif workspace.Heists.Pyramid:FindFirstChild("Level2") then
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 2 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-pyramid-level-2/main/README.md'))()
else
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

autorob:AddButton({
	Name = "Pyramid (instance E)",
	Callback = function()
	if workspace.Heists.Pyramid:FindFirstChild("Level1") then
  OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 1 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-pyramid-with-instance-E-no-hop-but-obf/main/README.md'))()
elseif workspace.Heists.Pyramid:FindFirstChild("Level2") then
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "Pyramid is open and it level 2 pyramid",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-pyramid-lv2-unobf-instance-E/main/README.md'))()
else
    OrionLib:MakeNotification({
	Name = "Pyramid",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})

autorob:AddButton({
	Name = "Club",
	Callback = function()
	if workspace.Heists.Club:FindFirstChild("Level1") then
  OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 1 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-club-FIX-no-hop-unobf/main/README.md'))()
elseif workspace.Heists.Club:FindFirstChild("Level2") then
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 2 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-club-FIX-no-hop-unobf/main/README.md'))()
elseif workspace.Heists.Club:FindFirstChild("Level3") then
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "Club is open and it level 3 club",
	Image = "rbxassetid://4483345998",
	Time = 5
})
loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/auto-rob-club-FIX-no-hop-unobf/main/README.md'))()
else
    OrionLib:MakeNotification({
	Name = "Club",
	Content = "close",
	Image = "rbxassetid://4483345998",
	Time = 5
})
end
	end
})











local Keybind = Window:MakeTab({
	Name = "Keybind",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

Keybind:AddBind({
	Name = "Instance E",
	Default = "gay",
	Hold = false,
	Callback = function()
		for i,v in next, getgc(true) do
                if type(v) == "table" and rawget(v, "ID") and rawget(v, "Seconds") then
                    if typeof(v.Seconds) == "number" then
                        rawset(v, "Seconds", 0.001)
                    end
                end
            end
	end    
})

Keybind:AddBind({
	Name = "Remove all laser",
	Default = "gay",
	Hold = false,
	Callback = function()
		for i, v in pairs(game.Workspace:GetDescendants()) do
                if v.Name == "MovingLasers" or v.Name == "Laser" or v.Name == "GreenLaser" or v.Name == "GreenLasers" or v.Name == "Lasers" or v.Name == "VaultLasers" or v.Name == "NightLaser" or v.Name == "LaserDoor" or v.Name == "Lava" or v.Name == "Spotlight" or v.Name == "Flamethrowers" or v.Name == "Saws" or v.Name == "SpikeTraps" or v.Name == "Balls" or v.Name == "PressurePlates" or v.Name == "LaserWalls" or v.Name == "WallBeam" or v.Name == "Detectors" or v.Name == "Turrets" then
                    v:Destroy()
                end
            end
	end    
})

Keybind:AddBind({
	Name = "tp to criminal base",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-criminal-base/main/README.md'))()
	end    
})

Keybind:AddBind({
	Name = "tp to Prison",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-prison/main/README.md'))()
	end 
})


Keybind:AddBind({
	Name = "tp to Jewelry",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/teleport-to-jew/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Bank",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-bank/main/README.md'))()
	end 
}) 

Keybind:AddBind({
	Name = "Casino",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-casino/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Club",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-club/main/README.md'))()
	end 
})




Keybind:AddBind({
	Name = "tp to Pyramid",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-pyramid/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Gun store",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-tp-gun-store/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Airport",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-airport/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Apple Store",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/apple-store/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Small rob1",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob1/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Small rob2",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob2/main/README.md'))()
	end 
})

Keybind:AddBind({
	Name = "tp to Small rob3",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob3/main/README.md'))()
	end 
})
Keybind:AddBind({
	Name = "tp to Small rob4",
	Default = "gay",
	Hold = false,
	Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ttjy9808/tp-to-small-rob4/main/README.md'))()
	end 
})
while true do
local function createBillboardGui(player)
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "Username"
    billboard.Size = UDim2.new(2, 0, 2, 0)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.LightInfluence = 0
    billboard.Parent = player.Character.Head
    local textLabel = Instance.new("TextLabel")
    textLabel.Name = "Name"
    textLabel.Text = "TTJY X RUBY HUB USER"
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.TextSize = 20
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.BackgroundTransparency = 1
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.TextColor3 = Color3.new(1, 0, 0)
    textLabel.Parent = billboard
    return billboard
end

local function onPlayerAdded(player)
    if player.Name == "LOserhaha111hjvhbhbh" or player.Name == "moonsim" or player.Name == "HellWagner" or player.Name == "pinhchill52" or player.Name == "ShiriruALT" or player.Name == "yayeban_567" or player.Name == "Ript0_legendary" or player.Name == "Crafter1030" or player.Name == "Nolkeh" or player.Name == "pipenpogi0123" or player.Name == "temvvjin1126" or player.Name == "Pixelxr984" or player.Name == "new4817" or player.Name == "LNOOBEZEZEZEZE" then
        createBillboardGui(player)
    end
end

for _, player in ipairs(game:GetService("Players"):GetPlayers()) do
    onPlayerAdded(player)
end

game:GetService("Players").PlayerAdded:Connect(onPlayerAdded)
task.wait(50)
for _,v in pairs(game.Workspace:GetDescendants()) do
if v.Name == "Username" then
v:Destroy()
end
end
end

local Mercury = loadstring(game:HttpGet("https://raw.githubusercontent.com/deeeity/mercury-lib/master/src.lua"))()

local savedWalkspeed = 16
local savedJumpPower = 50
local savedCloseKeybind = Enum.KeyCode.F

local followingPlayer = nil
local viewingPlayer = nil

local noclipEnabled = false

local function toggleNoclip(value)
    noclipEnabled = value
    if value then
        for _, part in ipairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
        print("Noclip enabled")
    else
        for _, part in ipairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
        print("Noclip disabled")
    end
end

local function followPlayer(player)
    followingPlayer = player
    while followingPlayer == player do
        local target = player.Character and player.Character.PrimaryPart
        if target then
            local distance = (target.Position - game.Players.LocalPlayer.Character.PrimaryPart.Position).magnitude
            if distance > 5 then
                game.Players.LocalPlayer.Character:MoveTo(target.Position)
            end
        end
        wait(0.1)
    end
end

local function unfollowPlayer()
    followingPlayer = nil
end

local function teleportToPosition(position)
    game.Players.LocalPlayer.Character:SetPrimaryPartCFrame(CFrame.new(position))
end

local function findPlayerFromName(name)
    for _, player in ipairs(game.Players:GetPlayers()) do
        if string.lower(string.sub(player.Name, 1, #name)) == string.lower(name) then
            return player
        end
    end
    return nil
end

local GUI = Mercury:Create{
    Name = "Admin Commands",
    Size = UDim2.fromOffset(500, 400),
    Theme = Mercury.Themes.Dark,
    Link = "https://github.com/deeeity/mercury-lib"
}

local playerTab = GUI:Tab{
    Name = "Player Commands",
    Icon = "rbxasset://textures/ui/PlayerList/FriendIcon@2x.png"
}

local walkspeedSlider = playerTab:Slider{
    Name = "Walkspeed",
    Default = savedWalkspeed,
    Min = 0,
    Max = 100,
    Callback = function(value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
    end
}

local jumpPowerSlider = playerTab:Slider{
    Name = "Jump Power",
    Default = savedJumpPower,
    Min = 0,
    Max = 250,
    Callback = function(value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
    end
}

playerTab:Button{
    Name = "Reset Sliders",
    Callback = function()
        walkspeedSlider:Set(savedWalkspeed)
        jumpPowerSlider:Set(savedJumpPower)
    end
}

playerTab:Button{
    Name = "Sit",
    Callback = function()
        local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.Sit = true
        end
    end
}

playerTab:Toggle{
    Name = "Noclip",
    StartingState = false,
    Callback = function(value)
        toggleNoclip(value)
    end
}

local scriptTab = GUI:Tab{
    Name = "Scripts",
    Icon = "rbxasset://textures/ui/PlayerList/OwnerIcon@2x.png"
}

scriptTab:Button{
    Name = "Infinity Yield",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
    end
}

scriptTab:Button{
    Name = "CMD X",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source", true))()
    end
}

scriptTab:Button{
    Name = "CTRL + Click (Teleport)",
    Callback = function()
        local mouse = game.Players.LocalPlayer:GetMouse()
        mouse.Button1Down:Connect(function()
            if not game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) then return end
            teleportToPosition(mouse.Hit.Position)
        end)
    end
}

local miscTab = GUI:Tab{
    Name = "Misc",
    Icon = "rbxasset://textures/ui/PlayerList/StarIcon@2x.png"
}

local followOtherPlayersTextBox = miscTab:Textbox{
    Name = "Follow Other Players",
    Placeholder = "Enter player name...",
    Callback = function(text)
        local player = findPlayerFromName(text)
        if player then
            followPlayer(player)
            print("Following player:", player.Name)
        else
            print("Player not found")
        end
    end
}

miscTab:Button{
    Name = "Unfollow",
    Callback = function()
        unfollowPlayer()
        print("Unfollowed player")
    end
}

local viewOtherPlayersTextBox = miscTab:Textbox{
    Name = "View Other Players",
    Placeholder = "Enter player name...",
    Callback = function(text)
        local player = findPlayerFromName(text)
        if player then
            game.Workspace.CurrentCamera.CameraSubject = player.Character.Humanoid
            print("Viewing player:", player.Name)
        else
            print("Player not found")
        end
    end
}

miscTab:Button{
    Name = "Unview",
    Callback = function()
        game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
        print("Unviewed player")
    end
}

local creditsTab = GUI:Tab{
    Name = "Credits",
    Icon = "rbxasset://textures/ui/PlayerList/developer@2x.png"
}

creditsTab:Credit{
    Name = "Scripter: ToriTreat",
    Description = "Adapted the script"
}

creditsTab:Credit{
    Name = "Designer: ToriTreat",
    Description = "Designed the UI"
}

local settingsTab = GUI:Tab{
    Name = "Settings",
    Icon = "rbxassetid://8569322835"
}

settingsTab:Button{
    Name = "Save jump power and walkspeed changes",
    Callback = function()
        savedWalkspeed = walkspeedSlider:Get()
        savedJumpPower = jumpPowerSlider:Get()
        print("Changes saved")
    end
}

settingsTab:Button{
    Name = "Load Saved",
    Callback = function()
        walkspeedSlider:Set(savedWalkspeed)
        jumpPowerSlider:Set(savedJumpPower)
        print("Previous settings loaded")
    end
}

settingsTab:Keybind{
    Name = "Close Keybind",
    Keybind = savedCloseKeybind,
    Description = "Set the keybind to close GUI"
}

GUI:Run()

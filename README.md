-- Astro X Hub (Fixed & Complete)
-- By: (reconstructed)
-- Notes: This preserves your original tabs, themes, popup, draggable toggle, and script execution behavior.

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Cleanup old GUIs
for _, guiName in ipairs({"AstroXHub", "AstroXToggleGui", "AstroXPopupGui"}) do
    local oldGui = PlayerGui:FindFirstChild(guiName)
    if oldGui then oldGui:Destroy() end
end

-- Popup function
local function showPopup(text)
    local oldPopup = PlayerGui:FindFirstChild("AstroXPopupGui")
    if oldPopup then oldPopup:Destroy() end

    local popupGui = Instance.new("ScreenGui")
    popupGui.Name = "AstroXPopupGui"
    popupGui.ResetOnSpawn = false
    popupGui.Parent = PlayerGui
    popupGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 280, 0, 40)
    frame.Position = UDim2.new(1, -300, 1, -70)
    frame.AnchorPoint = Vector2.new(0, 0)
    frame.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    frame.ClipsDescendants = true
    frame.Parent = popupGui

    local corner = Instance.new("UICorner", frame)
    corner.CornerRadius = UDim.new(0, 12)

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -20, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 20
    label.Text = text
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextYAlignment = Enum.TextYAlignment.Center
    label.Parent = frame
    label.TextTransparency = 1

    local fadeIn = TweenService:Create(label, TweenInfo.new(0.3), {TextTransparency = 0})
    fadeIn:Play()
    fadeIn.Completed:Wait()
    task.wait(2)
    local fadeOut = TweenService:Create(label, TweenInfo.new(0.3), {TextTransparency = 1})
    fadeOut:Play()
    fadeOut.Completed:Wait()
    popupGui:Destroy()
end

-- Toggle Button GUI (always visible)
local toggleGui = Instance.new("ScreenGui")
toggleGui.Name = "AstroXToggleGui"
toggleGui.Parent = PlayerGui
toggleGui.ResetOnSpawn = false

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 120, 0, 50)
toggleBtn.Position = UDim2.new(0, 10, 0, 10)
toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Text = "Toggle Astro X"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20
toggleBtn.ZIndex = 50
toggleBtn.Parent = toggleGui

local toggleCorner = Instance.new("UICorner", toggleBtn)
toggleCorner.CornerRadius = UDim.new(0, 12)

toggleBtn.MouseEnter:Connect(function()
    toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 120, 120)
end)
toggleBtn.MouseLeave:Connect(function()
    toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
end)

-- Draggable Toggle Button (fixed)
local draggingToggle, dragInputToggle, dragStartToggle, startPosToggle
local function updateDragToggle(input)
    local delta = input.Position - dragStartToggle
    toggleBtn.Position = UDim2.new(startPosToggle.X.Scale, startPosToggle.X.Offset + delta.X, startPosToggle.Y.Scale, startPosToggle.Y.Offset + delta.Y)
end

toggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        draggingToggle = true
        dragStartToggle = input.Position
        startPosToggle = toggleBtn.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                draggingToggle = false
            end
        end)
    end
end)

toggleBtn.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInputToggle = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInputToggle and draggingToggle then
        updateDragToggle(input)
    end
end)

-- Main Hub GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AstroXHub"
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false
ScreenGui.Enabled = false -- Start hidden

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 420, 0, 360)
Frame.Position = UDim2.new(0.5, 0, 0.5, 0)
Frame.AnchorPoint = Vector2.new(0.5, 0.5)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
Frame.Parent = ScreenGui
Frame.ClipsDescendants = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 16)

local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
TitleBar.Parent = Frame

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, 0, 1, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "Astro X Hub (Beta) 🌌"
TitleText.TextColor3 = Color3.fromRGB(230, 230, 230)
TitleText.Font = Enum.Font.GothamBold
TitleText.TextSize = 22
TitleText.Parent = TitleBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 40, 0, 40)
CloseBtn.Position = UDim2.new(1, -40, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.Parent = TitleBar
CloseBtn.AutoButtonColor = false

local closeCorner = Instance.new("UICorner", CloseBtn)
closeCorner.CornerRadius = UDim.new(0, 12)

CloseBtn.MouseEnter:Connect(function()
    CloseBtn.BackgroundColor3 = Color3.fromRGB(220, 80, 80)
end)
CloseBtn.MouseLeave:Connect(function()
    CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
end)

-- UI Animation Tweens
local TweenIn = TweenService:Create(Frame, TweenInfo.new(0.3, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Size = UDim2.new(0, 420, 0, 360)})
local TweenOut = TweenService:Create(Frame, TweenInfo.new(0.3, Enum.EasingStyle.Quint, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})

local isOpen = false
local toggleCooldown = false

CloseBtn.MouseButton1Click:Connect(function()
    if toggleCooldown then return end
    toggleCooldown = true
    TweenOut:Play()
    TweenOut.Completed:Wait()
    ScreenGui.Enabled = false
    showPopup("Astro X Hub closed!")
    isOpen = false
    toggleCooldown = false
end)

-- Tabs Frame
local TabsFrame = Instance.new("ScrollingFrame")
TabsFrame.Size = UDim2.new(0, 130, 1, -40)
TabsFrame.Position = UDim2.new(0, 0, 0, 40)
TabsFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
TabsFrame.BorderSizePixel = 0
TabsFrame.Parent = Frame
TabsFrame.ScrollBarThickness = 6
TabsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
TabsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)

local tabsUICorner = Instance.new("UICorner", TabsFrame)
tabsUICorner.CornerRadius = UDim.new(0, 16)

local TabsLayout = Instance.new("UIListLayout", TabsFrame)
TabsLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabsLayout.Padding = UDim.new(0, 8)

-- Content Frame
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -130, 1, -40)
ContentFrame.Position = UDim2.new(0, 130, 0, 40)
ContentFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
ContentFrame.Parent = Frame

local contentUICorner = Instance.new("UICorner", ContentFrame)
contentUICorner.CornerRadius = UDim.new(0, 16)

local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Size = UDim2.new(1, -20, 1, -20)
ScrollFrame.Position = UDim2.new(0, 10, 0, 10)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.Parent = ContentFrame
ScrollFrame.ScrollBarThickness = 6
ScrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)

local scrollLayout = Instance.new("UIListLayout", ScrollFrame)
scrollLayout.SortOrder = Enum.SortOrder.LayoutOrder
scrollLayout.Padding = UDim.new(0, 6)

-- Theme ScrollFrame for theme switcher tab
local ThemeScrollFrame = Instance.new("ScrollingFrame")
ThemeScrollFrame.Size = UDim2.new(1, -20, 1, -20)
ThemeScrollFrame.Position = UDim2.new(0, 10, 0, 10)
ThemeScrollFrame.BackgroundTransparency = 1
ThemeScrollFrame.Parent = ContentFrame
ThemeScrollFrame.ScrollBarThickness = 6
ThemeScrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
ThemeScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ThemeScrollFrame.Visible = false

local themeLayout = Instance.new("UIListLayout", ThemeScrollFrame)
themeLayout.SortOrder = Enum.SortOrder.LayoutOrder
themeLayout.Padding = UDim.new(0, 6)

-- Tabs Data with your old scripts plus new ones added (all combined)
local Tabs = {
    {
        Name = "📌 Info",
        Scripts = {
            {Name = "Copy Discord Link", Url = nil},
        },
    },
    {
        Name = "📝 Update Log",
        Scripts = {},
    },
    {
        Name = "🎨 Theme Switcher",
        Scripts = {},
    },
    {
        Name = "🌐 Universal",
        Scripts = {
            {Name = "Infinite Yield", Url = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"},
            -- Add any new universal scripts here if needed
        },
    },
    {
        Name = "🌱 Grow a Garden",
        Scripts = {
            {Name = "Original Grow a Garden", Url = "https://api.luarmor.net/files/v4/loaders/2529a5f9dfddd5523ca4e22f21cceffa.lua"},
            {Name = "Grow a Garden (m00ndiety)", Url = "https://raw.githubusercontent.com/m00ndiety/Grow-a-garden/refs/heads/main/Grow-A-fkin-Garden.txt"},
        },
    },
    {
        Name = "🏡 Brookhaven",
        Scripts = {
            {Name = "Original Brookhaven", Url = "https://raw.githubusercontent.com/kigredns/testUIDK/refs/heads/main/panel.lua"},
            {Name = "Brookhaven (TheDarkoneMarcillisePex)", Url = "https://raw.githubusercontent.com/TheDarkoneMarcillisePex/Other-Scripts/main/Brook%20Haven%20Gui"},
        },
    },
    {
        Name = "🍉 Blox Fruits",
        Scripts = {
            {Name = "Original Blox Fruits", Url = "https://api.luarmor.net/files/v3/loaders/7d8a2a1a9a562a403b52532e58a14065.lua"},
            {Name = "Blox Fruits (Speed Hub X)", Url = "https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua"},
        },
    },
    -- Adopt Me tab removed as requested
    {
        Name = "🧗 Tower of Hell",
        Scripts = {
            {Name = "Tower of Hell (Original)", Url = "https://luau.tech/build"},
            {Name = "Tower of Hell (very op skidded)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/very%20op%20skidded%20Tower%20of%20hell%20script.txt"},
        },
    },
    {
        Name = "🔪 Murder Mystery 2",
        Scripts = {
            {Name = "MM2 GUI (Original)", Url = "https://api.luarmor.net/files/v4/loaders/d7be76c234d46ce6770101fded39760c.lua"},
            {Name = "MM2 GUI (Universe King skidded)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/Mm2%20Gui%20Op%20Skidded%20By%20Universe%20King.txt"},
        },
    },
    {
        Name = "🐾 Pet Simulator X",
        Scripts = {
            {Name = "Pet Simulator X (Original)", Url = "https://raw.githubusercontent.com/junixaldrblxscripts/JunixHubScripts/refs/heads/main/johnhub"},
            -- No new script yet
        },
    },
    {
        Name = "🚪 Doors",
        Scripts = {
            {Name = "Doors (Original)", Url = "https://raw.githubusercontent.com/2btrleans/scr1ipt/refs/heads/main/Doors%20Latest.lua"},
            -- No new script yet
        },
    },
    {
        Name = "⚽ Blue Lock Rivals",
        Scripts = {
            {Name = "Blue Lock Rivals (Original)", Url = "https://pandadevelopment.net/virtual/file/c98e09ae28506354"},
            {Name = "Blue Lock Rivals (Luarmor)", Url = "https://api.luarmor.net/files/v3/loaders/255ac567ced3dcb9e69aa7e44c423f19.lua"},
        },
    },
    {
        Name = "🏀 Basketball Zero",
        Scripts = {
            {Name = "Basketball Zero (Original, unchanged)", Url = "https://api.luarmor.net/files/v3/loaders/0abf22d9dc1307a6cf1a4a17955e312d.lua"},
        },
    },
    {
        Name = "🖋 Ink Game",
        Scripts = {
            {Name = "Ink Game (Original)", Url = "https://raw.githubusercontent.com/NysaDanielle/games/refs/heads/main/inkgame.lua"},
            {Name = "Ink Game (ringta.lua)", Url = "https://raw.githubusercontent.com/wefwef127382/inkgames.github.io/refs/heads/main/ringta.lua"},
        },
    },
    {
        Name = "✋ Slap Battles",
        Scripts = {
            {Name = "Slap Battles (Original)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/Very%20Op%20slap%20battles%20skidded%204.txt"},
            {Name = "Slap Battles (Newer skidded)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/Very%20Op%20slap%20battles%20skidded%205.txt"},
        },
    },
    {
        Name = "🔫 Arsenal",
        Scripts = {
            {Name = "Arsenal (Original)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/Very%20Op%20Arsenal%20keyless%20script%20skidded.txt"},
            {Name = "Arsenal (Newer op script)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/very%20op%20script%20Source%20skidded.txt"},
        },
    },
    {
        Name = "🧠 Steal a Brainrot",
        Scripts = {
            {Name = "Steal a Brainrot (Very op GUI)", Url = "https://raw.githubusercontent.com/101iii101/file/refs/heads/main/Very%20op%20Steal%20a%20brainrot%20gui%208.txt"},
            {Name = "Steal a Brainrot (LyezHub)", Url = "https://raw.githubusercontent.com/walkdownej/main/refs/heads/main/lyezhub.lua"},
        },
    },
}

-- Store buttons so we can highlight selected tab
local tabButtons = {}

local function clearContent()
    -- Clear ScrollFrame and ThemeScrollFrame children (but preserve UIListLayout)
    for _, child in ipairs(ScrollFrame:GetChildren()) do
        if child:IsA("TextButton") or child:IsA("TextLabel") or child:IsA("Frame") or child:IsA("ImageLabel") then
            child:Destroy()
        end
    end
    for _, child in ipairs(ThemeScrollFrame:GetChildren()) do
        if child:IsA("TextButton") or child:IsA("TextLabel") or child:IsA("Frame") or child:IsA("ImageLabel") then
            child:Destroy()
        end
    end
end

-- Theme data including your new themes and old ones (original + new)
local Themes = {
    {
        Name = "Default",
        FrameColor = Color3.fromRGB(30, 30, 40),
        TitleBarColor = Color3.fromRGB(20, 20, 30),
        TabsFrameColor = Color3.fromRGB(40, 40, 60),
        ContentFrameColor = Color3.fromRGB(40, 40, 60),
        AccentColor = Color3.fromRGB(255, 80, 80),
    },
    {
        Name = "Dark Purple",
        FrameColor = Color3.fromRGB(45, 32, 59),
        TitleBarColor = Color3.fromRGB(38, 25, 51),
        TabsFrameColor = Color3.fromRGB(60, 45, 79),
        ContentFrameColor = Color3.fromRGB(60, 45, 79),
        AccentColor = Color3.fromRGB(201, 120, 255),
    },
    {
        Name = "Ocean Blue",
        FrameColor = Color3.fromRGB(26, 56, 82),
        TitleBarColor = Color3.fromRGB(16, 36, 57),
        TabsFrameColor = Color3.fromRGB(35, 75, 110),
        ContentFrameColor = Color3.fromRGB(35, 75, 110),
        AccentColor = Color3.fromRGB(84, 180, 255),
    },
    {
        Name = "Sunset Orange",
        FrameColor = Color3.fromRGB(55, 27, 17),
        TitleBarColor = Color3.fromRGB(45, 22, 14),
        TabsFrameColor = Color3.fromRGB(65, 32, 20),
        ContentFrameColor = Color3.fromRGB(65, 32, 20),
        AccentColor = Color3.fromRGB(255, 138, 70),
    },
    {
        Name = "Midnight Green",
        FrameColor = Color3.fromRGB(16, 48, 40),
        TitleBarColor = Color3.fromRGB(10, 30, 25),
        TabsFrameColor = Color3.fromRGB(25, 70, 60),
        ContentFrameColor = Color3.fromRGB(25, 70, 60),
        AccentColor = Color3.fromRGB(70, 255, 210),
    },
    {
        Name = "Candy Pink",
        FrameColor = Color3.fromRGB(255, 200, 220),
        TitleBarColor = Color3.fromRGB(255, 170, 190),
        TabsFrameColor = Color3.fromRGB(255, 140, 170),
        ContentFrameColor = Color3.fromRGB(255, 140, 170),
        AccentColor = Color3.fromRGB(255, 80, 140),
    },
}

local currentTheme = Themes[1]

local function applyTheme(theme)
    Frame.BackgroundColor3 = theme.FrameColor
    TitleBar.BackgroundColor3 = theme.TitleBarColor
    TabsFrame.BackgroundColor3 = theme.TabsFrameColor
    ContentFrame.BackgroundColor3 = theme.ContentFrameColor
    toggleBtn.BackgroundColor3 = theme.AccentColor
    CloseBtn.BackgroundColor3 = theme.AccentColor
end

-- Load theme switcher UI
local function loadThemeSwitcher()
    clearContent()
    ScrollFrame.Visible = false
    ThemeScrollFrame.Visible = true

    for i, theme in ipairs(Themes) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, 0, 0, 40)
        btn.BackgroundColor3 = theme.FrameColor
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 20
        btn.Text = theme.Name
        btn.Parent = ThemeScrollFrame

        local btnCorner = Instance.new("UICorner", btn)
        btnCorner.CornerRadius = UDim.new(0, 12)

        btn.MouseEnter:Connect(function()
            btn.BackgroundColor3 = theme.AccentColor
        end)
        btn.MouseLeave:Connect(function()
            btn.BackgroundColor3 = theme.FrameColor
        end)

        btn.MouseButton1Click:Connect(function()
            currentTheme = theme
            applyTheme(theme)
            showPopup("Theme changed to: "..theme.Name)
        end)
    end
end

-- Load scripts UI for given tab index
local function loadTab(index)
    clearContent()
    ThemeScrollFrame.Visible = false
    ScrollFrame.Visible = true

    for _, btn in pairs(tabButtons) do
        btn.BackgroundColor3 = TabsFrame.BackgroundColor3
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end
    tabButtons[index].BackgroundColor3 = currentTheme.AccentColor
    tabButtons[index].TextColor3 = Color3.fromRGB(0, 0, 0)

    local tab = Tabs[index]

    if tab.Name == "📌 Info" then
        local infoLabel = Instance.new("TextLabel")
        infoLabel.Size = UDim2.new(1, 0, 0, 80)
        infoLabel.BackgroundTransparency = 1
        infoLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        infoLabel.Font = Enum.Font.GothamBold
        infoLabel.TextSize = 20
        infoLabel.Text = "Astro X Hub - Roblox Script Hub\nUse the tabs to select scripts.\nToggle with the button on the screen."
        infoLabel.TextWrapped = true
        infoLabel.Parent = ScrollFrame
        return
    elseif tab.Name == "📝 Update Log" then
        local logLabel = Instance.new("TextLabel")
        logLabel.Size = UDim2.new(1, 0, 0, 200)
        logLabel.BackgroundTransparency = 1
        logLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        logLabel.Font = Enum.Font.Gotham
        logLabel.TextSize = 16
        logLabel.Text = [[
- Added new scripts to all game tabs
- Added 4 new themes to theme switcher
- Toggle button is now draggable
- UI remains unchanged, preserving your original design
- Popup messages confirm actions
- Scripts execute on button click only
]]
        logLabel.TextWrapped = true
        logLabel.Parent = ScrollFrame
        return
    elseif tab.Name == "🎨 Theme Switcher" then
        loadThemeSwitcher()
        return
    end

    -- For normal game tabs with scripts
    for _, scriptData in ipairs(tab.Scripts) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, 0, 0, 40)
        btn.BackgroundColor3 = currentTheme.FrameColor
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 18
        btn.Text = scriptData.Name
    

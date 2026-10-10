
-- SOULS HUB | TOKYO GHOUL SOCCER EDITION
-- Single LocalScript for your own Roblox Studio game.
-- HBE controls are a UI preview; no server hitboxes are modified.

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local old = playerGui:FindFirstChild("SoulsHub")
if old then old:Destroy() end

local RED = Color3.fromRGB(190, 20, 50)
local DARK = Color3.fromRGB(20, 20, 25)
local PANEL = Color3.fromRGB(32, 32, 40)
local WHITE = Color3.fromRGB(245, 245, 245)

local gui = Instance.new("ScreenGui")
gui.Name = "SoulsHub"
gui.ResetOnSpawn = false
gui.Parent = playerGui

local function create(className, properties, parent)
    local obj = Instance.new(className)
    for key, value in pairs(properties) do
        obj[key] = value
    end
    obj.Parent = parent
    return obj
end

local main = create("Frame", {
    Name = "Main",
    Size = UDim2.fromOffset(510, 365),
    Position = UDim2.new(0.5, -255, 0.5, -182),
    BackgroundColor3 = DARK,
    BorderSizePixel = 0,
}, gui)

create("UICorner", {CornerRadius = UDim.new(0, 10)}, main)
create("UIStroke", {Color = RED, Thickness = 2}, main)

local top = create("Frame", {
    Size = UDim2.new(1, 0, 0, 42),
    BackgroundColor3 = RED,
    BorderSizePixel = 0,
}, main)

create("UICorner", {CornerRadius = UDim.new(0, 10)}, top)

create("TextLabel", {
    Size = UDim2.new(1, -115, 1, 0),
    Position = UDim2.fromOffset(12, 0),
    BackgroundTransparency = 1,
    Text = "SOULS HUB | TOKYO GHOUL",
    TextColor3 = WHITE,
    Font = Enum.Font.GothamBold,
    TextSize = 15,
    TextXAlignment = Enum.TextXAlignment.Left,
}, top)

local minimize = create("TextButton", {
    Size = UDim2.fromOffset(32, 30),
    Position = UDim2.new(1, -74, 0, 6),
    Text = "-",
    TextSize = 22,
    Font = Enum.Font.GothamBold,
    TextColor3 = WHITE,
    BackgroundColor3 = DARK,
}, top)

local close = create("TextButton", {
    Size = UDim2.fromOffset(32, 30),
    Position = UDim2.new(1, -38, 0, 6),
    Text = "X",
    TextSize = 14,
    Font = Enum.Font.GothamBold,
    TextColor3 = WHITE,
    BackgroundColor3 = DARK,
}, top)

local floating = create("TextButton", {
    Size = UDim2.fromOffset(52, 52),
    Position = UDim2.new(0, 18, 0.5, -26),
    Text = "S",
    TextSize = 25,
    Font = Enum.Font.GothamBlack,
    TextColor3 = WHITE,
    BackgroundColor3 = RED,
    Visible = false,
}, gui)

create("UICorner", {CornerRadius = UDim.new(1, 0)}, floating)

local sidebar = create("Frame", {
    Position = UDim2.fromOffset(10, 52),
    Size = UDim2.new(0, 130, 1, -62),
    BackgroundColor3 = PANEL,
    BorderSizePixel = 0,
}, main)

create("UICorner", {CornerRadius = UDim.new(0, 8)}, sidebar)

create("UIPadding", {
    PaddingTop = UDim.new(0, 7),
    PaddingLeft = UDim.new(0, 6),
    PaddingRight = UDim.new(0, 6),
}, sidebar)

create("UIListLayout", {
    Padding = UDim.new(0, 6),
    SortOrder = Enum.SortOrder.LayoutOrder,
}, sidebar)

local content = create("ScrollingFrame", {
    Position = UDim2.fromOffset(150, 52),
    Size = UDim2.new(1, -160, 1, -62),
    BackgroundColor3 = PANEL,
    BorderSizePixel = 0,
    ScrollBarThickness = 5,
    CanvasSize = UDim2.new(),
    AutomaticCanvasSize = Enum.AutomaticSize.Y,
}, main)

create("UICorner", {CornerRadius = UDim.new(0, 8)}, content)

create("UIPadding", {
    PaddingTop = UDim.new(0, 10),
    PaddingLeft = UDim.new(0, 10),
    PaddingRight = UDim.new(0, 10),
    PaddingBottom = UDim.new(0, 10),
}, content)

create("UIListLayout", {
    Padding = UDim.new(0, 8),
    SortOrder = Enum.SortOrder.LayoutOrder,
}, content)

local function clearContent()
    for _, child in ipairs(content:GetChildren()) do
        if not child:IsA("UIListLayout")
            and not child:IsA("UIPadding") then
            child:Destroy()
        end
    end
end

local function label(text)
    return create("TextLabel", {
        Size = UDim2.new(1, -5, 0, 30),
        BackgroundTransparency = 1,
        Text = text,
        TextColor3 = WHITE,
        Font = Enum.Font.GothamBold,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    }, content)
end

local function button(text, callback)
    local b = create("TextButton", {
        Size = UDim2.new(1, -5, 0, 35),
        BackgroundColor3 = DARK,
        Text = text,
        TextColor3 = WHITE,
        Font = Enum.Font.Gotham,
        TextSize = 13,
        AutoButtonColor = true,
    }, content)

    create("UICorner", {CornerRadius = UDim.new(0, 6)}, b)
    b.Activated:Connect(callback)
    return b
end

local function slider(title, minValue, maxValue, initial, callback)
    local heading = label(title .. ": " .. initial)

    local holder = create("TextButton", {
        Size = UDim2.new(1, -5, 0, 24),
        BackgroundColor3 = DARK,
        Text = "",
        AutoButtonColor = false,
    }, content)

    create("UICorner", {CornerRadius = UDim.new(0, 6)}, holder)

    local fill = create("Frame", {
        Size = UDim2.new(
            (initial - minValue) / (maxValue - minValue), 0, 1, 0
        ),
        BackgroundColor3 = RED,
        BorderSizePixel = 0,
    }, holder)

    create("UICorner", {CornerRadius = UDim.new(0, 6)}, fill)

    local dragging = false

    local function update(x)
        local ratio = math.clamp(
            (x - holder.AbsolutePosition.X) / holder.AbsoluteSize.X,
            0, 1
        )

        local value = math.floor(
            minValue + ratio * (maxValue - minValue) + 0.5
        )

        fill.Size = UDim2.new(ratio, 0, 1, 0)
        heading.Text = title .. ": " .. value
        callback(value)
    end

    holder.Activated:Connect(function()
        update(UIS:GetMouseLocation().X)
    end)

    holder.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
            or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            update(input.Position.X)
        end
    end)

    UIS.InputChanged:Connect(function(input)
        if dragging and (
            input.UserInputType == Enum.UserInputType.MouseMovement
            or input.UserInputType == Enum.UserInputType.Touch
        ) then
            update(input.Position.X)
        end
    end)

    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
            or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
end

local hbeEnabled = false
local hbeSize = 1

local function showTab(tabName)
    clearContent()

    if tabName == "HBE" then
        label("HBE | FEET HITBOX")
        label("Toggle and size preview")

        local toggle = button("HBE TOGGLE: OFF", function() end)

        local function refreshToggle()
            toggle.Text = "HBE TOGGLE: " ..
                (hbeEnabled and "ON" or "OFF")

            toggle.BackgroundColor3 = hbeEnabled and WHITE or DARK
            toggle.TextColor3 = hbeEnabled and DARK or WHITE

            player:SetAttribute("SoulsHub_HBE_Enabled", hbeEnabled)
        end

        toggle.Activated:Connect(function()
            hbeEnabled = not hbeEnabled
            refreshToggle()
        end)

        label("Selected size: " .. hbeSize)

        for level = 1, 5 do
            local selectedLevel = level

            button("HBE SIZE " .. level, function()
                hbeSize = selectedLevel
                player:SetAttribute("SoulsHub_HBE_Size", hbeSize)
                showTab("HBE")
            end)
        end

        label("Current setting: " ..
            (hbeEnabled and "Enabled" or "Disabled"))

        label("Size 5 is the largest preview setting.")
        label("Actual hitbox changes must be handled by the game server.")

        refreshToggle()

    elseif tabName == "Player" then
        label("PLAYER SETTINGS")

        slider("Walk speed preview", 8, 32, 16, function(value)
            player:SetAttribute("SoulsHub_WalkSpeedPreview", value)
        end)

        button("Sprint preview: Toggle", function()
            local value = not player:GetAttribute("SoulsHub_SprintPreview")
            player:SetAttribute("SoulsHub_SprintPreview", value)
        end)

    elseif tabName == "Ball" then
        label("BALL SETTINGS")

        slider("Shot power preview", 1, 100, 50, function(value)
            player:SetAttribute("SoulsHub_ShotPowerPreview", value)
        end)

        slider("Curve preview", 0, 100, 25, function(value)
            player:SetAttribute("SoulsHub_CurvePreview", value)
        end)

        label("Preview settings only.")

    elseif tabName == "Reach" then
        label("REACH SETTINGS")

        slider("Reach preview", 1, 20, 5, function(value)
            player:SetAttribute("SoulsHub_ReachPreview", value)
        end)

    elseif tabName == "Training" then
        label("TRAINING")

        button("Mark training ready", function()
            player:SetAttribute("SoulsHub_TrainingReady", true)
            label("Training marked ready.")
        end)

        button("Reset preview values", function()
            for _, name in ipairs({
                "SoulsHub_WalkSpeedPreview",
                "SoulsHub_SprintPreview",
                "SoulsHub_ShotPowerPreview",
                "SoulsHub_CurvePreview",
                "SoulsHub_ReachPreview",
                "SoulsHub_TrainingReady",
                "SoulsHub_HBE_Enabled",
                "SoulsHub_HBE_Size",
            }) do
                player:SetAttribute(name, nil)
            end

            hbeEnabled = false
            hbeSize = 1
            showTab("Training")
        end)

    elseif tabName == "Misc" then
        label("MISCELLANEOUS")

        button("Reset window position", function()
            main.Position = UDim2.new(0.5, -255, 0.5, -182)
        end)

        button("Accent: Switch color", function()
            RED = RED == Color3.fromRGB(190, 20, 50)
                and Color3.fromRGB(100, 30, 180)
                or Color3.fromRGB(190, 20, 50)

            top.BackgroundColor3 = RED
            floating.BackgroundColor3 = RED

            for _, obj in ipairs(gui:GetDescendants()) do
                if obj:IsA("UIStroke") then
                    obj.Color = RED
                end
            end
        end)
    end
end

local tabs = {"Player", "Ball", "Reach", "HBE", "Training", "Misc"}

for _, tabName in ipairs(tabs) do
    local tab = create("TextButton", {
        Size = UDim2.new(1, 0, 0, 36),
        BackgroundColor3 = DARK,
        Text = tabName,
        TextColor3 = WHITE,
        Font = Enum.Font.GothamBold,
        TextSize = 13,
    }, sidebar)

    create("UICorner", {CornerRadius = UDim.new(0, 6)}, tab)

    tab.Activated:Connect(function()
        showTab(tabName)
    end)
end

minimize.Activated:Connect(function()
    main.Visible = false
    floating.Visible = true
end)

close.Activated:Connect(function()
    main.Visible = false
    floating.Visible = true
end)

floating.Activated:Connect(function()
    main.Visible = true
    floating.Visible = false
end)

-- Drag window with the top bar.
local draggingWindow = false
local dragStart
local startPosition

top.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        draggingWindow = true
        dragStart = input.Position
        startPosition = main.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if draggingWindow and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        local delta = input.Position - dragStart

        main.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        draggingWindow = false
    end
end)

UIS.InputBegan:Connect(function(input, processed)
    if not processed and input.KeyCode == Enum.KeyCode.RightShift then
        main.Visible = not main.Visible
        floating.Visible = not main.Visible
    end
end)

showTab("Player")
print("Souls Hub loaded successfully.")

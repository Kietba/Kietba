-- Création de l'interface utilisateur
local TweenService = game:GetService("TweenService")
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ScrollingFrame = Instance.new("ScrollingFrame")
local UIListLayout = Instance.new("UIListLayout")

-- Propriétés de base du GUI
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
MainFrame.Size = UDim2.new(0, 300, 0, 400)
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
MainFrame.Active = true
MainFrame.Draggable = false -- Désactiver la propriété Draggable pour une meilleure prise en charge mobile
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui
MainFrame.BorderSizePixel = 0
MainFrame.BackgroundTransparency = 0.2

-- Ajout d'un coin arrondi
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 15)
UICorner.Parent = MainFrame

-- Création d'un bouton de déplacement
local DragFrame = Instance.new("TextButton")
DragFrame.Size = UDim2.new(1, 0, 0.1, 0)
DragFrame.Position = UDim2.new(0, 0, 0, 0)
DragFrame.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
DragFrame.Text = "RemoteEvent Gui"
DragFrame.Parent = MainFrame
DragFrame.BorderSizePixel = 0

local DragCorner = Instance.new("UICorner")
DragCorner.CornerRadius = UDim.new(0, 10)
DragCorner.Parent = DragFrame

-- Animation de déplacement
local UIS = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

DragFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

DragFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        local goal = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        
        local tween = TweenService:Create(MainFrame, TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Position = goal})
        tween:Play()
    end
end)

-- Configuration du ScrollingFrame
ScrollingFrame.Size = UDim2.new(1, 0, 0.7, 0)
ScrollingFrame.Position = UDim2.new(0, 0, 0.1, 0)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 5, 0)
ScrollingFrame.Parent = MainFrame
ScrollingFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ScrollingFrame.ScrollBarThickness = 5

UIListLayout.Parent = ScrollingFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Fonction pour scanner les RemoteEvents
function scanRemoteEvents()
    for _, obj in pairs(game:GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            createRemoteButton(obj)
        end
    end
end

-- Fonction pour créer un bouton pour chaque RemoteEvent
function createRemoteButton(remote)
    local RemoteButton = Instance.new("TextButton")
    RemoteButton.Size = UDim2.new(1, 0, 0, 30)
    RemoteButton.Text = remote.Name
    RemoteButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    RemoteButton.Parent = ScrollingFrame
    
    -- Ajout de coins arrondis
    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 10)
    ButtonCorner.Parent = RemoteButton
    
    -- Bouton Fire
    RemoteButton.MouseButton1Click:Connect(function()
        remote:FireServer()
    end)
    
    -- Bouton LoopFire
    local Loop = false
    local LoopButton = Instance.new("TextButton")
    LoopButton.Size = UDim2.new(0.3, 0, 1, 0)
    LoopButton.Position = UDim2.new(0.7, 0, 0, 0)
    LoopButton.Text = "Loop"
    LoopButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    LoopButton.Parent = RemoteButton
    
    local LoopCorner = Instance.new("UICorner")
    LoopCorner.CornerRadius = UDim.new(0, 10)
    LoopCorner.Parent = LoopButton
    
    LoopButton.MouseButton1Click:Connect(function()
        Loop = not Loop
        LoopButton.BackgroundColor3 = Loop and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
        while Loop do
            remote:FireServer()
            wait(0.1)
        end
    end)
end

-- Scanner les RemoteEvents au lancement du script
scanRemoteEvents()

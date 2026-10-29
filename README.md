-- // SH Admin Smart Chat UI (Optimized & Fast Arabic Text) 🐣🔥
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- حذف أي واجهة قديمة حتى ما يصير تكرار
if PlayerGui:FindFirstChild("SH_AdminChatUI") then
    PlayerGui.SH_AdminChatUI:Destroy()
end

-- إنشاء الـ ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SH_AdminChatUI"
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false

-- الإطار الرئيسي (على اليمين وفوق بشوي)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 45)
MainFrame.Position = UDim2.new(1, -230, 0.35, 0) -- رفعناه لفوق شوي (0.35 بدل 0.5)
MainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 50) -- رصاصي داكن فخم
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

-- حواف ناعمة
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- إطار خفيف جمالي (Stroke)
local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(200, 50, 50) -- لمسة حمراء ستايل SH
UIStroke.Thickness = 1.5
UIStroke.Parent = MainFrame

-- خانة الكتابة (TextBox)
local TextBox = Instance.new("TextBox")
TextBox.Name = "CommandInput"
TextBox.Size = UDim2.new(1, -40, 1, 0)
TextBox.Position = UDim2.new(0, 10, 0, 0)
TextBox.BackgroundTransparency = 1
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.PlaceholderText = "اكتب أمر أو كلام..."
TextBox.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
TextBox.Text = ""
TextBox.TextSize = 14
TextBox.Font = Enum.Font.GothamBold
TextBox.TextXAlignment = Enum.TextXAlignment.Left
TextBox.Parent = MainFrame

-- زر الإرسال / أيقونة 🐣
local SendButton = Instance.new("TextButton")
SendButton.Name = "SendButton"
SendButton.Size = UDim2.new(0, 30, 0, 30)
SendButton.Position = UDim2.new(1, -35, 0.5, -15)
SendButton.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
SendButton.Text = "🐣"
SendButton.TextSize = 16
SendButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SendButton.BorderSizePixel = 0
SendButton.Parent = MainFrame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 8)
ButtonCorner.Parent = SendButton

-- وظيفة الإرسال السريع جداً بدون أي تأخير
local function SendCommand()
    local text = TextBox.Text
    if text and text ~= "" then
        local finalCommand = ""
        
        -- إذا النص يبدأ بـ ; يرسله وحده، وإذا لا يضيف قبله ;to
        if string.sub(text, 1, 1) == ";" then
            finalCommand = text
        else
            finalCommand = ";to " .. text -- تصحيح عملية الدمج بعلامة .. السريعة باللوا
        end
        
        local args = {
            [1] = finalCommand
        }
        
        -- إرسال فوري بدون pcall بطيء أو انتظار زمني
        ReplicatedStorage.HDAdminHDClient.Signals.RequestCommand:InvokeServer(unpack(args))
        
        TextBox.Text = ""
    end
end

-- الإرسال الفوري عند الضغط على Enter
TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        SendCommand()
    end
end)

-- الإرسال بالنقر على الزر 🐣
SendButton.MouseButton1Click:Connect(SendCommand)

print("--- [SH] Fast Admin Chat UI Loaded Successfully! ---")

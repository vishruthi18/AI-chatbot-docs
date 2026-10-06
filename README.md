<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Attendance Management System</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>

    <!-- Chatbot -->
    <div class="chatbot" id="chatbot">

        <div class="chat-header">
            <span>🤖 Attendance AI Assistant</span>
            <button onclick="closeChatbot()">×</button>
        </div>

        <div class="chat-messages" id="chatMessages">
            <div class="bot-message">
                Hello! 👋 I'm your Attendance AI Assistant.
                <br><br>
                You can ask me about:
                <br>• Student registration
                <br>• Marking attendance
                <br>• Attendance reports
                <br>• Viewing attendance
                <br>• System usage
            </div>
        </div>

        <div class="quick-buttons">
            <button onclick="askQuestion('How do I register a student?')">
                Register Student
            </button>

            <button onclick="askQuestion('How do I mark attendance?')">
                Mark Attendance
            </button>

            <button onclick="askQuestion('How can I generate a report?')">
                Reports
            </button>
        </div>

        <div class="chat-input">
            <input
                type="text"
                id="userInput"
                placeholder="Ask something..."
                onkeypress="handleEnter(event)"
            >

            <button onclick="sendMessage()">Send</button>
        </div>

    </div>

    <script src="script.js"></script>

</body>
</html>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f4f7fb;
    color: #333;
}

/* Header */

header {
    background: linear-gradient(135deg, #2563eb, #4f46e5);
    color: white;
    text-align: center;
    padding: 30px;
}

header h1 {
    margin-bottom: 10px;
}

/* Dashboard */

.dashboard {
    width: 90%;
    max-width: 1000px;
    margin: 40px auto;
    text-align: center;
}

.dashboard h2 {
    margin-bottom: 30px;
}

.cards {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
}

.card {
    background: white;
    width: 250px;
    padding: 30px;
    border-radius: 12px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.card h3 {
    color: #2563eb;
    margin-bottom: 15px;
}

.card p {
    font-size: 18px;
}

/* Dashboard button */

.dashboard > button {
    margin-top: 30px;
    padding: 14px 25px;
    border: none;
    background: #2563eb;
    color: white;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
}

.dashboard > button:hover {
    background: #1d4ed8;
}

/* Chatbot */

.chatbot {
    position: fixed;
    right: 25px;
    bottom: 25px;
    width: 370px;
    max-width: calc(100% - 40px);
    height: 550px;
    background: white;
    border-radius: 15px;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.25);

    display: none;
    flex-direction: column;
    overflow: hidden;
}

/* Chat header */

.chat-header {
    background: linear-gradient(135deg, #2563eb, #4f46e5);
    color: white;
    padding: 18px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
}

.chat-header button {
    background: none;
    border: none;
    color: white;
    font-size: 25px;
    cursor: pointer;
}

/* Messages */

.chat-messages {
    flex: 1;
    padding: 15px;
    overflow-y: auto;
    background: #f8fafc;
}

.bot-message,
.user-message {
    padding: 12px 15px;
    margin-bottom: 12px;
    max-width: 85%;
    border-radius: 12px;
    line-height: 1.5;
}

.bot-message {
    background: #e5e7eb;
    color: #111827;
}

.user-message {
    background: #2563eb;
    color: white;
    margin-left: auto;
}

/* Quick buttons */

.quick-buttons {
    padding: 8px;
    display: flex;
    gap: 5px;
    flex-wrap: wrap;
    border-top: 1px solid #ddd;
}

.quick-buttons button {
    border: none;
    background: #eef2ff;
    color: #3730a3;
    padding: 7px 9px;
    border-radius: 15px;
    cursor: pointer;
    font-size: 11px;
}

/* Input */

.chat-input {
    display: flex;
    border-top: 1px solid #ddd;
}

.chat-input input {
    flex: 1;
    padding: 15px;
    border: none;
    outline: none;
    font-size: 14px;
}

.chat-input button {
    padding: 0 18px;
    border: none;
    background: #2563eb;
    color: white;
    cursor: pointer;
}

.chat-input button:hover {
    background: #1d4ed8;
}

/* Mobile */

@media (max-width: 600px) {

    .chatbot {
        right: 10px;
        bottom: 10px;
        width: calc(100% - 20px);
        height: 80vh;
    }

    header h1 {
        font-size: 24px;
    }
}
// Student count example
let students = [];

document.getElementById("studentCount").textContent = students.length;


// Open chatbot
function openChatbot() {
    document.getElementById("chatbot").style.display = "flex";
}


// Close chatbot
function closeChatbot() {
    document.getElementById("chatbot").style.display = "none";
}


// Send message
function sendMessage() {

    const input = document.getElementById("userInput");
    const message = input.value.trim();

    if (message === "") {
        return;
    }

    addUserMessage(message);

    input.value = "";

    // Show typing message
    const typing = document.createElement("div");
    typing.className = "bot-message";
    typing.id = "typing";
    typing.textContent = "AI is typing...";
    
    document.getElementById("chatMessages").appendChild(typing);

    setTimeout(() => {

        typing.remove();

        const response = getBotResponse(message);

        addBotMessage(response);

    }, 600);
}


// Add user message
function addUserMessage(message) {

    const chatMessages = document.getElementById("chatMessages");

    const messageElement = document.createElement("div");

    messageElement.className = "user-message";

    messageElement.textContent = message;

    chatMessages.appendChild(messageElement);

    scrollChat();
}


// Add bot message
function addBotMessage(message) {

    const chatMessages = document.getElementById("chatMessages");

    const messageElement = document.createElement("div");

    messageElement.className = "bot-message";

    messageElement.innerHTML = message;

    chatMessages.appendChild(messageElement);

    scrollChat();
}


// Quick question
function askQuestion(question) {

    document.getElementById("userInput").value = question;

    sendMessage();
}


// Enter key
function handleEnter(event) {

    if (event.key === "Enter") {
        sendMessage();
    }
}


// Scroll chat
function scrollChat() {

    const chatMessages = document.getElementById("chatMessages");

    chatMessages.scrollTop = chatMessages.scrollHeight;
}


// AI chatbot response system
function getBotResponse(message) {

    const text = message.toLowerCase();


    // Greeting
    if (
        text.includes("hello") ||
        text.includes("hi") ||
        text.includes("hey")
    ) {
        return `
            Hello! 👋<br>
            I'm your Attendance AI Assistant.<br><br>
            How can I help you today?
        `;
    }


    // Student registration
    if (
        text.includes("register") ||
        text.includes("add student") ||
        text.includes("new student")
    ) {
        return `
            🧑‍🎓 <b>Student Registration</b><br><br>

            To register a student:<br>
            1. Open the Student Registration section.<br>
            2. Enter the student's name.<br>
            3. Enter the student ID.<br>
            4. Select the class.<br>
            5. Click <b>Register Student</b>.
        `;
    }


    // Mark attendance
    if (
        text.includes("mark attendance") ||
        text.includes("attendance")
    ) {

        if (text.includes("mark")) {
            return `
                ✅ <b>How to Mark Attendance</b><br><br>

                1. Login to the system.<br>
                2. Select your class.<br>
                3. Select the date.<br>
                4. Mark each student as Present or Absent.<br>
                5. Click <b>Save Attendance</b>.
            `;
        }

        return `
            📋 Attendance allows you to record whether
            students are present or absent for a particular class and date.
            <br><br>
            You can also generate attendance reports.
        `;
    }


    // View attendance
    if (
        text.includes("view attendance") ||
        text.includes("check attendance") ||
        text.includes("attendance status")
    ) {
        return `
            📊 <b>View Attendance</b><br><br>

            Select the class and date to view the attendance
            status of students.<br><br>

            You can check whether each student is
            <b>Present</b> or <b>Absent</b>.
        `;
    }


    // Reports
    if (
        text.includes("report") ||
        text.includes("generate")
    ) {
        return `
            📈 <b>Attendance Reports</b><br><br>

            To generate a report:<br>
            1. Open the Reports section.<br>
            2. Select the class.<br>
            3. Select the date range.<br>
            4. Click <b>Generate Report</b>.<br><br>

            The system can display attendance information
            for the selected period.
        `;
    }


    // Login
    if (
        text.includes("login") ||
        text.includes("sign in")
    ) {
        return `
            🔐 <b>Login</b><br><br>

            Enter your username and password on the
            login page and click <b>Login</b>.<br><br>

            After logging in, you can access classes,
            attendance and reports.
        `;
    }


    // Class
    if (
        text.includes("class") ||
        text.includes("select class")
    ) {
        return `
            🏫 <b>Select Class</b><br><br>

            After logging in, select the class you want
            to manage from the class selection menu.
        `;
    }


    // Help
    if (
        text.includes("help") ||
        text.includes("what can you do")
    ) {
        return `
            🤖 <b>I can help you with:</b><br><br>

            • Student registration<br>
            • Marking attendance<br>
            • Viewing attendance<br>
            • Generating reports<br>
            • Login<br>
            • Selecting classes<br><br>

            Try asking: <i>"How do I mark attendance?"</i>
        `;
    }


    // Thank you
    if (
        text.includes("thank") ||
        text.includes("thanks")
    ) {
        return `
            You're welcome! 😊<br>
            I'm always here to help with the attendance system.
        `;
    }


    // Default response
    return `
        🤔 I'm not sure about that yet.<br><br>

        Try asking me something like:<br>
        • "How do I register a student?"<br>
        • "How do I mark attendance?"<br>
        • "How can I generate a report?"<br>
        • "How do I view attendance?"
    `;
}

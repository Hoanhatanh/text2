<!doctype html>


    
    
    iOS Chat
    <script type="module">
      import { initializeApp } from "https://www.gstatic.com/firebasejs/11.3.1/firebase-app.js";
      import { getDatabase, ref, push, onChildAdded } from "https://www.gstatic.com/firebasejs/11.3.1/firebase-database.js";

      const firebaseConfig = {
        apiKey: "AIzaSyAoQh03WjWZk_6_Qfud8MuKFzCt5CfMGrE",
        authDomain: "cach-b5630.firebaseapp.com",
        databaseURL: "https://cach-b5630-default-rtdb.firebaseio.com",
        projectId: "cach-b5630",
        storageBucket: "cach-b5630.appspot.com",
        messagingSenderId: "602633871142",
        appId: "1:602633871142:web:b5055dd3e2821fc91d6be7",
        measurementId: "G-C7Q4YL908V"
      };

      const app = initializeApp(firebaseConfig);
      const db = getDatabase(app);

      document.addEventListener("DOMContentLoaded", () => {
        const sendBtn = document.getElementById("sendBtn");
        const messageInput = document.getElementById("messageInput");
        const usernameInput = document.getElementById("usernameInput");
        const chatBox = document.getElementById("chatBox");

        sendBtn.addEventListener("click", () => {
          const username = usernameInput.value.trim() || "Ẩn danh";
          const message = messageInput.value.trim();

          if (message) {
            push(ref(db, "messages"), {
              user: username,
              text: message,
              timestamp: Date.now()
            });
            messageInput.value = "";
          }
        });

        onChildAdded(ref(db, "messages"), (snapshot) => {
          const data = snapshot.val();
          const messageElement = document.createElement("div");
          messageElement.classList.add("message");
          if (data.user === usernameInput.value) {
            messageElement.classList.add("my-message");
          }
          messageElement.innerHTML = `<strong>${data.user}:</strong> ${data.text}`;
          chatBox.appendChild(messageElement);
          chatBox.scrollTop = chatBox.scrollHeight;
        });
      });
    </script>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: Arial, sans-serif;
            background: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .chat-container {
            width: 100%;
            max-width: 400px;
            background: white;
            border-radius: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }
        .chat-header {
            background: #007aff;
            color: white;
            padding: 15px;
            text-align: center;
            font-size: 18px;
            font-weight: bold;
        }
        .chat-box {
            flex: 1;
            height: 300px;
            overflow-y: auto;
            padding: 10px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            background: #f9f9f9;
        }
        .message {
            padding: 10px;
            border-radius: 8px;
            max-width: 70%;
            word-wrap: break-word;
        }
        .my-message {
            align-self: flex-end;
            background: #007aff;
            color: white;
        }
        .message:not(.my-message) {
            align-self: flex-start;
            background: #e5e5ea;
            color: black;
        }
        .chat-input {
            display: flex;
            padding: 10px;
            background: #f1f1f1;
            border-top: 1px solid #ddd;
            gap: 5px;
        }
        .chat-input #usernameInput {
            width: 30%; /* Giảm xuống 50% so với trước */
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .chat-input #messageInput {
            flex: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .chat-input button {
            background: #007aff;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            white-space: nowrap;
        }
        @media (max-width: 400px) {
            .chat-input {
                flex-wrap: wrap;
            }
            .chat-input #usernameInput {
                width: 100%; /* Khi màn hình nhỏ, input sẽ full */
            }
            .chat-input #messageInput {
                flex: 100%;
            }
            .chat-input button {
                width: 100%;
            }
        }
    </style>


    <div class="chat-container">
        <div class="chat-header">iOS Chat</div>
        <div class="chat-box" id="chatBox"></div>
        <div class="chat-input">
            <input type="text" id="usernameInput" placeholder="Tên của bạn" />
            <input type="text" id="messageInput" placeholder="Nhập tin nhắn..." />
            <button id="sendBtn">Gửi</button>
        </div>
    </div>

</!doctype>

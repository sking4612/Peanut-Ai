# Peanut-Ai
A simple AI assistant powered by OpenAI
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Peanut AI Assistant (Secure)</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f3f3f3; padding: 40px; text-align: center; }
    button { padding: 15px 30px; font-size: 18px; background-color: #4CAF50; color: white; border: none; border-radius: 10px; cursor: pointer; }
    #chatbox { width: 90%; max-width: 500px; background: white; border-radius: 10px; margin: 30px auto; padding: 20px; box-shadow: 0 0 10px #ccc; }
    #messages { height: 300px; overflow-y: auto; text-align: left; padding: 10px; border: 1px solid #eee; }
    .msg { margin-bottom: 15px; }
    .user { color: #4CAF50; font-weight: bold; }
    .bot { color: #333; }
    #input { width: 100%; padding: 10px; font-size: 16px; margin-top: 10px; }
  </style>
</head>
<body>

  <h1>🧠 Peanut AI Assistant</h1>
  <button onclick="payWithPeanut()">Pay with Peanut</button>

  <div id="chatbox">
    <div id="messages"></div>
    <input id="input" placeholder="Ask Peanut anything..." onkeydown="if(event.key==='Enter') sendMessage()" />
  </div>

  <script>
    const OPENAI_API_KEY = "sk‑proj‑gcidQFdNqAW09vtqbt…"; // Your original key

    function payWithPeanut() {
      alert("✅ Payment successful using Peanut!");
    }

    async function sendMessage() {
      const input = document.getElementById("input");
      const text = input.value.trim();
      if (!text) return;
      appendMessage("user", text);
      input.value = "";
      appendMessage("bot", "Thinking...");
      const reply = await askGPT(text);
      updateLastBotMessage(reply);
    }

    function appendMessage(role, text) {
      const msg = document.createElement("div");
      msg.className = "msg " + role;
      msg.innerHTML = `<span class="${role}">${role === "user" ? "You" : "Peanut"}:</span> ${text}`;
      document.getElementById("messages").appendChild(msg);
      document.getElementById("messages").scrollTop = 9999;
    }

    function updateLastBotMessage(newText) {
      const messages = document.querySelectorAll(".msg.bot");
      if (messages.length) {
        messages[messages.length - 1].innerHTML = `<span class="bot">Peanut:</span> ${newText}`;
      }
    }

    async function askGPT(question) {
      try {
        const res = await fetch("https://corsproxy.io/?https://api.openai.com/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${OPENAI_API_KEY}`
          },
          body: JSON.stringify({
            model: "gpt-3.5-turbo",
            messages: [
              { role: "system", content: "You are Peanut, a smart and helpful assistant." },
              { role: "user", content: question }
            ]
          })
        });
        const data = await res.json();
        return data.choices?.[0]?.message?.content ?? "❌ No response from API.";
      } catch {
        return "❌ Error contacting OpenAI.";
      }
    }
  </script>

</body>
</html>

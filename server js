import express from "express";
import bodyParser from "body-parser";
import fetch from "node-fetch";
import path from "path";
import { fileURLToPath } from "url";

const app = express();
const PORT = process.env.PORT || 3000;

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

app.use(bodyParser.json());
app.use(express.static("public"));

// Chat endpoint
app.post("/chat", async (req, res) => {
  const { message } = req.body;

  try {
    // 🔗 Simple plugin example: Weather
    if (message.toLowerCase().includes("weather")) {
      const fakeWeather = "Sunny, 29°C in Lagos 🌞";
      return res.json({ reply: `According to my nano-banana 🌐: ${fakeWeather}` });
    }

    // 🔗 Call OpenAI
    const response = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${process.env.OPENAI_API_KEY}`
      },
      body: JSON.stringify({
        model: "gpt-4o-mini",  // 2025 upgrade 🚀
        messages: [{ role: "user", content: message }],
        stream: false
      })
    });

    const data = await response.json();

    if (data.error) {
      return res.status(500).json({ reply: "Error: " + data.error.message });
    }

    const reply = data.choices[0].message.content;
    res.json({ reply });
  } catch (err) {
    res.status(500).json({ reply: "Something went wrong 🤖🍌" });
  }
});

app.listen(PORT, () => console.log(`✅ Akin AI running on port ${PORT}`));

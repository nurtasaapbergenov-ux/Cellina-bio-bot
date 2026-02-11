<!DOCTYPE html>
<html>
<head>
  <title>Cellina</title>
  <style>
    body { font-family: Arial; max-width: 600px; margin: auto; padding: 20px; }
    input { width: 80%; padding: 8px; }
    button { padding: 8px 12px; }
    #answer { margin-top: 20px; background: #f0f0f0; padding: 10px; border-radius: 5px; }
  </style>
</head>
<body>
  <h2>🧬 BioBot — спроси про биологию</h2>
  <input id="question" placeholder="Напиши вопрос..." />
  <button onclick="ask()">Спросить</button>
  <p id="answer"></p>

<script>
async function ask() {
  let q = document.getElementById("question").value;

  let res = await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Bearer ТВОЙ_API_KEY"
    },
    body: JSON.stringify({
      model: "gpt-4.1-mini",
      messages: [
        {role:"system", content:"Ты дружелюбный учитель биологии для школьников."},
        {role:"user", content:q}
      ]
    })
  });

  let data = await res.json();
  document.getElementById("answer").innerText =
    data.choices[0].message.content;
}
</script>
</body>
</html>

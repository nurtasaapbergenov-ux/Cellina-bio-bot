<!DOCTYPE html>
<html>
<head>
  <title>BioBot</title>
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

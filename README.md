<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Cellina BioBot</title>
  <style>
    body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; max-width: 600px; margin: 40px auto; padding: 20px; line-height: 1.6; }
    .container { border: 1px solid #ddd; padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
    input { width: 75%; padding: 12px; border: 1px solid #ccc; border-radius: 8px; font-size: 16px; }
    button { padding: 12px 20px; background: #28a745; color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 16px; }
    button:hover { background: #218838; }
    #answer { margin-top: 20px; background: #f9f9f9; padding: 15px; border-left: 5px solid #28a745; min-height: 50px; white-space: pre-wrap; }
  </style>
</head>
<body>
  <div class="container">
    <h2>🧬 BioBot — уроки биологии</h2>
    <input id="question" placeholder="Например: Как работает фотосинтез?" />
    <button onclick="ask()">Спросить</button>
    <div id="answer">Ответ появится здесь...</div>
  </div>

<script>
async function ask() {
  const q = document.getElementById("question").value;
  const answerDiv = document.getElementById("answer");
  
  if(!q) return alert("Введите вопрос!");
  
  answerDiv.innerText = "⏳ Селлина думает...";

  try {
    const res = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer ТВОЙ_API_KEY" // Вставь свой ключ сюда
      },
      body: JSON.stringify({
        model: "gpt-4o-mini", // Используем актуальную модель
        messages: [
          {role: "system", content: "Ты дружелюбный и понятный учитель биологии для детей. Объясняй сложные вещи простыми словами."},
          {role: "user", content: q}
        ]
      })
    });

    const data = await res.json();
    if (data.error) {
        answerDiv.innerText = "Ошибка: " + data.error.message;
    } else {
        answerDiv.innerText = data.choices[0].message.content;
    }
  } catch (err) {
    answerDiv.innerText = "Произошла ошибка при подключении к API.";
  }
}
</script>
</body>
</html>

<script>
  function addEntry(event, type) {
    event.preventDefault();
    const form = event.target;
    const inputs = form.querySelectorAll('input');
    let text = "";
    inputs.forEach(input => {
      if (input.value.trim()) {
        text += `<strong>${input.placeholder}</strong>: ${input.value}<br>`;
      }
      input.value = ""; // очистить поле
    });

    const container = document.getElementById(`${type}-entries`);
    const div = document.createElement("div");
    div.className = "entry";
    div.innerHTML = text;
    container.prepend(div);
  }
</script>
<script>
  const endpoint = "https://script.google.com/macros/s/AKfycbxRPv_2wa6-FdHkOwihGblH-erTVc1-VfzGwUctml8yLimUUTiVrFQLFLPVZWspYRKB1A/exec"; // заменишь своим

  function addEntry(event, type) {
    event.preventDefault();
    const form = event.target;
    const inputs = form.querySelectorAll('input');
    let text = "";
    let previewHTML = "";

    inputs.forEach(input => {
      if (input.value.trim()) {
        text += `${input.placeholder}: ${input.value}\n`;
        previewHTML += `<strong>${input.placeholder}</strong>: ${input.value}<br>`;
      }
    });

    // Показать на сайте
    const container = document.getElementById(`${type}-entries`);
    const div = document.createElement("div");
    div.className = "entry";
    div.innerHTML = previewHTML;
    container.prepend(div);

    // Отправить на Google Таблицу
    fetch(endpoint, {
      method: "POST",
      body: JSON.stringify({ type: type, content: text }),
      headers: {
        "Content-Type": "application/json"
      }
    }).then(res => {
      alert("✅ Отправлено и сохранено!");
    }).catch(err => {
      alert("❌ Ошибка отправки");
    });

    inputs.forEach(input => input.value = ""); // очистить форму
  }
</script>

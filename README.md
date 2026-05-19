// 送出數據去 Google Sheet API
  function submitToSheet() {
    const statusText = document.getElementById('statusText');
    statusText.innerText = "正在儲存...";
    statusText.style.color = "#8e8e93";

    // 將呢度換成你頭先複製嘅 GAS Web App URL !!!
    const scriptUrl = 'https://script.google.com/macros/s/你嘅部署ID/exec'; 

    const payload = new URLSearchParams({
      orderNum: document.getElementById('orderNum').value.trim(),
      skuNum: document.getElementById('skuNum').value.trim(),
      snNum: document.getElementById('snNum').value.trim(),
      quantity: document.getElementById('quantity').value,
      whNum: document.getElementById('whNum').value.trim()
    });

    // 透過 GET Request 傳送數據
    fetch(`${scriptUrl}?${payload.toString()}`)
      .then(response => response.json())
      .then(data => {
        if(data.status === "success") {
          statusText.innerText = data.message;
          statusText.style.color = "#34c759";
          
          // 成功後清空欄位
          document.getElementById('orderNum').value = '';
          document.getElementById('skuNum').value = '';
          document.getElementById('snNum').value = '';
          document.getElementById('quantity').value = '1';
        }
      })
      .catch(error => {
        statusText.innerText = "儲存失敗: " + error;
        statusText.style.color = "#ff3b30";
      });
  }

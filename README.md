<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>QR Code Generator</title>
  <style>
    body {
      background-color: #f8eddc;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      font-family: Arial, sans-serif;
    }

    .container {
      background: white;
      padding: 20px;
      border-radius: 8px;
      text-align: center;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    input {
      width: 80%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }

    button {
      background-color: #4CAF50;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin: 5px;
    }

    button:hover {
      background-color: #45a049;
    }

    #qrcode {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>QR Code Generator</h2>
    <input type="text" id="urlInput" placeholder="Enter URL here" />
    <button onclick="generateAndDownload()">Click Here to Generate & Download</button>
    <div id="qrcode"></div>
  </div>

  <!-- QRCode Library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  
  <script>
    function generateAndDownload() {
      const text = document.getElementById('urlInput').value.trim();
      const container = document.getElementById('qrcode');
      container.innerHTML = "";

      if (!text) {
        alert("অনুগ্রহ করে URL লিখুন!");
        return;
      }

      // QR কোড জেনারেট
      new QRCode(container, {
        text,
        width: 200,
        height: 200,
      });

      // একটু অপেক্ষা করে ইমেজ এলে ডাউনলোড ট্রিগার
      setTimeout(() => {
        // QRCode.js সাধারণত <img> এলিমেন্ট তৈরি করে
        const img = container.querySelector('img');
        if (img && img.src) {
          const link = document.createElement('a');
          link.href = img.src;
          link.download = 'qrcode.png';
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
        } else {
          // যদি ইমেজ না মেলে, ফallback—ইমেজ নতুন ট্যাবে ওপেন
          const svg = container.querySelector('svg');
          if (svg) {
            const xml = new XMLSerializer().serializeToString(svg);
            const svg64 = btoa(xml);
            const b64start = 'data:image/svg+xml;base64,';
            const image64 = b64start + svg64;
            const link2 = document.createElement('a');
            link2.href = image64;
            link2.download = 'qrcode.svg';
            document.body.appendChild(link2);
            link2.click();
            document.body.removeChild(link2);
          } else {
            alert("QR কোড ইমেজ পাওয়া যায়নি!");
          }
        }
      }, 500);
    }
  </script>
</body>
</html>

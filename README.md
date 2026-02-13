<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>ডুপ্লিকেট নাম চেকার - একদম সেম নাম</title>
  <style>
    body { font-family: sans-serif; padding: 15px; background: #0d1117; color: #c9d1d9; line-height: 1.6; }
    h2, h3 { color: #58a6ff; }
    textarea { width: 100%; height: 320px; background: #161b22; color: #e6edf3; border: 1px solid #30363d; padding: 12px; font-family: monospace; font-size: 15px; border-radius: 6px; }
    button { background: #238636; color: white; border: none; padding: 12px 30px; margin: 15px 0; border-radius: 6px; font-size: 17px; cursor: pointer; }
    button:hover { background: #2ea043; }
    #result { background: #161b22; border: 1px solid #30363d; padding: 15px; border-radius: 6px; white-space: pre-wrap; font-family: monospace; }
    .dup-item { margin: 15px 0; padding: 10px; background: #1f2937; border-radius: 6px; }
    .name { font-weight: bold; color: #79c0ff; font-size: 18px; }
    .count { color: #f85149; font-size: 16px; }
    .numbers { color: #a5d6ff; }
  </style>
</head>
<body>
  <h2>ডুপ্লিকেট চেকার (একদম সেম পুরো নাম)</h2>
  <p>লিস্ট পেস্ট করুন (যেমন আছে ঠিক তেমনি):</p>
  
  <textarea id="input" placeholder="1️⃣➤@Akter Bhai 
2️⃣➤@MD Meheraj 
..."></textarea>
  
  <button onclick="check()">চেক করো</button>
  
  <h3>যাদের পুরো নাম একদম সেম হয়ে একাধিকবার এসেছে:</h3>
  <div id="result">এখনো চেক করা হয়নি...</div>

  <script>
    function check() {
      const text = document.getElementById('input').value.trim();
      if (!text) { alert("লিস্ট দিন!"); return; }

      const lines = text.split('\n');
      const nameToPos = new Map();

      lines.forEach(line => {
        line = line.trim();
        if (!line) return;

        // নাম্বার বের করা
        const numMatch = line.match(/^(\d+)[️⃣➤]/);
        if (!numMatch) return;
        const num = numMatch[1];

        // পুরো @... টা বের করা (লাইনের শেষ পর্যন্ত)
        const userMatch = line.match(/@.+/);
        if (userMatch) {
          const username = userMatch[0].trim();
          if (username.length > 1) {  // খালি @ বাদ
            if (!nameToPos.has(username)) nameToPos.set(username, []);
            nameToPos.get(username).push(num);
          }
        }
      });

      let output = '';
      let found = false;

      nameToPos.forEach((positions, name) => {
        if (positions.length > 1) {
          found = true;
          const posStr = positions.join(', ');
          output += `<div class="dup-item">
            <div class="name">${name}</div>
            <div class="count">→ ${positions.length} বার</div>
            <div class="numbers">নাম্বার: ${posStr}</div>
          </div>`;
        }
      });

      document.getElementById('result').innerHTML = found 
        ? output 
        : '<span style="color:#8b949e;">কোনো একদম সেম পুরো নাম ডুপ্লিকেট পাওয়া যায়নি ✓</span>';
    }
  </script>
</body>
</html>

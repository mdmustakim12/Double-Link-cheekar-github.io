<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ডুপ্লিকেট নাম চেকার</title>
    <style>
        body { font-family: sans-serif; padding: 20px; background: #0d1117; color: #c9d1d9; line-height: 1.6; }
        h2 { color: #58a6ff; text-align: center; }
        .container { max-width: 500px; margin: auto; background: #161b22; padding: 20px; border-radius: 10px; border: 1px solid #30363d; }
        textarea { width: 100%; height: 150px; background: #0d1117; color: #e6edf3; border: 1px solid #30363d; border-radius: 6px; padding: 12px; font-family: monospace; box-sizing: border-box; }
        button { width: 100%; background: #238636; color: white; border: none; padding: 12px; margin-top: 15px; border-radius: 6px; cursor: pointer; font-size: 16px; font-weight: bold; }
        button:hover { background: #2ea043; }
        #result { margin-top: 20px; background: #0d1117; padding: 15px; border-radius: 8px; border: 1px solid #30363d; }
        .dup-item { margin-bottom: 15px; padding: 10px; background: #1f2937; border-radius: 6px; border-left: 4px solid #f85149; }
        .name { font-weight: bold; color: #79c0ff; font-size: 18px; }
        .count { color: #f85149; font-size: 14px; }
        .numbers { color: #a5d6ff; font-size: 14px; display: block; margin-top: 5px; }
    </style>
</head>
<body>

<div class="container">
    <h2>ডুপ্লিকেট চেকার (একদম সেম নাম)</h2>
    <p>লিস্ট পেস্ট করুন (যেমন আছে ঠিক তেমনি):</p>
    <textarea id="input" placeholder="১️⃣২️⃣৪️⃣➤@Akter Bhai..."></textarea>
    <button onclick="check()">চেক করো</button>

    <div id="result">
        এখানো চেক করা হয়নি...
    </div>
</div>

<script>
function check() {
    const text = document.getElementById('input').value.trim();
    if (!text) { alert("দয়া করে লিস্ট দিন!"); return; }

    const lines = text.split('\n');
    const nameToPos = new Map();

    lines.forEach(line => {
        const lineTrim = line.trim();
        if (!lineTrim) return;

        // নম্বর এবং নাম আলাদা করার লজিক (➤ চিহ্নের মাধ্যমে)
        if (lineTrim.includes('➤')) {
            const parts = lineTrim.split('➤');
            const numberPart = parts[0].trim(); // নম্বর অংশ
            const namePart = parts[1].trim();   // নাম অংশ

            if (!nameToPos.has(namePart)) {
                nameToPos.set(namePart, []);
            }
            nameToPos.get(namePart).push(numberPart);
        }
    });

    let output = '<h3 style="color:#58a6ff">ডুপ্লিকেট ফলাফল:</h3>';
    let found = false;

    nameToPos.forEach((positions, name) => {
        if (positions.length > 1) {
            found = true;
            output += `
                <div class="dup-item">
                    <span class="name">${name}</span>
                    <span class="count"> — ${positions.length} বার আছে</span>
                    <span class="numbers">সিরিয়াল নম্বর: ${positions.join(', ')}</span>
                </div>`;
        }
    });

    document.getElementById('result').innerHTML = found ? output : '<span style="color:#3fb950">✅ কোনো ডুপ্লিকেট নাম পাওয়া যায়নি!</span>';
}
</script>

</body>
</html>

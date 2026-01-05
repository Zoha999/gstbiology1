<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>গুচ্ছ বায়োলজি চ্যালেঞ্জ - ইউটোপিয়ান একাডেমী</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --accent-color: #27ae60;
            --bg-color: #f4f7f6;
            --error-color: #e74c3c;
        }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: var(--bg-color); margin: 0; padding: 20px; line-height: 1.6; }
        .container { max-width: 800px; margin: auto; background: white; padding: 30px; border-radius: 15px; box-shadow: 0 5px 25px rgba(0,0,0,0.15); }
        
        .header { text-align: center; border-bottom: 3px solid var(--accent-color); margin-bottom: 30px; padding-bottom: 15px; }
        .header h1 { color: var(--primary-color); font-size: clamp(24px, 5vw, 36px); margin-bottom: 5px; text-shadow: 1px 1px 2px #ddd; }
        .header p { color: #555; font-weight: bold; font-size: 20px; margin: 0; }

        .info-section, .quiz-section, .result-section, .admin-section { display: none; }
        .active { display: block; }

        .rules-box { background: #fff3cd; padding: 10px; border-radius: 5px; margin-bottom: 20px; font-size: 14px; border: 1px solid #ffeeba; }

        input[type="text"], input[type="email"] { width: 100%; padding: 12px; margin: 10px 0; border: 2px solid #eee; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        button { background: var(--accent-color); color: white; border: none; padding: 15px 25px; border-radius: 8px; cursor: pointer; font-size: 18px; width: 100%; transition: 0.3s; font-weight: bold; }
        button:hover { background: #219150; transform: translateY(-2px); }

        .timer-container { position: sticky; top: 10px; z-index: 1000; display: flex; justify-content: space-between; align-items: center; background: white; padding: 10px; border-radius: 10px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); margin-bottom: 20px; }
        .timer { background: var(--error-color); color: white; padding: 8px 15px; border-radius: 5px; font-weight: bold; font-size: 20px; }
        
        .question { margin-bottom: 30px; padding: 20px; border-radius: 10px; background: #fff; border: 1px solid #eee; box-shadow: 2px 2px 10px rgba(0,0,0,0.02); }
        .question p { font-size: 18px; font-weight: 600; color: #333; }
        .options label { display: block; margin: 12px 0; cursor: pointer; padding: 10px; border: 1px solid #ddd; border-radius: 6px; transition: 0.2s; }
        .options label:hover { background: #f0fdf4; border-color: var(--accent-color); }
        .options input { margin-right: 10px; transform: scale(1.2); }

        .result-card { background: #f8f9fa; padding: 30px; border-radius: 15px; border: 2px dashed var(--accent-color); }
        .score-box { font-size: 40px; color: var(--accent-color); margin: 20px 0; }
        .neg-info { color: var(--error-color); font-size: 14px; }

        .admin-login-btn { position: fixed; bottom: 10px; left: 10px; opacity: 0.2; background: none; color: #999; border: none; font-size: 12px; cursor: pointer; }
        
        table { width: 100%; border-collapse: collapse; margin-top: 20px; font-size: 14px; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: center; }
        th { background-color: var(--primary-color); color: white; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>🧬 গুচ্ছ বায়োলজি চ্যালেঞ্জ 🧬</h1>
        <p>ইউটোপিয়ান একাডেমী</p>
    </div>

    <div id="info-page" class="info-section active">
        <div class="rules-box">
            <strong>নির্দেশনা:</strong> <br>
            • মোট প্রশ্ন: ৩০টি | সময়: ২০ মিনিট <br>
            • সঠিক উত্তরের জন্য: +১ <br>
            • প্রতিটি ভুল উত্তরের জন্য: <b>-০.৫ (নেগেটিভ মার্ক)</b>
        </div>
        <h3>আপনার তথ্য দিন:</h3>
        <input type="text" id="userName" placeholder="সম্পূর্ণ নাম" required>
        <input type="email" id="userEmail" placeholder="জিমেইল এড্রেস" required>
        <input type="text" id="userCollege" placeholder="কলেজের নাম" required>
        <button onclick="startQuiz()">পরীক্ষা শুরু করুন</button>
    </div>

    <div id="quiz-page" class="quiz-section">
        <div class="timer-container">
            <span style="font-weight: bold; color: #555;">সময় বাকি:</span>
            <div class="timer" id="time">20:00</div>
        </div>
        <form id="quizForm">
            <div id="questions-container"></div>
            <button type="button" onclick="calculateResult()" style="background: var(--primary-color);">ফাইনাল সাবমিট</button>
        </form>
    </div>

    <div id="result-page" class="result-section">
        <div class="result-card" style="text-align: center;">
            <h2>অভিনন্দন! আপনার রেজাল্ট:</h2>
            <div class="score-box" id="score-display">00</div>
            <p id="stats-display" style="font-weight: bold;"></p>
            <p id="time-taken" style="color: #666;"></p>
            <p class="neg-info">*ভুল উত্তরের জন্য ০.৫ করে কাটা হয়েছে।</p>
            <hr>
            <button onclick="location.reload()" style="width: auto; background: #666;">আবার চেষ্টা করুন</button>
        </div>
    </div>

    <div id="admin-page" class="admin-section">
        <h3 style="text-align: center;">অ্যাডমিন কন্ট্রোল প্যানেল</h3>
        <div style="overflow-x: auto;">
            <table>
                <thead>
                    <tr><th>নাম</th><th>কলেজ</th><th>স্কোর</th><th>সময়</th></tr>
                </thead>
                <tbody id="leaderboard"></tbody>
            </table>
        </div>
        <button onclick="location.reload()" style="margin-top:20px; background:#444;">ড্যাশবোর্ড বন্ধ করুন</button>
    </div>
</div>

<button class="admin-login-btn" onclick="adminLogin()">Admin Login</button>

<script>
    const questions = [
        { q: "১. কোষচক্রের কোন পর্যায়ে DNA-এর পরিমাণ দ্বিগুণ হয় কিন্তু ক্রোমোজোম সংখ্যা অপরিবর্তিত থাকে?", options: ["G₁", "S", "G₂", "M"], ans: 1 },
        { q: "২. কোন অঙ্গাণুতে নিজস্ব DNA ও রাইবোজোম উভয়ই থাকে?", options: ["লাইসোজোম", "গলজি বডি", "মাইটোকন্ড্রিয়া", "এন্ডোপ্লাজমিক রেটিকুলাম"], ans: 2 },
        { q: "৩. মানুষের রক্তে সবচেয়ে বেশি পরিমাণে পাওয়া যায়—", options: ["প্লাটিলেট", "RBC", "WBC", "লিম্ফোসাইট"], ans: 1 },
        { q: "৪. উদ্ভিদের কোন হরমোন কোষ দীর্ঘায়নের জন্য দায়ী?", options: ["অক্সিন", "জিব্বেরেলিন", "সাইটোকাইনিন", "এবসিসিক এসিড"], ans: 0 },
        { q: "৫. DNA ও RNA-এর মধ্যে পার্থক্য নয় কোনটি?", options: ["সুগারের ধরন", "নাইট্রোজেন বেস", "ফসফেট গ্রুপ", "হেলিক্স গঠন"], ans: 2 },
        { q: "৬. মানব হৃদপিণ্ডে অক্সিজেনযুক্ত রক্ত প্রথম প্রবেশ করে—", options: ["বাম অলিন্দ", "ডান অলিন্দ", "বাম নিলয়", "ডান নিলয়"], ans: 0 },
        { q: "৭. কোন টিস্যু বিদ্যুৎ সংকেত পরিবহনে বিশেষায়িত?", options: ["পেশি", "সংযোজক", "স্নায়ু", "আবরণী"], ans: 2 },
        { q: "৮. উদ্ভিদের কোন অংশে কেবল জাইলেম ও ফ্লোয়েম থাকে?", options: ["কর্টেক্স", "ভাস্কুলার বান্ডিল", "পিথ", "এপিডার্মিস"], ans: 1 },
        { q: "৯. কোন ভিটামিনের অভাবে রাতকানা রোগ হয়?", options: ["Vit-B₁", "Vit-C", "Vit-D", "Vit-A"], ans: 3 },
        { q: "১০. হরমোন ও এনজাইমের মধ্যে প্রধান পার্থক্য—", options: ["গঠন", "কাজের গতি", "নিঃসরণ স্থান", "সবগুলোই"], ans: 3 },
        { q: "১১. কোনটি অ্যান্টিবডি উৎপাদন করে?", options: ["নিউট্রোফিল", "মনোসাইট", "লিম্ফোসাইট", "ইওসিনোফিল"], ans: 2 },
        { q: "১২. মানবদেহে সবচেয়ে বড় অস্থি কোনটি?", options: ["হিউমেরাস", "ফিমার", "রেডিয়াস", "টিবিয়া"], ans: 1 },
        { q: "১৩. কোন কোষ বিভাজন গ্যামেট সৃষ্টি করে?", options: ["মাইটোসিস", "মিয়োসিস-I", "মিয়োসিস-II", "অ্যামাইটোসিস"], ans: 1 },
        { q: "১৪. নিউক্লিওটাইডের উপাদান নয় কোনটি?", options: ["নাইট্রোজেন বেস", "পেন্টোজ সুগার", "ফসফেট", "অ্যামিনো এসিড"], ans: 3 },
        { q: "১৫. মানবদেহে সর্বাধিক ATP উৎপন্ন হয়—", options: ["গ্লাইকোলাইসিসে", "ক্রেবস চক্রে", "ইলেকট্রন পরিবহন শৃঙ্খলে", "ফারমেন্টেশনে"], ans: 2 },
        { q: "১৬. কোনটি উদ্ভিদের গ্রাউন্ড টিস্যু সিস্টেমের অংশ?", options: ["জাইলেম", "ফ্লোয়েম", "প্যারেনকাইমা", "ক্যাম্বিয়াম"], ans: 2 },
        { q: "১৭. কোনটি ভাইরাসের বৈশিষ্ট্য?", options: ["কোষপ্রাচীর আছে", "নিজস্ব বিপাক ক্রিয়া আছে", "কেবল জীব কোষে বৃদ্ধি পায়", "দ্বিকোষী গঠন"], ans: 2 },
        { q: "১৮. মানব চোখে কোন অংশে ইমেজ তৈরি হয়?", options: ["কর্নিয়া", "লেন্স", "রেটিনা", "আইরিস"], ans: 2 },
        { q: "১৯. কোন গ্রন্থিকে “Master gland” বলা হয়?", options: ["থাইরয়েড", "অ্যাড্রিনাল", "পিটুইটারি", "প্যারাথাইরয়েড"], ans: 2 },
        { q: "২০. শ্বাসক্রিয়ার গ্যাস বিনিময় ঘটে—", options: ["ট্রাকিয়া", "ব্রঙ্কাস", "অ্যালভিওলাই", "ব্রঙ্কিওল"], ans: 2 },
        { q: "২১. উদ্ভিদের কোন হরমোন পাতাঝরা ঘটায়?", options: ["অক্সিন", "জিব্বেরেলিন", "সাইটোকাইনিন", "এবসিসিক এসিড"], ans: 3 },
        { q: "২২. মানবদেহে রক্তচাপ নিয়ন্ত্রণে ভূমিকা রাখে—", options: ["ইনসুলিন", "অ্যাড্রেনালিন", "গ্লুকাগন", "থাইরক্সিন"], ans: 1 },
        { q: "২৩. কোনটি সঠিক জোড়া?", options: ["রাইবোজোম — লিপিড সংশ্লেষ", "SER — প্রোটিন সংশ্লেষ", "RER — প্রোটিন সংশ্লেষ", "গলজি — শক্তি উৎপাদন"], ans: 2 },
        { q: "২৪. কোনটি স্নায়ুতন্ত্রের গঠনগত একক?", options: ["নিউরন", "নিউরোগ্লিয়ার", "সাইন্যাপ্স", "অ্যাক্সন"], ans: 0 },
        { q: "২৫. মানুষের কোন অঙ্গে হ্যাপলয়েড কোষ পাওয়া যায়?", options: ["যকৃত", "অগ্ন্যাশয়", "শুক্রাশয়", "কিডনি"], ans: 2 },
        { q: "২৬. এনজাইমের কাজের উপর সবচেয়ে বেশি প্রভাব ফেলে—", options: ["আলো", "pH ও তাপমাত্রা", "শব্দ", "চাপ"], ans: 1 },
        { q: "২৭. উদ্ভিদের কোন টিস্যু কোষ বিভাজনে সক্রিয়?", options: ["স্থায়ী টিস্যু", "পরিবাহী টিস্যু", "মেরিস্টেম", "আবরণী টিস্যু"], ans: 2 },
        { q: "২৮. কোনটি সংযোজক টিস্যু নয়?", options: ["রক্ত", "অস্থি", "কার্টিলেজ", "এপিথেলিয়াম"], ans: 3 },
        { q: "২৯. মানবদেহে ইউরিয়া উৎপন্ন হয়—", options: ["কিডনি", "যকৃত", "ফুসফুস", "প্লীহা"], ans: 1 },
        { q: "৩০. কোন প্রক্রিয়ায় উদ্ভিদ খাদ্য তৈরি করে?", options: ["শ্বাসক্রিয়া", "ট্রান্সপিরেশন", "সালোকসংশ্লেষণ", "পরিবহন"], ans: 2 }
    ];

    let timer;
    let timeLeft = 1200; 
    let startTime;

    function startQuiz() {
        const name = document.getElementById('userName').value.trim();
        const email = document.getElementById('userEmail').value.trim();
        const college = document.getElementById('userCollege').value.trim();

        if(!name || !email || !college) { alert("দয়া করে সব তথ্য দিন!"); return; }

        document.getElementById('info-page').classList.remove('active');
        document.getElementById('quiz-page').classList.add('active');
        
        loadQuestions();
        startTime = new Date();
        timer = setInterval(updateTimer, 1000);
    }

    function loadQuestions() {
        const container = document.getElementById('questions-container');
        questions.forEach((item, index) => {
            let html = `<div class="question">
                <p>${item.q}</p>
                <div class="options">`;
            item.options.forEach((opt, i) => {
                html += `<label><input type="radio" name="q${index}" value="${i}"> ${opt}</label>`;
            });
            html += `</div></div>`;
            container.innerHTML += html;
        });
    }

    function updateTimer() {
        let mins = Math.floor(timeLeft / 60);
        let secs = timeLeft % 60;
        document.getElementById('time').innerHTML = `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        if (timeLeft <= 0) { calculateResult(); }
        timeLeft--;
    }

    function calculateResult() {
        clearInterval(timer);
        let correct = 0;
        let wrong = 0;
        let unanswered = 0;

        questions.forEach((item, index) => {
            const selected = document.querySelector(`input[name="q${index}"]:checked`);
            if (selected) {
                if (parseInt(selected.value) === item.ans) {
                    correct++;
                } else {
                    wrong++;
                }
            } else {
                unanswered++;
            }
        });

        // Negative Marking Calculation
        let totalScore = (correct * 1) - (wrong * 0.5);
        if (totalScore < 0) totalScore = 0; // Negative total can be zeroed or kept as is

        const endTime = new Date();
        const timeSpent = Math.floor((endTime - startTime) / 1000);
        const m = Math.floor(timeSpent / 60);
        const s = timeSpent % 60;

        // Display results
        document.getElementById('quiz-page').classList.remove('active');
        document.getElementById('result-page').classList.add('active');
        document.getElementById('score-display').innerHTML = totalScore.toFixed(1);
        document.getElementById('stats-display').innerHTML = `সঠিক: ${correct} | ভুল: ${wrong} | উত্তর দেননি: ${unanswered}`;
        document.getElementById('time-taken').innerHTML = `সময় লেগেছে: ${m} মিনিট ${s} সেকেন্ড`;

        // Prepare record
        const record = {
            name: document.getElementById('userName').value,
            college: document.getElementById('userCollege').value,
            score: totalScore.toFixed(1),
            time: `${m}m ${s}s`
        };

        // Save locally for admin
        let history = JSON.parse(localStorage.getItem('quiz_results') || "[]");
        history.push(record);
        localStorage.setItem('quiz_results', JSON.stringify(history));

        // Submit to Formspree
        submitToFormspree(record);
    }

    function submitToFormspree(data) {
        const formData = new FormData();
        formData.append("Name", data.name);
        formData.append("College", data.college);
        formData.append("Final Score", data.score);
        formData.append("Time Taken", data.time);
        formData.append("_subject", "New Quiz Submission - " + data.name);

        fetch('https://formspree.io/f/abdullahzoha999@gmail.com', {
            method: 'POST',
            body: formData,
            headers: { 'Accept': 'application/json' }
        });
    }

    function adminLogin() {
        const pass = prompt("অ্যাডমিন পাসওয়ার্ড দিন:");
        if(pass === "admin123") {
            document.querySelector('.container div.active').classList.remove('active');
            document.getElementById('admin-page').classList.add('active');
            const history = JSON.parse(localStorage.getItem('quiz_results') || "[]");
            const tbody = document.getElementById('leaderboard');
            tbody.innerHTML = history.reverse().map(r => `<tr><td>${r.name}</td><td>${r.college}</td><td>${r.score}</td><td>${r.time}</td></tr>`).join('');
        } else {
            alert("অ্যাক্সেস ডিনাইড!");
        }
    }
</script>

</body>
</html>

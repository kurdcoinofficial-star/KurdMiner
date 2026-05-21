<!DOCTYPE html>
<html lang="ku">
<head>
    <meta charset="UTF-8">
    <title>Kurd Miner</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body { background: #000; color: #ffd700; text-align: center; font-family: sans-serif; padding: 20px; }
        .coin { width: 150px; height: 150px; background: gold; border-radius: 50%; margin: 20px auto; cursor: pointer; box-shadow: 0 0 20px #ffd700; }
        button { padding: 10px; margin: 5px; background: #ffd700; border: none; border-radius: 5px; cursor: pointer; }
    </style>
</head>
<body>

    <h1>کۆین: <span id="balance">0</span></h1>
    <div class="coin" onclick="mine()"></div>

    <h3>ئەرکەکان:</h3>
    <button onclick="completeTask('https://t.me/NecherwanGhafour', 10)">سەردانی کەناڵ (10 کۆین)</button>
    
    <div id="ton-connect-button" style="margin-top: 20px;"></div>

    <script>
        const firebaseConfig = { 
            apiKey: "AizaSyC7o8_t_b-x8U8E9N9J9H8Q8S8R8T8Q8P8",
            authDomain: "android-kurdistan.firebaseapp.com",
            databaseURL: "https://android-kurdistan-default-rtdb.firebaseio.com",
            projectId: "android-kurdistan",
            storageBucket: "android-kurdistan.appspot.com",
            messagingSenderId: "94553229105",
            appId: "1:94553229105:web:d259c6298539e6a9f4c3a2"
        };
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        const tg = window.Telegram.WebApp;
        const userId = tg.initDataUnsafe.user ? tg.initDataUnsafe.user.id : "guest_123";
        const startParam = tg.initDataUnsafe.start_param;

        // سیستەمی ڕیفاڕاڵ
        if (startParam) {
            database.ref('users/' + userId).once('value', (s) => {
                if (!s.exists()) {
                    database.ref('users/' + userId).set({ score: 10 });
                    database.ref('users/' + startParam).once('value', (refUser) => {
                        let refScore = refUser.exists() ? refUser.val().score : 0;
                        database.ref('users/' + startParam).update({ score: refScore + 5 });
                    });
                }
            });
        }

        // تازەکردنەوەی بالانس
        database.ref('users/' + userId).on('value', (s) => {
            document.getElementById('balance').innerText = s.val() ? s.val().score : 0;
        });

        function mine() {
            let current = parseInt(document.getElementById('balance').innerText);
            database.ref('users/' + userId).update({ score: current + 1 });
        }

        function completeTask(url, amount) {
            window.open(url, '_blank');
            let current = parseInt(document.getElementById('balance').innerText);
            database.ref('users/' + userId).update({ score: current + amount });
        }
    </script>
</body>
</html>

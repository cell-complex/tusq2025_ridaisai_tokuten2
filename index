<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8" />
    <title>N○M休</title>
    <style>
        html,
        body {
            height: 100vh;
            margin: 0;
            font-family: "Yu Gothic", sans-serif;
            background: #f6f6f6;
            text-align: center;
            overflow: hidden;
        }

        #controls {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 1vw;
            padding: 0.8vw 0;
        }

        #board {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            grid-template-rows: repeat(2, 1fr);
            gap: 0.8vw;
            justify-items: center;
            align-items: center;
            width: 95%;
            height: 75vh;
            margin: 0 auto;
        }

        .player {
            width: 13vw;
            height: 35vh;
            background: white;
            border-radius: 0.8vw;
            box-shadow: 0 0 0.4vw rgba(0, 0, 0, 0.3);
            padding: 1vw;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            transition: 0.3s;
            font-weight: bold;
            font-size: 4vh;
        }

        .inactive {
            background: #ddd;
            color: #888;
        }

        .winner {
            background: red;
            color: #000;
            font-weight: bold;
        }

        .winner-text {
            font-size: 3vw;
            color: white;
        }

        .score {
            font-size: 4vw;
            font-weight: bold;
        }

        .status {
            font-size: 2vw;
            font-weight: bold;
            height: 2.5vw;
        }

        .lock {
            font-size: 3vw;
            color: black;
            position: relative;
            top: -2vw;
            display: inline-block;
        }

button {
    outline: none;
    border: none;
}
button:focus {
    outline: none;
}
        
        .btns {
            display: flex;
            gap: 0.6vw;
            justify-content: center;
            width: 90%;
            flex-wrap: wrap;
        }

        /* ======== 統一アクションボタン ======== */
        .actionBtn {
            flex: 1;
            aspect-ratio: 1;
            border: none;
            border-radius: 0.8vw;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5vw;
            font-weight: bold;
            transition: 0.2s;
            color: white;
        }

        .action-plus {
            background: red;
        }

        .action-miss {
            background: blue;
        }

        .action-reset {
            background: gray;
        }

        .reset {
            flex: none;
            aspect-ratio: 2;
            width: 85%;
            font-size: 1.5vw;
            background: gray;
            color: white;
            border-radius: 0.6vw;
        }

        .controlBtn {
            min-width: 8vw;
            height: 4vw;
            border: none;
            border-radius: 0.8vw;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
        }

        .GreenBtn {
            background: #4CAF50;
            color: white;
            font-size: 90%;
        }

        .PinkBtn {
            background: rgb(255, 0, 183);
            color: white;
            font-size: 70%;
        }

        #controls input[type="number"] {
            width: 3vw;
            padding: 1vw 1vw;
            border: 0.1vw solid #ccc;
            border-radius: 0.5vw;
            text-align: center;
            font-size: 2vw;
            font-weight: bold;
            margin: 1.5vw 0;
            background: white;
        }

        #controls label {
            font-weight: bold;
            font-size: 2vw;
        }

        #KaiYashumi {
            margin-right: 3vw;
        }

        #questionCount {
            font-weight: bold;
            font-size: 3vw;
            margin-right: 3vw;
        }
    </style>
</head>

<body>

    <div id="controls">
        <input type="number" id="winCount" value="3" min="1" />
        <label>回正解で抜け！ 誤答で</label>
        <input type="number" id="lockCount" value="1" min="1" />
        <label id="KaiYashumi">回休み</label>

        <span id="questionCount"></span>

        <button id="nextBtn" class="controlBtn GreenBtn">スルー</button>
        <button id="backBtn" class="controlBtn GreenBtn">1つ戻る</button>
        <button id="AllResetBtn" class="controlBtn PinkBtn">全枠リセット</button>
        <button id="resetQuestionCount" class="controlBtn PinkBtn">問題数リセット</button>
    </div>

    <div id="board"></div>

    <script>
        const board = document.getElementById("board");
        const N = 12;
        let winNeeded = parseInt(document.getElementById("winCount").value);
        let lockTurns = parseInt(document.getElementById("lockCount").value);

        let players = Array.from({ length: N }, (_, i) => ({
            id: i + 1,
            score: 0,
            lock: 0,
            active: true,
            winner: false
        }));

        let history = [];
        let questionCount = 1;

        function saveState() {
            history.push({
                players: JSON.parse(JSON.stringify(players)),
                questionCount: questionCount
            });
            if (history.length > 50) history.shift();
            localStorage.setItem("players", JSON.stringify(players));
            localStorage.setItem("questionCount", questionCount);
        }

        function updateQuestionCount() {
            document.getElementById("questionCount").textContent = "Q" + questionCount;
        }

        function incrementQuestionCount() {
            questionCount++;
            updateQuestionCount();
        }

        function render() {
            board.innerHTML = "";
            winNeeded = parseInt(document.getElementById("winCount").value);
            lockTurns = parseInt(document.getElementById("lockCount").value);

            players.forEach(p => {
                const div = document.createElement("div");
                div.className = "player";
                if (!p.active && !p.winner) div.classList.add("inactive");
                if (p.winner) div.classList.add("winner");

                const header = document.createElement("div");
                header.textContent = `No.${p.id}`;

                const score = document.createElement("div");
                score.className = "score";
                score.textContent = p.winner ? "" : p.score;
                if (p.winner) score.innerHTML = `<span class="winner-text">抜け！</span>`;

                const status = document.createElement("div");
                status.className = "status";
                if (p.lock > 0 && !p.winner) status.innerHTML = `<span class="lock">Lock${p.lock}</span>`;
                else status.innerHTML = "";

                const btns = document.createElement("div");
                btns.className = "btns";

                if (!p.winner) {

                    /* ○ボタン */
                    const plusBtn = document.createElement("button");
                    plusBtn.textContent = "○";
                    plusBtn.className = "actionBtn action-plus";
                    plusBtn.disabled = !p.active;
                    plusBtn.onclick = () => {
                        saveState();
                        p.score++;
                        incrementQuestionCount();
                        if (p.score >= winNeeded) { p.winner = true; p.active = false; }
                        players.forEach(other => {
                            if (other.id !== p.id && other.lock > 0) {
                                other.lock--;
                                if (other.lock === 0 && !other.winner) other.active = true;
                            }
                        });
                        render();
                    };

                    /* ×ボタン */
                    const missBtn = document.createElement("button");
                    missBtn.textContent = "×";
                    missBtn.className = "actionBtn action-miss";
                    missBtn.disabled = !p.active;
                    missBtn.onclick = () => {
                        saveState();
                        p.lock = lockTurns;
                        p.active = false;
                        incrementQuestionCount();
                        players.forEach(other => {
                            if (other.id !== p.id && other.lock > 0) {
                                other.lock--;
                                if (other.lock === 0 && !other.winner) other.active = true;
                            }
                        });
                        render();
                    };

                    /* リセ（得点リセット）ボタン */
                    const scoreReset = document.createElement("button");
                    scoreReset.textContent = "0";
                    scoreReset.className = "actionBtn action-reset";
                    scoreReset.onclick = () => {
                        saveState();      // 変更前の状態を履歴に残す
                        p.score = 0;      // 得点を 0 にする

                        // ★ ロックも解除する（ここが重要）
                        p.lock = 0;       // 数値ロックを 0 に
                        p.active = true;  // アクティブに戻す

                        render();         // UI 更新
                    };


                    btns.append(plusBtn, missBtn, scoreReset);

                } else {
                    const resetBtn = document.createElement("button");
                    resetBtn.textContent = "リセット";
                    resetBtn.className = "reset";
                    resetBtn.onclick = () => {
                        saveState();
                        Object.assign(p, { score: 0, lock: 0, active: true, winner: false });
                        render();
                    };
                    btns.append(resetBtn);
                }

                div.append(header, score, status, btns);
                board.appendChild(div);
            });

            updateQuestionCount();
        }

        document.getElementById("nextBtn").onclick = () => {
            saveState();
            players.forEach(p => {
                if (p.lock > 0) {
                    p.lock--;
                    if (p.lock === 0 && !p.winner) p.active = true;
                }
            });
            incrementQuestionCount();
            render();
        };

        document.getElementById("AllResetBtn").onclick = () => {
            if (confirm("全枠をリセットしますか？")) {
                saveState();
                players = players.map(p => ({ id: p.id, score: 0, lock: 0, active: true, winner: false }));
                localStorage.removeItem("players");
                render();
            }
        };

        document.getElementById("backBtn").onclick = () => {
            if (history.length > 0) {
                const last = history.pop();
                players = last.players;
                questionCount = last.questionCount;
                render();
            }
        };

        document.getElementById("resetQuestionCount").onclick = () => {
            if (confirm("問題数をリセットしますか？")) {
                questionCount = 1;
                updateQuestionCount();
            }
        };

        window.addEventListener("load", () => {
            const savedPlayers = localStorage.getItem("players");
            const savedQ = localStorage.getItem("questionCount");
            if (savedPlayers) players = JSON.parse(savedPlayers);
            if (savedQ) questionCount = parseInt(savedQ);
            render();
        });

        render();
    </script>

</body>

</html>

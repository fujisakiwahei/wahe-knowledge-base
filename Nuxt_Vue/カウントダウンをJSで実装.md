# カウントダウンをJSで実装

タイプ: スニペット

```jsx
/* ===============================================
 *  ウェイトリストのカウントダウン
 =============================================== */
document.addEventListener("DOMContentLoaded", () => {
  // 要素の存在確認
  const day = document.getElementById("day");
  const hour = document.getElementById("hour");
  const min = document.getElementById("min");
  const sec = document.getElementById("sec");

  // 要素が存在しない場合は処理を終了
  if (!day || !hour || !min || !sec) {
    console.log("カウントダウン要素が見つかりません");
    return;
  }

  function countdown() {
    const now = new Date(); // 現在時刻を取得
    const releaseDate = new Date(2025, 7, 1, 0, 0, 0); // リリース日を取得（2025年8月1日）
    const diff = releaseDate.getTime() - now.getTime(); // 時間の差を取得（ミリ秒）

    // ミリ秒から単位を修正
    const calcDay = Math.floor(diff / 1000 / 60 / 60 / 24);
    const calcHour = Math.floor(diff / 1000 / 60 / 60) % 24;
    const calcMin = Math.floor(diff / 1000 / 60) % 60;
    const calcSec = Math.floor(diff / 1000) % 60;

    // 取得した時間を表示（2桁表示）
    day.innerHTML = calcDay < 10 ? "0" + calcDay : calcDay;
    hour.innerHTML = calcHour < 10 ? "0" + calcHour : calcHour;
    min.innerHTML = calcMin < 10 ? "0" + calcMin : calcMin;
    sec.innerHTML = calcSec < 10 ? "0" + calcSec : calcSec;
  }

  // 初回実行
  countdown();
  // 1秒ごとに実行
  setInterval(countdown, 1000);
});

```

```html
    <div class="waitListCountdown">
      リリースまで<span id="day">00</span>:<span id="hour">00</span>:<span id="min">00</span>:<span id="sec">00</span>
    </div>
```
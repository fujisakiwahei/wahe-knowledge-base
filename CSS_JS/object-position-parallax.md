# object-position パララックス マニュアル

## 全体の仕組み

```
┌─────────────────────────┐ ← wrapper（overflow: hidden）
│  ┌───────────────────┐  │
│  │                   │  │ ← 見えない部分（上）
│  │- - - - - - - - - -│  │
│  │   表示される部分   │  │ ← wrapperの表示領域
│  │- - - - - - - - - -│  │
│  │                   │  │ ← 見えない部分（下）
│  └───────────────────┘  │
└─────────────────────────┘

スクロールに応じて object-position を変化させ、
画像の「どの部分を見せるか」を動的に変更する
```

---

## 1. 画像素材の準備

**パララックスには余白が必要。元画像を表示領域より大きく用意する。**

| 表示領域   | 推奨画像サイズ | 余白              |
| ---------- | -------------- | ----------------- |
| 1440×645px | 1440×800px程度 | 上下に約20%の余白 |
| 536×536px  | 536×650px程度  | 上下に約20%の余白 |

※ 縦方向のパララックスなら、**高さ**を大きく用意する

---

## 2. CSSの設定（クラス名は適宜変更）

```scss
.wrapper {
  overflow: hidden; // はみ出しを隠す（必須）
}

.wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover; // 領域を覆う
  object-position: center 0%; // 初期位置（上端揃え）
  transition: object-position 0.3s ease-out; // イージング（任意）
}
```

### object-position の値

| 値            | 意味                                  |
| ------------- | ------------------------------------- |
| `center 0%`   | 画像の**上端**をwrapperの上端に揃える |
| `center 50%`  | 画像の**中央**をwrapperの中央に揃える |
| `center 100%` | 画像の**下端**をwrapperの下端に揃える |

### パララックスの方向

| 動き            | 初期値        | 終了値        |
| --------------- | ------------- | ------------- |
| 下→上（一般的） | `center 0%`   | `center 100%` |
| 上→下           | `center 100%` | `center 0%`   |

---

## 3. JavaScriptのテンプレート

```javascript
const wrapper = document.querySelector(".wrapper");
const img = document.querySelector(".wrapper img");

if (wrapper && img) {
  let isInView = false;

  function updateParallax() {
    if (!isInView) return;

    const rect = wrapper.getBoundingClientRect();
    const windowHeight = window.innerHeight;

    // 進行度を計算（0〜1）
    const totalScrollRange = windowHeight + rect.height;
    const currentScroll = windowHeight - rect.top;
    let progress = currentScroll / totalScrollRange;
    progress = Math.max(0, Math.min(1, progress));

    // object-position を変化させる
    const startPos = 0; // 開始位置（%）
    const endPos = 100; // 終了位置（%）
    const posY = startPos + (endPos - startPos) * progress;

    img.style.objectPosition = `center ${posY}%`;
  }

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        isInView = entry.isIntersecting;
        if (isInView) updateParallax();
      });
    },
    { threshold: 0 },
  );

  observer.observe(wrapper);
  window.addEventListener("scroll", updateParallax);
}
```

### 調整ポイント

| 変数       | 説明                        |
| ---------- | --------------------------- |
| `startPos` | スクロール開始時の位置（%） |
| `endPos`   | スクロール終了時の位置（%） |

- 控えめな動き: `20` → `80`
- 大きな動き: `0` → `100`

---

## 4. 注意点

1. **CSSの初期値とJSの`startPos`を合わせる**
   - ズレると初回表示時にガクッと動く

2. **transitionはお好みで**
   - 付けると滑らかに遅れて追従
   - 付けないとスクロールに直接連動

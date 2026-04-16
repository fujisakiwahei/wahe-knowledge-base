# Stylelint（保存時自動修正）メモ — 再利用用

Cursor / VS Code で CSS・SCSS・Vue のスタイルを Stylelint し、保存時に自動修正するための手順まとめにゃ。

---

## 前提

- Node.js（プロジェクト推奨版に合わせる）にゃん
- 拡張: Stylelint（拡張 ID は stylelint.vscode-stylelint）にゃ
- 公式: vscode-stylelint の README（Installation / editor.codeActionsOnSave）にゃー

---

## 1. パッケージ導入

ターミナルで実行するコマンド例にゃ:

    npm install -D stylelint stylelint-order postcss-scss postcss-html

- Vue SFC の style ブロックを対象にするなら postcss-html が欲しいにゃ
- 素の CSS / SCSS だけなら postcss-html と Vue 用 overrides は省略可にゃん

---

## 2. package.json に type module があるときの落とし穴

"type": "module" があると stylelint.config.js は ESM 扱いになるにゃ。
中身が module.exports（CommonJS）だと「module is not defined」系で落ちるにゃ。

対処のおすすめ:

- ファイル名を stylelint.config.cjs にして module.exports のまま使うにゃ（手堅い）にゃん

代替:

- stylelint.config.js を export default { ... } の ESM で書くにゃー

---

## 3. stylelint.config.cjs（ルートに配置）

次の内容をプロジェクトルートの stylelint.config.cjs にするにゃ（Vue 使わないなら overrides の vue ブロックだけ削る）にゃん:

    module.exports = {
      plugins: ["stylelint-order"],
      overrides: [
        {
          files: ["**/*.vue"],
          customSyntax: "postcss-html",
        },
        {
          files: ["**/*.{css,scss}"],
          customSyntax: "postcss-scss",
        },
      ],
      rules: {
        "order/properties-order": [
          {
            groupName: "Content",
            properties: [
              "content",
              "list-style",
              "list-style-type",
              "list-style-position",
              "list-style-image",
              "counter-reset",
              "counter-increment",
              "quotes",
            ],
          },
          {
            groupName: "Positioning",
            properties: ["position", "z-index", "top", "right", "bottom", "left", "inset"],
          },
          {
            groupName: "Box Model",
            properties: [
              "margin",
              "margin-top",
              "margin-right",
              "margin-bottom",
              "margin-left",
              "margin-inline",
              "margin-inline-start",
              "margin-inline-end",
              "margin-block",
              "margin-block-start",
              "margin-block-end",
              "padding",
              "padding-top",
              "padding-right",
              "padding-bottom",
              "padding-left",
              "padding-inline",
              "padding-inline-start",
              "padding-inline-end",
              "padding-block",
              "padding-block-start",
              "padding-block-end",
              "width",
              "min-width",
              "max-width",
              "height",
              "min-height",
              "max-height",
            ],
          },
          {
            groupName: "Display",
            properties: [
              "display",
              "flex",
              "flex-direction",
              "flex-wrap",
              "flex-flow",
              "justify-content",
              "justify-items",
              "justify-self",
              "align-items",
              "align-content",
              "align-self",
              "gap",
              "row-gap",
              "column-gap",
              "grid",
              "grid-template",
              "grid-template-columns",
              "grid-template-rows",
              "grid-column",
              "grid-row",
              "grid-area",
              "visibility",
              "float",
              "clear",
              "overflow",
              "overflow-x",
              "overflow-y",
            ],
          },
          {
            groupName: "Typography",
            properties: [
              "font",
              "font-family",
              "font-size",
              "font-weight",
              "font-style",
              "font-variant",
              "line-height",
              "letter-spacing",
              "word-spacing",
              "text-align",
              "text-decoration",
              "text-transform",
              "text-indent",
              "text-overflow",
              "white-space",
              "word-break",
              "word-wrap",
            ],
          },
          {
            groupName: "Visual",
            properties: [
              "color",
              "background",
              "background-color",
              "background-image",
              "background-position",
              "background-size",
              "background-repeat",
              "border",
              "border-top",
              "border-right",
              "border-bottom",
              "border-left",
              "border-width",
              "border-style",
              "border-color",
              "border-radius",
              "box-shadow",
              "outline",
            ],
          },
          {
            groupName: "Decoration",
            properties: [
              "opacity",
              "transform",
              "transform-origin",
              "transition",
              "transition-property",
              "transition-duration",
              "animation",
              "animation-name",
              "animation-duration",
              "cursor",
              "pointer-events",
              "user-select",
              "will-change",
              "appearance",
            ],
          },
        ],
      },
    };

---

## 4. .vscode/settings.json（保存時に Stylelint fix）

ワークスペース用に置く例にゃ（公式の source.fixAll.stylelint: explicit）にゃん:

    {
      "css.validate": false,
      "scss.validate": false,
      "stylelint.validate": ["css", "scss", "vue", "postcss"],
      "stylelint.lintFiles.glob": "**/*.{css,scss,vue}",
      "editor.codeActionsOnSave": {
        "source.fixAll.stylelint": "explicit"
      }
    }

css.validate / scss.validate をオフにすると標準 CSS 検証との二重表示を減らしやすいにゃ（任意）にゃー

---

## 5. .vscode/extensions.json（任意）

    {
      "recommendations": ["stylelint.vscode-stylelint"]
    }

---

## 6. package.json の scripts（任意）

    "lint:style": "stylelint \"**/*.{css,scss,vue}\""

CLI でまとめて直す例にゃ:

    npx stylelint "**/*.{css,scss,vue}" --fix

---

## 7. .stylelintignore（例）

    node_modules/

ビルド成果物はプロジェクトに合わせて足すにゃん

---

## 8. 動作確認の流れ

1. 拡張を入れてウィンドウ再読み込みにゃ
2. .vue や .scss を開き、プロパティ順を崩して保存にゃ
3. order/properties-order が fix 対象なら並び替わるにゃん
4. ダメなら出力パネル Stylelint のログを見るにゃー

---

## 参考 URL（そのままコピー用）

- https://stylelint.io/user-guide/get-started
- https://github.com/stylelint/vscode-stylelint/blob/main/README.md

---

## 自己評価・振り返りにゃ

- 単一のコードブロック内に収め、ネストしたフェンスは使わず、設定本文は 4 スペースインデントで区別したにゃ
- 公式 README の保存時修正キーと、type module 時の .cjs 対策を両方入れたにゃん
- バージョンは npm の時点で変わるのでパッケージ行は固定バージョンにしていないにゃー

# 読書クエスト 📚

読書をRPG風にゲーム化したWebアプリ。本を登録し、ミッションカードを引いて15分の読書クエストに挑戦。読書メモ(アウトプット)を書くと経験値がもらえてレベルアップしていく。

## 起動方法

```bash
node server.js
# → http://localhost:3006 を開く
```

※ このMacのNode.jsは `/Users/a818011/.local/node/bin/node` にある(PATH未設定)。

## 遊びの流れ

1. **本を登録** — 本棚に読みたい本を追加
2. **今日はこれ!** — 今日読む本を選ぶ
3. **ミッションカードを引く** — ノーマル/レア/エピックの3段階(レア度で獲得EXPボーナスが変化)
4. **15分タイマー** — 一時停止・早期終了・中断が可能
5. **読書メモ** — ミッションに沿った学びを記録(書くと +20 EXP)
6. **リザルト** — EXP獲得内訳とレベルアップ演出

## EXPのしくみ

- クエストクリア +20 / 読書1分ごと +2 / ミッションボーナス +10〜50 / メモ記入 +20
- 次のレベルに必要なEXP = `100 + (レベル − 1) × 50`
- レベルに応じて称号が変化(見習い読書家 → … → 伝説の読書王)

## 技術構成(ビルド不要版)

Node.js のビルド環境なしで動くよう、CDN読み込みの構成にしている:

- **React 18**(UMD版・CDN)
- **Babel Standalone v7**(JSXをブラウザ内で変換。**v8はimport文を出力して動かないため必ずv7に固定**)
- **Tailwind CSS v4**(ブラウザ版CDN。`<style type="text/tailwindcss">` でテーマ拡張)
- **localStorage** にデータ保存(キー: `dokusho-quest-data-v1` / `dokusho-quest-theme`)
- インターネット接続が必要(CDN・Google Fonts)

## ファイル構成

```
dokusho-quest/
├── index.html        # エントリ。CDN読み込み・Tailwindテーマ・アニメーション定義
├── server.js         # 静的ファイルサーバー(依存パッケージなし、port 3006)
└── js/
    ├── data.js       # ミッションカード・レベル・称号・XP計算(プレーンJS)
    ├── storage.js    # localStorage 保存・読み込み(プレーンJS)
    ├── components.js # UIコンポーネント(JSX)
    └── app.js        # アプリ本体・画面遷移(JSX)
```

画面遷移: `home → mission(カード抽選) → reading(タイマー) → memo → result → home`

## デザイン

- 温かみのあるアンバー/オレンジ基調のRPG風
- 見出しはドット絵風フォント(DotGothic16)、本文は丸ゴシック(Zen Maru Gothic)
- ヘッダーの 🌙/☀️ ボタンでダークモード切替(OS設定にも追従)
- スマホ・PC両対応(レスポンシブ)

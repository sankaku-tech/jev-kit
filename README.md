# Jev 導入マニュアル（Claude Code・Codex 両対応）

文章を書かずに「どれか選ぶ・点数・はい／いいえ」だけを返すAI、Jev（ジェブ）を、自分の Claude Code か Codex から使えるようにするまでの手順です。

ページ版: https://sankaku-tech.github.io/jev-kit/

> これは個人がまとめた非公式の手順書です。Jev は TypeSafe AI のサービスで、ここで配っているのは手順と頼み文だけです。
> 公式: https://typesafe.ai ／ 公式スキル: https://github.com/typesafe-ai/skills

## 前提

- Claude Code か Codex のどちらかが、すでに入っていること
- Vercel のアカウント（無料で作れます）

## 1. Vercel で AI Gateway のキーを発行する

公式サイトからの利用は順番待ちですが、Vercel の AI Gateway 経由なら待たずに使えます（モデルID `typesafe-ai/jev`）。

1. Vercel のダッシュボードで **AI Gateway → API Keys** を開く
2. **Create key** を押してキーを作る
3. 表示されたキーをコピーする（この画面を閉じると二度と見られません）

## 2. キーを環境変数に入れる

Mac / Linux のターミナル:

```bash
export AI_GATEWAY_API_KEY="ここにキーを貼る"
```

Windows の PowerShell:

```powershell
$env:AI_GATEWAY_API_KEY="ここにキーを貼る"
```

**実行しても何も表示されません。それが正常です。** 入ったかどうかは、次の1行で確かめます（キーの中身は表示されません）。

Mac / Linux:

```bash
echo ${AI_GATEWAY_API_KEY:+キーは設定されています}
```

Windows の PowerShell:

```powershell
if ($env:AI_GATEWAY_API_KEY) { "キーは設定されています" }
```

「キーは設定されています」と出れば成功です。何も出なければ、`=` のあとにキーが入っていません。

- **キーをAIのチャットに貼らないでください。** ターミナルで設定してから、その同じターミナルで Claude Code / Codex を起動します
- 効くのは**その1行を実行したターミナルの窓だけ**です。窓を閉じると消えます。別の窓や、アプリ版の Claude Code には届きません
- 毎回使う・アプリ版で使うなら、Mac は `~/.zshrc` に同じ1行を足して、ターミナルとアプリを開き直してください

## 3-A. Claude Code に公式スキルを入れる

```bash
claude plugin marketplace add typesafe-ai/skills
claude plugin install typesafe@typesafe-ai
```

どのフォルダの Claude Code からも使えるようになります。

## 3-B. Codex に公式スキルを入れる

使いたいプロジェクトのフォルダで実行します。

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai -a codex -y
```

そのフォルダの `.agents/skills/typesafe-ai/` に入ります。全部のフォルダで使うなら `-g` を足してください。

## 4. 最初の1件を通す

[PROMPT.md](PROMPT.md) の枠の中を、Claude Code か Codex に貼って送ってください。問い合わせ3件の「担当・緊急度・クレームかどうか」が表で返ってきたら成功です。

## つまずいたとき

| 症状 | 原因 | 対処 |
|---|---|---|
| 認証エラー（401） | キーが読めていない | キーを設定したのと同じターミナルで起動し直す |
| 429 が返る | 無料枠の回数制限 | 少し待ってやり直す。続くなら AI Gateway のクレジットを足す |
| モデルが見つからない | AI SDK が古い | `ai` パッケージを 7.0.105 以降に上げる |
| スキルが出てこない | 入れる前から開いていた | Claude Code / Codex を新しく開き直す |

## 知っておいてほしいこと

- **料金は使った分だけ** Vercel にかかります。心配なら AI Gateway の Budgets で上限を決めておけます
- Jev は**文章を書けません**。仕分け・点数づけ・はい／いいえの判定に使い、考える仕事や書く仕事は今までのAIに任せます
- 「自信がないものだけ人に回す」は自動では起きません。返ってくる自信の数字（confidence）を見て、低いものを人に回す、と自分で決めて組みます
- よく引用される「8秒が0.1秒」「最大400分の1」は開発元が出している数字です。速度のデモは、入力が短く開発元に有利な条件だと公式自身が書いています
- Jev はオープンソースではなく、API でだけ使えます

## License

MIT（この手順書と頼み文について）

---
theme: default
title: データ基盤運用にSkillsを求めるのは間違っているだろうか
info: 第二回 試されDATA
drawings:
  persist: false
transition: slide-left
fonts:
  sans: 'Noto Sans JP'
  weights: '400,700'
---

# データ基盤運用にSkillsを求めるのは<br>間違っているだろうか

試されDATA SAPPORO #2

---
---

<div class="intro-slide">
  <div class="intro-info">
    <div class="intro-name">makotyo</div>
    <div class="intro-company">SimpleForm株式会社</div>
    <div class="intro-role">Data Engineer</div>
  </div>
  <div class="intro-certs">
    <img src="/badge-data-engineer.png" alt="SnowPro Advanced Data Engineer" class="cert-badge-img" />
    <img src="/badge-architect.png" alt="SnowPro Advanced Architect" class="cert-badge-img" />
  </div>
</div>

<style>
.intro-slide {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  gap: 1.8rem;
}
.intro-info {
  text-align: center;
}
.intro-name {
  font-size: 2.2rem;
  font-weight: 700;
  color: #000;
  line-height: 1.2;
}
.intro-role {
  font-size: 1.1rem;
  color: #333;
  margin-top: 0.4rem;
}
.intro-company {
  font-size: 0.95rem;
  color: #888;
  margin-top: 0.3rem;
}
.intro-certs {
  display: flex;
  gap: 0.8rem;
  flex-wrap: wrap;
  justify-content: center;
}
.cert-badge-img {
  height: 80px;
  width: auto;
  object-fit: contain;
}
</style>

---
layout: statement
---

<style>
.statement-small h1 { font-size: 2.5rem !important; }
.slidev-layout p { opacity: 1 !important; }
</style>

<div class="statement-small">

# コーディングエージェントを<br>運用タスクにも使いたい

Claude Code / Codex / Gemini CLI / Skills / MCP / Hooks ...

コードを書くだけでなく、定常的な運用作業にも活用できるのでは？

</div>

---

# Agent Skillsおさらい

<div class="mt-6 space-y-4 text-base">

- エージェントに手順やナレッジを渡すためのオープンな標準規格
- `SKILL.md` ファイルに Markdown で手順・ルール・ナレッジを記述する
- チーム固有の専門知識を再利用可能な手順として残せる
- 定常業務を再現可能で一貫性のあるワークフローにできる

</div>

---

# Skillコンテンツの2つの種類

<div class="mt-6 grid grid-cols-2 gap-6">

<div class="content-box">

**Reference content**

Claudeの知識を拡張する

- 規約、パターン、スタイルガイド
- ドメイン知識、API仕様
- 会話の中でインラインで参照される

</div>

<div class="content-box highlight">

**Task content** ← 今日の主役

Claudeにステップを実行させる

- デプロイ、コミット、コード生成
- `/skill-name` で直接呼び出せる
- Claudeが自動で判断して実行することも可能

</div>

</div>

<div class="mt-6 text-center text-sm" style="color: #666;">

従来の Custom Slash Command は Skills に統合された（既存の `.claude/commands/` も動作する）

</div>

<style>
.content-box {
  padding: 1rem;
  border: 1.5px solid #ddd;
  border-radius: 8px;
  background: #fafafa;
}
.content-box.highlight {
  border-color: #000;
  background: #f5f5f5;
}
</style>

---

# データエンジニアの定常業務

<div class="grid grid-cols-3 gap-3 mt-6">
<div class="task-item">テーブル追加</div>
<div class="task-item">カラム変更</div>
<div class="task-item">ユーザー追加</div>
<div class="task-item">データ出力</div>
<div class="task-item">権限の棚卸し</div>
<div class="task-item">バックフィル</div>
</div>

<div class="mt-8 text-center text-lg">

不定期だけど繰り返し発生し、毎回そこそこ手間がかかる

</div>

<style>
.task-item {
  text-align: center;
  padding: 0.6rem;
  border: 1.5px solid #ddd;
  border-radius: 6px;
  font-size: 0.95rem;
  color: #333;
}
</style>

---

# テーブル追加タスク、Skill化できそう？

<div class="mt-6 text-center">
<div class="big-numbers">
  <div class="big-num"><span class="num">10</span><span class="num-label">Step</span></div>
  <div class="big-num"><span class="num">800+</span><span class="num-label">行の手順書</span></div>
</div>
</div>

<div class="mt-8 space-y-2 text-base">

- dbt / Snowflake / GitHub / Terraform … 関わるツールが多い
- Step間に依存関係があり、順番を守る必要がある
- 月数回の頻度で発生するタスク

</div>

<style>
.big-numbers {
  display: flex;
  justify-content: center;
  gap: 3rem;
}
.big-num {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.num {
  font-size: 3rem;
  font-weight: 700;
  color: #000;
  line-height: 1;
}
.num-label {
  font-size: 0.85rem;
  color: #888;
  margin-top: 0.3rem;
}
</style>


---
layout: two-cols-header
---

# Skillに何を書いたか

手順そのものに加え、つまづきポイントやStep間の依存関係も記述した

::left::

<div class="pr-4 space-y-4">

**手順書にあるもの**

- 各Stepの具体的な操作手順

**手順書にないもの**

- Step間の依存関係
- 本番操作時の安全確認
- よくあるエラーの対処法

</div>

::right::

<div class="pl-4 space-y-4">

**Skillでの表現**

- 手順をそのままStepとして記述
- 依存関係を実行順序として定義
- トラブルシューティングもSkillに含める
- 本番操作は **permission設定** でガードレール
```json
{
  "permissions": {
    "ask": ["Bash(*本番ワークフロー用コマンド*)"]
  }
}
```


</div>

---
---

# 「○○テーブルを追加してください」

<div class="flex justify-center mt-4">
  <video controls class="rounded-lg shadow-lg" style="max-height: 400px;">
    <source src="/tamesare_demo.mp4" type="video/mp4">
  </video>
</div>

---

# 実行するとこうなる

<div class="mt-6 space-y-6">

<v-clicks>

<div class="flex items-start gap-4">
<div class="bg-black text-white rounded-full w-8 h-8 flex items-center justify-center shrink-0 font-bold">1</div>
<div>Skillを呼ぶと、まず全体の実行計画が提示される</div>
</div>

<div class="flex items-start gap-4">
<div class="bg-black text-white rounded-full w-8 h-8 flex items-center justify-center shrink-0 font-bold">2</div>
<div>Step 1から順に自動実行が進む</div>
</div>

<div class="flex items-start gap-4">
<div class="bg-black text-white rounded-full w-8 h-8 flex items-center justify-center shrink-0 font-bold">3</div>
<div>本番操作の前で <strong>permission設定により確認を求めてくる</strong></div>
</div>

<div class="flex items-start gap-4">
<div class="bg-black text-white rounded-full w-8 h-8 flex items-center justify-center shrink-0 font-bold">4</div>
<div>承認すると続行し、完了後にPRが作成される</div>
</div>

</v-clicks>

</div>

---
---

# 嬉しさ☺️

<div class="mt-8 space-y-6 text-lg">

- Skillを呼ぶだけで、テーブル追加が最初から最後まで完了する

- 実行中は別のタスクを進められる

</div>

---

# 定常業務のSkill化、おすすめです

<div class="mt-8 space-y-6 text-lg">

- 自社固有のルールやエラー対処をそのまま反映できる

- 言語化されてこなかったノウハウを書き残しておく機会になる

- 作業内容は理解した上で実行しましょう

</div>

---
layout: center
class: text-center
---

# Thank you!

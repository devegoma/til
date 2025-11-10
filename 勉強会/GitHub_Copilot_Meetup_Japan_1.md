# Copilotの精度を上げる！カスタムプロンプト入門

```embed
title: "Copilotの精度を上げる！カスタムプロンプト入門.pdf"
image: "https://files.speakerdeck.com/presentations/811833ad055f454a8fad807601d0aad8/slide_0.jpg?37268987"
description: "2025/11/07 GitHub Copilot Meetup Japan #1 の登壇資料です。  最初は copilot-instructions.md の解説だけを想定していましたが、予想以上に奥が深かったのでタイトルをカスタムプロンプト入門に変更させていただきました（スライドタイトルは当時のままです）"
url: "https://speakerdeck.com/ismk/copilotnojing-du-woshang-keru-kasutamupuronputoru-men"
favicon: ""
aspectRatio: "56.25"
```

GitHub Copilotのカスタマイズの種類
![[Pasted image 20251108163210.png]]
※Yumeさん([@engix_dev](https://x.com/engix_dev))のスライドから抜粋

今まで個人設定をリポジトリに直接入れていたが、これを機に調べなおしてみた。

## 個人設定の仕方
Copilot Chatの左下アイコンから"Personal instructions"を選び、そこに書き込む。
![[Pasted image 20251108161326.png]]

## エージェント設定
下記のツリー状にファイルを入れる
```markdown-tree
.github/
	copilot-instructions.md: リポジトリ設定(グローバル設定)
	instructions/
		*.instructions.md: リポジトリ設定(パス固有設定)
	prompts/
		{command}.prompt.md: プロンプト設定
	**/
		AGENTS.md: エージェント設定 近いものから優先
	CLAUDE.md: ClaudeCode用設定 rootのみ
	GEMINI.md: Gemini設定 rootのみ
```
### リポジトリ設定(グローバル)
エージェントがリポジトリ上のコンテキストで動作するとき、自動的にプロンプトに追加される。

含めるべき内容
- 何のリポジトリなのか、主要言語・ランタイム、大枠の構成
- 初期化、ビルド、テスト、起動、lintなどの手順や前提条件
- プロジェクトレイアウト、重要ファイル、設定ファイル、依存ファイル等の簡単なインデックス
- モデルが学習していない最新技術のドキュメントURL等
## リポジトリ設定(パス固有)
特定のglobパターンに一致するファイルを指定して指示の適応ができる。
記述例
```markdown
---
applyTo: "**/*.ts,**/*.tsx"
---

- 適応させたい指示
```
### プロンプト設定
再利用頻度が高いプロンプト(決まった作業手順、まとまっているレビュー観点)を複数定義可能
chatインターフェースからスラッシュコマンドとして実行可能
```markdown
---
mode: "agent" | "ask" | "edit"
description: ""
model: "×××"
tools: ["search", "todos",...]
---
```
### 一般AIエージェント向け設定
リポジトリ設定(グローバル)と同様に扱われる
複数ファイル存在した時の挙動についてはドキュメント上で明確化されていない
ただしコードレビューには聞かないので注意
### 組織設定
これは組織向けアカウントでしかいじれないので詳しいことは割愛
セキュリティーポリシーや、コンプライアンス周りを記述するらしい
## 設定上注意する点
設定ごとに矛盾した指示を出さないこと
ファイルごとに正しいスコープで記述してあげると、保守性や拡張性が高くなる。

## おまけ
個人設定で参考にしている記事
```embed
title: "【コピペOK】技術的負債を作らないためのルールを設定しよう（Claude Code, Codex, Cursor対応）"
image: "https://res.cloudinary.com/zenn/image/upload/s--czjxOlpb--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E3%2580%2590%25E3%2582%25B3%25E3%2583%2594%25E3%2583%259AOK%25E3%2580%2591%25E6%258A%2580%25E8%25A1%2593%25E7%259A%2584%25E8%25B2%25A0%25E5%2582%25B5%25E3%2582%2592%25E4%25BD%259C%25E3%2582%2589%25E3%2581%25AA%25E3%2581%2584%25E3%2581%259F%25E3%2582%2581%25E3%2581%25AE%25E3%2583%25AB%25E3%2583%25BC%25E3%2583%25AB%25E3%2582%2592%25E8%25A8%25AD%25E5%25AE%259A%25E3%2581%2597%25E3%2582%2588%25E3%2581%2586%25EF%25BC%2588Claude%2520Code%252C%2520Codex%252C%2520Cursor%25E5%25AF%25BE%25E5%25BF%259C%25EF%25BC%2589%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%25A8%25E3%2581%25BE%25E3%2581%25A0%2540AI%2520%25E9%25A7%2586%25E5%258B%2595%25E9%2596%258B%25E7%2599%25BA%25E6%2595%2599%25E8%2582%25B2%25E8%2580%2585%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzYyNTkxYzBkNDYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
description: ""
url: "https://zenn.dev/tmasuyama1114/articles/global_rule_file"
favicon: ""
aspectRatio: "52.5"
```

個人設定に関しては、セキュリティ、エラーハンドリング、コード品質を担保する方法等、コーディングするうえで大事なこと・厳守しなければいけないことを自分で学び、解釈したうえで書く必要がある気がする。
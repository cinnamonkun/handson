## フェーズ4：ブランチの活用と共同作業の初歩

---

**スライド35：タイトル**

*   **タイトル：GitとGitHubをもっと深く！ ～ブランチ戦略と共同作業の入り口～**
*   サブタイトル：一人でもチームでも役立つ、より高度なバージョン管理へ
*   （イメージ：複数の道が合流・分岐する図、チームで協力するイラスト、設計図）

---

**スライド36：なぜブランチが重要なのか？（再確認）**

*   **`main` (または `master`) ブランチは聖域！**
    *   常に安定していて、いつでも動く状態を保ちたい。
    *   直接 `main` ブランチで大きな変更作業をするのはリスクが高い。
        *   バグが混入すると、安定版が壊れてしまう。
        *   作業途中の状態で `push` してしまうと、他の人に影響が出る (チーム開発の場合)。
*   **ブランチを使うメリット：**
    1.  **安全な実験場：** 新しい機能、バグ修正、試行錯誤などを `main` から隔離して行える。
    2.  **並行作業：** 複数の機能を同時に開発したり、複数のバグ修正を並行して進めたりできる (特にチームで)。
    3.  **整理整頓：** 変更内容ごとにブランチを分けることで、何のための変更かが明確になる。
*   （イメージ：`main` ブランチに保護バリア、そこから安全に分岐するブランチの図）

---

**スライド37：ブランチ命名規則のすすめ (一人でも！)**

*   ブランチに分かりやすい名前をつけると、後から見返したときに便利！
*   **よく使われる命名パターン：**
    *   **機能追加：** `feature/〇〇` (例: `feature/user-login`, `feature/add-new-button`)
    *   **バグ修正：** `fix/〇〇` (例: `fix/login-bug`, `fix/typo-in-readme`)
    *   **リファクタリング (コード改善)：** `refactor/〇〇` (例: `refactor/cleanup-code`)
    *   **その他：** `hotfix/〇〇` (緊急のバグ修正), `chore/〇〇` (雑務、ビルド関連など)
*   **ポイント：**
    *   英語の小文字、数字、ハイフン (`-`) を使うのが一般的。
    *   短く、具体的に内容が分かるように。
*   （イメージ：整理されたラベルが付いた箱、ブランチ名の良い例と悪い例）

---

**スライド38：一般的なブランチ活用ワークフロー (一人開発編)**

1.  **作業開始前：**
    *   `git checkout main` (まずは `main` ブランチにいることを確認)
    *   `git pull` (GitHub上の `main` ブランチの最新状態を取得)
2.  **新しいブランチを作成して移動：**
    *   `git checkout -b feature/新しい機能名`
    *   (または `git switch -c feature/新しい機能名`)
3.  **ブランチで作業：**
    *   コード編集 → `git add .` → `git commit -m "〇〇機能の△△を実装"` (こまめにコミット！)
4.  **(任意) GitHubにブランチをPush：**
    *   `git push -u origin feature/新しい機能名` (バックアップや進捗共有のため)
5.  **作業完了後、`main` ブランチに統合：**
    *   `git checkout main` (mainブランチに移動)
    *   `git pull` (念のため `main` を最新に)
    *   `git merge feature/新しい機能名` (`main` に `feature` ブランチの変更を取り込む)
    *   (コンフリクトが起きたら解決する ※これは後述)
6.  **`main` ブランチをGitHubにPush：**
    *   `git push origin main`
7.  **(任意) 不要になった作業ブランチを削除：**
    *   ローカル: `git branch -d feature/新しい機能名`
    *   リモート (GitHub): `git push origin --delete feature/新しい機能名`
*   （イメージ：このワークフローを図解したもの）

---

**スライド39：コンフリクトって何？ どうして起こるの？**

*   **コンフリクト (Conflict = 衝突)**
    *   Gitが自動でマージ (統合) できない場合に発生する。
*   **なぜ起こる？**
    *   複数のブランチで、**同じファイルの同じ箇所**を**異なる内容に編集**してしまった場合。
    *   例：
        *   `main` ブランチ：「こんにちは、世界！」
        *   `feature/greeting` ブランチ：「ハロー、ワールド！」
        *   この2つをマージしようとすると、Gitはどちらが正しいか判断できない。
*   コンフリクトは悪いことじゃない！開発ではよくあること。
*   （イメージ：2つの矢印がぶつかっている図、Gitが困っている顔）

---

**スライド40：コンフリクトの解決 (基本的な流れ)**

1.  **`git merge <ブランチ名>` を実行 → コンフリクト発生！**
    *   Gitが「コンフリクトしてるよ！」と教えてくれる。
    *   `git status` でも確認できる。
2.  **コンフリクトしたファイルを開く：**
    *   ファイルの中に、以下のような特殊なマーカーが書かれている。
        ```
        <<<<<<< HEAD (または mainなど現在のブランチ名)
        こちらのブランチでの変更内容
        =======
        マージしようとしているブランチでの変更内容
        >>>>>>> <マージ元のブランチ名>
        ```
3.  **ファイルを編集して、正しい状態にする：**
    *   マーカー (`<<<<<<<`, `=======`, `>>>>>>>`) を削除。
    *   両方の変更を残すか、どちらか一方を選ぶか、あるいは全く新しい内容にするか、自分で判断して修正。
4.  **修正後、ファイルを保存。**
5.  **`git add <修正したファイル名>`** (解決したことをGitに伝える)
6.  **`git commit`** (マージコミットを作成。メッセージは自動で入ることが多いが、自分で書いてもOK)
    *   **注意：** `git commit -m "メッセージ"` ではなく、単に `git commit` とするとエディタが開き、マージコミットのメッセージを編集できる。
*   （イメージ：コンフリクトマーカーの例、手作業で修正しているイメージ、解決後のスッキリしたファイル）
*   **VS Codeなどのエディタは、コンフリクト解決を支援する機能があることも！**

---

**スライド41：プルリクエスト (Pull Request / PR) とは？ (GitHub)**

*   **共同作業のための重要な機能！** (一人でも使える！)
*   **プルリクエストとは？**
    *   「私のこの変更 (ブランチ) を、あなたのブランチ (通常は `main`) に取り込んでほしいんだけど、レビューしてもらえませんか？」というお願い。
*   **主な目的：**
    *   **コードレビュー：** 他の人に変更内容を見てもらい、問題がないか確認してもらう。
    *   **議論の場：** 変更についてコメントし合える。
    *   **変更履歴の明確化：** なぜこの変更が入ったのか、どのような議論があったのかが記録される。
    *   **自動テスト連携：** (設定すれば) PR作成時に自動でテストを実行できる。
*   （イメージ：手紙や提案書を渡すアイコン、レビュー中の会話の吹き出し）

---

**スライド42：プルリクエストの基本的な流れ (GitHub上)**

1.  **作業ブランチをGitHubに `push` する。**
    *   例: `git push -u origin feature/my-new-feature`
2.  **GitHubのリポジトリページを開く。**
    *   多くの場合、「`feature/my-new-feature` has recent pushes. Compare & pull request」のようなボタンが表示される。
    *   または、「Pull requests」タブ → 「New pull request」ボタン。
3.  **プルリクエスト作成画面：**
    *   **Base (取り込み先):** 通常は `main`
    *   **Compare (取り込み元):** 作業したブランチ (例: `feature/my-new-feature`)
    *   タイトルと説明を分かりやすく書く。(何のための変更か、どんな点をレビューしてほしいか等)
    *   「Create pull request」をクリック。
4.  **(レビュー期間)** 他の人がレビューしたり、コメントしたりする。
    *   必要に応じて、指摘箇所を修正し、再度 `push` する (PRは自動で更新される)。
5.  **問題がなければ、PRがマージされる。**
    *   通常、リポジトリの管理者やレビュワーが「Merge pull request」ボタンを押す。
    *   マージ後、作業ブランチは削除しても良いことが多い (GitHub上でオプションが出る)。
*   （イメージ：GitHubのプルリクエスト作成画面とマージボタンのキャプチャ）
*   **一人開発でも、セルフレビューや変更履歴の整理のためにPRを使うのは良い習慣！**

---

**スライド43：フォーク (Fork) とは？ (GitHub)**

*   **フォークとは？**
    *   他の人の公開リポジトリを、**自分のGitHubアカウントに丸ごとコピー**すること。
    *   コピーなので、元のリポジトリに影響を与えずに自由に編集・実験できる。
*   **どんな時に使う？**
    *   **オープンソースプロジェクトに貢献したい時：**
        1.  プロジェクトをフォークする。
        2.  自分のフォークしたリポジトリで変更を加える (`clone` してローカルで作業 → `push`)。
        3.  フォーク元 (本家) のリポジトリに対してプルリクエストを送る。
    *   **他人のコードを元に、自分用にカスタマイズして使いたい時。**
*   （イメージ：食器のフォークのアイコン、リポジトリが分岐してコピーされる図）

---

**スライド44：共同作業のイメージ (非常にシンプル版)**

*   **チームメンバーAさん、Bさんがいるとする。**
1.  **Aさん：**
    *   `main` から `feature/A-task` ブランチ作成。
    *   作業して `push`。
    *   `main` へのプルリクエスト作成。
2.  **Bさん：**
    *   AさんのPRをレビュー。OKならマージ。
    *   (または) `main` から `feature/B-task` ブランチ作成。
    *   作業して `push`。
    *   `main` へのプルリクエスト作成。
3.  **Aさん (BさんのPRに対して)：**
    *   BさんのPRをレビュー。OKならマージ。
*   **常に `main` は最新の状態を保ち、`pull` してから作業開始。**
*   変更はブランチで行い、プルリクエスト経由で `main` にマージ。
*   （イメージ：複数の人がそれぞれブランチで作業し、PRを通じて中央のmainに集約される図）
*   **実際にはもっと色々なルールやツールがあるが、これが基本の考え方。**

---

**スライド45：フェーズ4のまとめ**

*   **ブランチは積極的に活用しよう！**
    *   `main` は安定版。作業は専用ブランチで。
    *   分かりやすいブランチ名を心がける。
*   **コンフリクトは解決できる！**
    *   落ち着いて対処すれば大丈夫。
*   **プルリクエストは強力なコミュニケーションツール！**
    *   コードレビューを通じて品質向上。
    *   一人でも変更履歴の整理に役立つ。
*   **フォークでオープンソースに参加も！**
*   **共同作業では、こまめな `pull`, `push`, コミュニケーションが重要。**
*   （イメージ：パズルのピースが組み合わさる、チームで目標達成するイラスト）

---

**スライド46：ここから先へ (Next Steps)**

*   **さらにGit/GitHubを使いこなすために：**
    *   `git rebase`: コミット履歴を綺麗にする (上級者向け、注意が必要)
    *   `git cherry-pick`: 特定のコミットだけを取り込む
    *   GitHub Actions: CI/CD (自動テストやデプロイ)
    *   GitHub Projects: タスク管理
    *   様々なブランチ戦略 (Git Flowなど)
*   **ドキュメントを読んだり、実際に色々試したりしてみよう！**
*   **コミュニティに参加して情報交換するのも良いでしょう。**
*   （イメージ：道がさらに先に続いている、学習リソースのロゴなど）

---

**スライド作成のポイント（フェーズ4）：**

*   **概念図を多用：** ブランチの分岐・合流、PRの流れ、フォークなどは言葉だけでは分かりにくい。
*   **段階的に：** 一人でブランチを使うメリットから始め、徐々に共同作業のイメージへ。
*   **「なぜ」を強調：** 各機能がなぜ必要なのか、どんな問題を解決するのかを明確に。
*   **実践を促す：** 「一人でもPRを使ってみよう」など、すぐに試せるアクションを提示。
*   **難易度への配慮：** 「上級者向け」「注意が必要」など、扱うべき情報のレベルを示す。
*   **「終わり」ではなく「始まり」を示す：** 更なる学習への意欲を刺激する。

フェーズ4は、GitとGitHubの強力な機能を垣間見ることで、学習者のモチベーションを高め、今後の自学自習に繋げることを意識しました。実際の講義では、受講者の反応を見ながら、特に難しい部分は補足説明を加えたり、デモンストレーションを交えたりするとより効果的です。

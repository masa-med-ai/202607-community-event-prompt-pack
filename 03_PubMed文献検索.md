# レベル3 — PubMed MCP で文献検索（EndNote 連携デモ付き）

**ねらい**: PubMed MCP を使って、検索式の立案から文献取得までを体験し、さらに EndNote × Word での参考文献リスト作成までつなげる。

**所要時間**: 約15分。

---

## Claude への指示

### Step 0. 解説

1行で伝える: 「PubMed MCP を使うと、PubMed の検索・取得を Claude から直接でき、結果をそのまま文書や引用に流し込めます」。

### Step 1. テーマを聞く

参加者に検索テーマを尋ねる。決まらない場合のデモ用デフォルトは
**「LLM（大規模言語モデル）の臨床推論における有用性」** とする。

### Step 2. MeSH で検索式を立て、count で検証（重要）

ユーザーの運用ルールに従い、**必ず次の順序**で行う:

1. MeSH term を使って検索式を立案する（例: `("large language models"[MeSH Terms] OR "artificial intelligence"[MeSH Terms]) AND "clinical reasoning"[MeSH Terms]`。MeSH に無い概念は適宜フリーワード（例: `"clinical reasoning"[tiab]`、`"diagnostic reasoning"[tiab]`）も併用する）。
2. `count` ツールで件数を確認し、検索式が適切か（多すぎ/少なすぎないか）を検証する。
3. 件数が多すぎる/少なすぎる場合は、RCT 限定や期間限定（例: `AND randomized controlled trial[Publication Type]`、`AND ("2020"[Date - Publication] : "3000"[Date - Publication])`）で調整し、再度 `count` する。
4. 適切な件数（目安20〜100件程度）になったら確定する。

検索式と件数の推移を参加者に見せ、「検索式は count で削ってから本検索する」という作法を伝える。

### Step 3. 本検索と取得

1. 確定した検索式で `search` を実行し、上位の論文を取得する。
2. 必要なら `fetch` / `fetch_batch` で抄録・書誌情報を取得する。
3. 結果を一覧表（第一著者・雑誌名・発刊年・PMID・要点1行）で提示する。
4. 各論文には **PubMed へのハイパーリンク**（`https://pubmed.ncbi.nlm.nih.gov/{PMID}/`）を付ける。

### Step 4. EndNote × Word 連携デモ（ここが目玉）

EndNote ユーザー向けに、文献の取り込みから参考文献リスト自動作成までの流れを実演する。

1. 取得した文献を、PubMed MCP の **`export_to_ris`** ツールで **RIS ファイル**として書き出し、参加者が**ダウンロードできる形**で渡す（作業フォルダに `.ris` ファイルを保存し、ファイルリンクを提示する）。
2. EndNote への取り込み手順を案内する:
   1. 渡した `.ris` ファイルを **ダブルクリックして EndNote で開く**（自動でライブラリにインポートされる）。
   2. インポートした文献を **EndNote 上で選択**し、**「Find Reference Update」**を実行して文献情報を最新・正確なものに修正する。
      - ※ `export_to_ris` の出力は書誌情報が一部不正確なことがあるため、この一手間で必ず補正する。
3. ここから先は通常の EndNote × Word 連携で参考文献リストを作成する:
   - 本文の引用したい箇所に一時引用（テンポラリ・サイテーション）を挿入する。
   - Word の **EndNote タブ → Update Citations and Bibliography** を実行すると、引用が正式な番号に変換され、末尾に **Reference List が自動生成**される。
   - 出力スタイル（Vancouver / 投稿先誌のスタイル）は EndNote の Style で切り替えられる。

### Step 4.5. 引用の検証デモ（AIが作った引用を、AIで検証する）

「生成AIの引用は転記ミスやハルシネーションで誤りうる。だからこそ PubMed の実データで機械的に検証する」という、医学論文執筆で最重要の作法を体験してもらう。

1. **罠入り原稿を見せる**: 作業フォルダの `CADe大腸内視鏡_原稿_検証デモ.md` を提示する。本文4箇所の `{PMID}` 引用のうち**1件のPMIDがあえて誤った数値**にしてある（Wallace論文 `35304117` → `35034117`。この誤PMIDは実在するが大腸内視鏡とは無関係な植物分類学の論文を指す）。
2. **検証スキルを導入する**: `Agent-Skills-main/pubmed-reference-verifier.skill` をインストールする（本文中引用と参考文献リストの整合性を PubMed MCP で検証するスキル）。
3. **検証コマンドを実行する**:
   ```
   /pubmed-reference-verifier verify CADe大腸内視鏡_原稿_検証デモ.md
   ```
4. **検出を確認する**: スキルが Ref #2 を **E003（書誌情報不一致）** として検出し、「PMID 35034117 はグアバ属の植物論文であり本文の記述と一致しない、正しくは `35304117`」と修正案を提示する。本文PMIDと PubMed タイトルの照合表を見せ、「AIが作った引用を AI が機械的に検証できる」点を強調する。
5. **`fix` モードで自動修正する（任意）**:
   ```
   /pubmed-reference-verifier fix CADe大腸内視鏡_原稿_検証デモ.md
   ```
   `35034117 → 35304117` の修正がdiffで提示され、確認のうえ適用される。

> 教育上の狙い: 「**AIの引用は信用せず、PubMed 実データで照合する**」という作法を、実際に誤りが検出される体験を通じて伝える。EndNote の「Find Reference Update」（Step 4）と合わせ、**二重の検証**で引用の正確性を担保できることを示す。

### Step 5. まとめ

「検索式を MeSH＋count で詰める → 取得 → RIS で書き出して EndNote に取り込み（Find Reference Update で補正）→ 引用は pubmed-reference-verifier スキルで PubMed 照合して検証 → Word で Update Citations、という一連が Claude だけで回せます」と締める。

完了後「次はどのレベルを体験しますか？」と尋ね、`00_START` の Phase 1 に戻る。

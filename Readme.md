# 学習ログ / Learning Log

## ■ 日付（JST） / Date

2025/10/02

## ■ 学習テーマ / Topic

Kaggle – R 基礎（入出力・データ型・基本関数）

## ■ 目的 / Goal (JP→EN one-liner)

* **JP:** Kaggle環境でRの基本入出力とデータ型の扱いを身につけ、次回から迷わずデータを読み込めるようにする。
* **EN:** Gain confidence with R I/O and data types in Kaggle so I can import and inspect datasets without friction.

---

## ■ 今日やったこと

* Rにおける四則演算と基本データ型（numeric / character / logical / factor の基礎）
* `print()`, `head()`, `tail()` の挙動確認（tibble/data.frame双方）
* CSVの読み込み

  * `readr::read_csv()`（tidyverse / tibble）
  * `base::read.csv()`（base R / data.frame）
* 読み込み後の基本確認：`dim()`, `names()`, `glimpse()`, `str()`
* Kaggle の相対パス（例：`../input/<dataset-name>/<file>.csv`）の扱い
* 取り違えやすい点（列名重複の自動改名、型推定メッセージの意味）

---

## ■ 参照した資料・リンク

* Kaggle Notebook: Getting Started in R (First Steps)
  [https://www.kaggle.com/code/rtatman/getting-started-in-r-first-steps/notebook](https://www.kaggle.com/code/rtatman/getting-started-in-r-first-steps/notebook)

---

## ■ 自分の理解で怪しい所（要修正ポイント → 正しい説明）

1. **`read.csv` と `read_csv` の違いが曖昧**

* **正:** `read.csv()` は **base R**（返り値は data.frame）。`readr::read_csv()` は **tidyverse**（返り値は tibble）。tibble は印字が簡潔で、型推定が速く、`glimpse()` との相性が良い。
* **Tip:** Kaggleでtidyverseを使う場合は **`readr::read_csv()` を優先**。base流儀が必要なときだけ `read.csv()`。

2. **パスや文字列のクォート忘れ**

* **正:** ファイルパスは**必ず文字列で指定**：`read_csv("../input/food-choices/food_coded.csv")`。

3. **読み込み後メッセージの解釈**（“New names…”, `spec()`）

* **正:** 同名列があると**自動で連番付きに改名**（例：`...10`, `...12`）。エラーではない。
  型推定メッセージは**通知**であり、`show_col_types = FALSE` で非表示にできる。
  必要なら `col_types=` で型を明示指定できる。

4. **因子（factor）と文字列（character）の混同**

* **正:** R 4.0以降、`read.csv()` でもデフォルトで文字列は **stringsAsFactors=FALSE**（＝character）が基本。分類用途・順序が必要な列のみ **`factor()`** へ変換する。

5. **`print()` の必要性**

* **正:** R コンソールは最後の値を**自動表示**する。Notebook でもオブジェクト名を単独行にすれば表示される。`print()` は**明示的に整形したい時**やループ内での出力に使う。

---



## ■ 英語の簡単要約（Short EN Summary）

I practiced basic R in Kaggle: arithmetic, core data types, and essential I/O.
I learned the difference between `base::read.csv()` (data.frame) and `readr::read_csv()` (tibble), how to interpret the import messages (auto-renaming duplicate columns and type inference), and how to quickly inspect data with `glimpse()`, `head()`, and `dim()`.


---

## ■ 所要時間 / Time spent

未記録（あとで追記）

---

## ■ 次にやりたいこと（Next steps）

* 列型の**明示指定**（`col_types=`）で実務的に堅い読み込みを体験
* **欠損処理**（`mutate()`, `replace_na()`, `drop_na()`）の練習
* **要約と可視化**：`group_by()` + `summarise()` → `ggplot2` で1枚の図に
* 使う列が固まったら、**前処理レシピ**（関数化 or RMarkdown/Notebook化）を作成して再利用可能にする


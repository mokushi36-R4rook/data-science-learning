
# 学習ログ / Learning Log

## ■ 日付（JST） / Date
2025/10/04

## ■ 学習テーマ / Topic
Kaggle – Getting started in R: Graphing Data

## ■ 目的 / Goal (JP→EN one-liner)
- JP: `tidyverse` を用いてデータを読み込み、型を整え、要約統計と基本グラフを作る流れを身につける  
- EN: Build a repeatable pipeline in R (read → clean types → summarize → basic plots) with tidyverse.

---

## ■ 今日やったこと（一次メモ・JP中心）
- `library(tidyverse)` の呼び出し（新ノート＝新セッションの理解）
- `read_csv("...")` によるデータ読み込み（Kaggle の `/kaggle/input` 探索も学習）
- `make.names()` で列名の空白を安全な名前へ
- `str()` / `glimpse()` でデータ型の確認
- `type_convert()` と `parse_number()` で `Cocoa.Percent` を数値化
- 先頭1行削除：`df[-1, ]`
- パイプ `%>%` と `summarise()` / `group_by()` / `mean()` / `sd()`
- `summarise_all()` の使い方と注意点（数値列のみ対象）

---

## ■ 重要ポイント（正しい使い方・定着用スニペット）
```r
library(tidyverse)

# 1) 読み込み
# 例: Kaggle で実在 CSV を自動検出して読む
csv <- list.files("/kaggle/input", pattern="\\.csv$", full.names=TRUE, recursive=TRUE)[1]
d <- readr::read_csv(csv, show_col_types = FALSE)

# 2) 列名正規化（空白→ドット）
names(d) <- make.names(names(d), unique = TRUE)

# 3) 型の点検
glimpse(d)   # or str(d)

# 4) Cocoa.Percent を数値に（"70%" → 70）
if ("Cocoa.Percent" %in% names(d)) {
  d <- d %>% mutate(Cocoa.Percent = readr::parse_number(Cocoa.Percent))
}

# 5) 先頭1行を除去（必要なら）
d <- d[-1, ]

# 6) 要約統計（平均・標準偏差）
d %>% summarise(
  avg_rating = mean(Rating, na.rm = TRUE),
  sd_rating  = sd(Rating,   na.rm = TRUE),
  avg_cocoa  = mean(Cocoa.Percent, na.rm = TRUE),
  sd_cocoa   = sd(Cocoa.Percent,   na.rm = TRUE)
)

# 7) グループ要約（例：産地別に平均評価）
d %>%
  group_by(Broad.Bean.Origin) %>%
  summarise(
    n = n(),
    avg_rating = mean(Rating, na.rm = TRUE),
    sd_rating  = sd(Rating,   na.rm = TRUE)
  ) %>%
  arrange(desc(avg_rating)) %>%
  head(10)

# 8) 基本グラフ（散布図と箱ひげ）
# Cocoa% と Rating の関係
ggplot(d, aes(x = Cocoa.Percent, y = Rating)) +
  geom_point(alpha = 0.4) +
  geom_smooth(method = "lm", se = TRUE) +
  labs(title = "Cocoa % vs Rating", x = "Cocoa %", y = "Rating")

# 原産地別の評価分布（上位カテゴリだけ表示したいときは filter(n >= 20) などで事前に絞る）
ggplot(d, aes(x = Broad.Bean.Origin, y = Rating)) +
  geom_boxplot(outlier.alpha = 0.3) +
  coord_flip() +
  labs(title = "Rating by Broad Bean Origin", x = "Origin", y = "Rating")
````

---

## ■ 自分の理解で怪しい所（要修正ポイント）

1. **`summarise_all()` の挙動**

   * 誤解：全列に `mean()` をかければOK。
   * 修正：**文字列や因子列に `mean()` は不可**。`select_if(is.numeric)` で数値列に限定するか、`~ if (is.numeric(.)) ... else NA_real_` のように分岐させる。
   * 例：

     ```r
     d %>% select_if(is.numeric) %>% summarise_all(~ mean(., na.rm = TRUE))
     ```
2. **`%>%` の評価スコープ**

   * 誤解：パイプの外で `sd(Rating)` だけ書けば列を参照できる。
   * 修正：**データフレームの列参照はパイプ内で `summarise()` の引数として評価**する。
   * 例：

     ```r
     d %>% summarise(avg = mean(Rating, na.rm=TRUE), sd = sd(Rating, na.rm=TRUE))
     ```
3. **行削除の記法**

   * 誤解：`d(-1,)` のように関数呼び出しで書ける。
   * 修正：**インデックス指定**で `d[-1, ]`（行, 列）と書く。
4. **パス指定と Kaggle のデータ添付**

   * 誤解：`../input/...` なら常に読める。
   * 修正：**そのノートに “Add data” で添付したデータのみ** `/kaggle/input` に現れる。まず `list.files("/kaggle/input", recursive = TRUE)` で実在確認。
5. **`type_convert()` の効果範囲**

   * 誤解：実行するだけで元オブジェクトが書き換わる。
   * 修正：**代入が必要**（`d <- type_convert(d)`）。`%` を含む列は別途 `parse_number()`。
6. **列名**

   * 誤解：教材の `Cocoa_Percent` をそのまま使える。
   * 修正：`make.names()` 後は **`Cocoa.Percent`** になっている可能性が高い。`names(d)`で確認。

---

## ■ 英語の簡単要約（3–4文）

I practiced a full tidyverse workflow in R: loading packages, reading a CSV, cleaning column names, fixing types with `type_convert()` and `parse_number()`, and summarizing with `summarise()` and `group_by()`.
I learned to remove the first row using negative indexing, to add `na.rm = TRUE` when computing means/SDs, and to apply summaries only to numeric columns.
I also created basic plots (scatter and boxplot) to explore relationships between cocoa percentage and rating and variation across bean origins.
Common pitfalls included path issues on Kaggle, `summarise_all()` on non-numeric columns, and column names after `make.names()`.


---

## ■ 所要時間 / Time spent

90分

```
```

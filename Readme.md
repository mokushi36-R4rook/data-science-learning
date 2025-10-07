# 学習ログ / Learning Log

## ■ 日付（JST） / Date
2025/10/07

## ■ 学習テーマ / Topic
R（tidyverse）基礎：ファイルI/O、前処理、集計、可視化、保存＆表示

## ■ 目的 / Goal (JP→EN one-liner)
- JP: Kaggle環境でCSVの所在確認→読み込み→整形→集計→可視化→保存→表示までを一気通貫で自走できるようにする  
- EN: Become able to independently locate, load, clean, aggregate, plot, save, and display a CSV in R (tidyverse) on Kaggle.

---

## ■ 今日やったこと（一次メモ・JP中心）
- `getwd()`, `dir.exists()`, `list.dirs()`, `list.files()` で**実在パスの発見**を練習  
- `readr::read_csv()` で**読み込み**（`list.files(..., "\\.csv$")` で得たフルパスを渡す）  
- 先頭行削除：`df <- df[-1, ]`（**負の添字**）  
- 列名整形：`gsub("\\s+", "_", names(df))`（空白/改行→`_`）  
- 型の正規化：`parse_number(Cocoa_Percent)`、`as.integer(Review_Date)`、`type_convert()`  
- 集計：`group_by(Review_Date) |> summarize(mean, sd, n)`（`na.rm = TRUE`）  
- 可視化：`ggplot(aes(...)) + geom_point() + geom_line()`  
- 画像保存：`ggsave("average_cocoa_by_year.png", plot = p, ...)`  
- 表示：`knitr::include_graphics("average_cocoa_by_year.png")`（または `IRdisplay::display_png()`）

---

## ■ 参照した資料・リンク
- Kaggle データセット（Chocolate Bar Ratings / flavors_of_cacao.csv）
- tidyverse パッケージ群（readr, dplyr, ggplot2, stringr）リファレンス
- knitr / IRdisplay による画像表示

---

## ■ 自分の理解で怪しい所（要修正ポイント）
- **`aes` のタイポ**（`ase` と書いてエラー）→ *ggplot の審美マッピングは `aes()`*  
- **正規表現**：`"[[:space:]+]"` は「空白1文字**または**+」の意味になりやすい  
  → *意図は「空白1回以上」＝ `\\s+` を使う*  
- **型の扱い**：`"70%"` を `gsub("%","",x)` だけだと文字列のまま  
  → *`readr::parse_number()` で**数値**化、`mean()`/`sd()` が動く*  
- **Kaggleのパス**：`/kaggle/input` に**データセットを “+ Add data” でアタッチ**しないと見つからない  
- **改行と真偽値**：`"\n"`（`"/n"`は文字列）、`TRUE/FALSE`（`True/False` はRでは不可）  
- **インデックス**：`df[-1, 0]` は“列ゼロ本”で空になる  
  → *行だけ指定して `df[-1, ]`*  
- **オブジェクトの長さ**：`list.files()` は**文字ベクトル**を返す  
  → *`csv_path[1]` のように1要素を `read_csv()` に渡す*

---

## ■ 英語の簡単要約（あれば下書き）
I practiced a full pipeline in R with tidyverse on Kaggle: locating a CSV, loading it, cleaning column names, parsing types, aggregating by year, and plotting the results. Key learnings were using `list.files()` to find real paths, `parse_number()` to convert `"70%"` to numeric, and `aes()` for ggplot mapping. I also saved and displayed the plot from `/kaggle/working`.

---

## ■ 成果物（コード/図/ノート）
### 最終ワークフロー（要点のみ）
```r
library(tidyverse)

# 実在パスの取得（例：flavors_of_cacao.csv を検索）
csv_path <- list.files("/kaggle/input", pattern = "flavors_of_cacao\\.csv$", 
                       recursive = TRUE, full.names = TRUE)[1]
stopifnot(length(csv_path) == 1)

# 読み込み
chocolateData <- readr::read_csv(csv_path)

# 前処理
chocolateData <- chocolateData[-1, ]
names(chocolateData) <- gsub("\\s+", "_", names(chocolateData))
chocolateData <- chocolateData %>%
  mutate(
    Cocoa_Percent = readr::parse_number(Cocoa_Percent),
    Review_Date   = as.integer(Review_Date)
  ) %>%
  readr::type_convert()

# 集計（年ごと平均＋SD）
averageCocoaPercentbyYear <- chocolateData %>%
  group_by(Review_Date) %>%
  summarize(
    averageCocoaPercent = mean(Cocoa_Percent, na.rm = TRUE),
    sdCocoaPercent      = sd(Cocoa_Percent,   na.rm = TRUE),
    n = dplyr::n(),
    .groups = "drop"
  )

# 可視化＋保存
p <- ggplot(averageCocoaPercentbyYear, aes(Review_Date, averageCocoaPercent)) +
  geom_point() + geom_line() +
  labs(title = "Average Cocoa Percent by Review Year",
       x = "Review Year", y = "Average Cocoa Percent (%)") +
  theme_minimal()
ggsave("average_cocoa_by_year.png", plot = p, width = 8, height = 5, dpi = 150)

# 表示（どちらか一方でOK）
# knitr::include_graphics("average_cocoa_by_year.png")
# IRdisplay::display_png(file = "average_cocoa_by_year.png")

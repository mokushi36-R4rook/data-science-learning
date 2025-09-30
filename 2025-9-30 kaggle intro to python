# 学習ログ / Learning Log

## ■ 日付（JST） / Date
2025/09/30

## ■ 学習テーマ / Topic
Kaggle – Intro to Python（Booleans, Conditionals, Functions, `help`, `round`）

## ■ 目的 / Goal (JP→EN one-liner)
- JP: Kaggle演習を通じて、真偽値と条件分岐・関数定義の基本を“手順化”して定着させる  
- EN: Turn booleans, conditionals, and functions into a repeatable routine via Kaggle exercises.

---

## ■ 今日やったこと（一次メモ・JP中心）
- `help()` の読み方（ヘッダーと短い説明）
- `round(x, ndigits)` の挙動（バンカーズ・ラウンディング）
- `if / elif / else` と境界条件の扱い（`> 0` vs `> 1`）
- `and / or / not` の優先順位（`not` > `and` > `or`）と括弧の重要性
- デフォルト引数（`def f(a, b=3)`）
- 符号判定関数 `sign` の実装
- 簡潔なブール返却（`return number < 0`）
- Kaggleセットアップセルの意味（`binder.bind(globals())`, `from learntools... import *`）

---

## ■ 参照した資料・リンク
- Kaggle Notebook: https://www.kaggle.com/code/moku36/exercise-booleans-and-conditionals/edit

---

## ■ 自分の理解で怪しい所（要修正ポイント）→ **正しい説明**
1. **`help()` のヘッダー解釈**  
   - × `round(number, ndigits=None)` の `=None` の意味が曖昧  
   - ✓ **省略可能な引数**で、指定しなければ `None` が渡る設計というサイン

2. **`round` の丸め規則**  
   - × 常に「.5 は切り上げ」  
   - ✓ Python は **最近接の偶数**に丸める（例：`round(2.5)==2`, `round(3.5)==4`）。必要なら `decimal` の `ROUND_HALF_UP` を使用

3. **境界条件の取り違え**  
   - × 正の判定に `n > 1` を使った  
   - ✓ **正**は `n > 0`、**負**は `n < 0`、**ゼロ**は `n == 0`

4. **ブール式の優先順位**  
   - × `not`/`and`/`or` が混じる式をそのまま書いた  
   - ✓ `not` > `and` > `or`。**迷ったら括弧で意図を固定**する  
     ```python
     ok = have_umbrella or (rain < 5 and have_hood) or not (rain > 0 and is_workday)
     ```

5. **デフォルト引数の使い方**  
   - × 「引数1個のとき」「2個のとき」で実装が揺れる  
   - ✓ 2つ目に **既定値**を置く：`def to_smash(total, friends=3): ...`

6. **三項演算子の構文**  
   - × `True if cond False` のような形  
   - ✓ 正しくは `A if cond else B`。ただし比較式自体がブールを返すなら三項演算子は不要

7. **Kaggle のセットアップ未実行**  
   - × `q1` 未定義のまま `q1.check()`  
   - ✓ **最初に**セットアップセルを実行（`Restart & Run All` でも可）

---

## ■ 英語の簡単要約（2–3文）
Practiced reading `help()` headers and writing concise conditionals.  
Clarified Python’s rounding rule (banker’s rounding) and fixed boundary mistakes in `if` chains.  
Learned to rely on default parameters and parentheses to control boolean precedence.

---

## ■ 成果物（コード/断片）

```python
# 1) sign: 三分岐（読みやすさ重視）
def sign(n):
    if n < 0:
        return -1
    elif n > 0:
        return 1
    else:
        return 0

# ワンライナー版
def sign_one_liner(n):
    return (n > 0) - (n < 0)

# 2) 簡潔なブール返却
def concise_is_negative(number):
    return number < 0

# 3) デフォルト引数
def to_smash(total, friends=3):
    return total % friends

# 4) 論理の意図固定（括弧で明示）
def prepared_for_weather(have_umbrella, rain_level, have_hood, is_workday):
    return have_umbrella \
        or (rain_level < 5 and have_hood) \
        or not (rain_level > 0 and is_workday)

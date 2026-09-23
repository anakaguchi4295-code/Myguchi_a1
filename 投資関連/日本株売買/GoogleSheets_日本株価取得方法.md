# 📊 Google Sheetsで日本株の現在値を取得する方法

**記録日**: 2026-08-12
**結論**: GOOGLEFINANCEはTYO（東京証券取引所）データにアクセス不可。Yahoo!ファイナンスをIMPORTXMLで取得する方式が有効。

---

## ⚠️ 問題：GOOGLEFINANCEがTYOで使えない

```
=GOOGLEFINANCE("TYO:7203","price")
```

上記のような数式（手入力・API経由どちらも）で、以下のエラーが発生：

```
GOOGLEFINANCE の評価で、Google スプレッドシートは「TYO」のデータに
アクセスする権限がありません
```

### 原因
- Google側が国際取引所（TYO、SGX、SHA、KLSEなど）のGOOGLEFINANCE対応を制限している既知の問題
- 個人の設定や数式の書き方では解決できない、Googleのデータライセンス方針によるもの
- 米国株（NASDAQ/NYSE）は対象外で、`=GOOGLEFINANCE("NVDA","price")`のような数式は問題なく動く

参考：[Google Docs Editors Community - GOOGLEFINANCE various exchanges "not authorised"](https://support.google.com/docs/thread/189014298/googlefinance-various-exchanges-not-authorised-sha-sgx-tyo-klse-bkk-tse?hl=en)

---

## ✅ 解決策：Yahoo!ファイナンスをIMPORTXMLで取得

証券コードが入っているセル（例：C2）を参照し、Yahoo!ファイナンスの株価ページから直接スクレイピングする。

```
=IFERROR(VALUE(SUBSTITUTE(IMPORTXML("https://finance.yahoo.co.jp/quote/"&REGEXEXTRACT(ASC(TO_TEXT(C2)), "\d{4}")&".T", "//span[contains(@class, 'price')] | //span[contains(@class, '_1_1g9_N_')]"), ",", "")), "取得失敗")
```

### 数式の仕組み
1. `REGEXEXTRACT(ASC(TO_TEXT(C2)), "\d{4}")` — C2セルの内容から4桁の証券コードを抽出（全角→半角変換も実施）
2. `IMPORTXML("https://finance.yahoo.co.jp/quote/"&コード&".T", XPath)` — Yahoo!ファイナンスの該当ページから株価をXPathで抽出
3. `SUBSTITUTE(..., ",", "")` — 価格のカンマ区切り（例：1,234）を除去
4. `VALUE(...)` — 文字列を数値に変換
5. `IFERROR(..., "取得失敗")` — 失敗時はエラーで壊れず「取得失敗」と表示

### 注意点
- C列（コード）には証券コードの数字が含まれていればよい（`8306`でも`TYO:8306`でも動く。REGEXEXTRACTが4桁を自動抽出するため）
- IMPORTXMLは複数セルで同時に大量アクセスすると、一時的に読み込みが遅い・失敗することがある
- Yahoo!ファイナンス側のページ構造（class名）が変わると動かなくなる可能性がある。その場合はXPathの`class`部分を見直す必要あり

---

## 📄 適用先シート

**[日本株ポートフォリオ_Yahoo連動_コード順_v2](https://docs.google.com/spreadsheets/d/1N_TPgFuZUWvfx-5FlsRisqpalHV3FqZQ0B79aEbcNUA/edit)**

全31銘柄（特定・新NISA・旧NISA、証券コード昇順）に適用済み。列構成：

| 列 | 内容 |
|---|---|
| A 口座区分 | 特定・新NISA・旧NISA |
| B 銘柄 | 銘柄名 |
| C コード | 証券コード（4桁） |
| D 保有数 | |
| E 取得単価 | |
| F 取得金額 | `=D×E` |
| G 現在値 | 上記のYahoo!ファイナンス連動数式 |
| H 現在金額 | `=D×G` |
| I 評価損益 | `=H-F` |
| J 利益率 | `=ROUND(I/F*100,1)&"%"` |

---

## 🔗 関連ノート
- [[投資関連/日本株売買/最終評価表_2026-07-23.md]]
- [[投資関連/日本株売買/Google_Sheets用_最終表_2026-07-23.md]]

---

*米国株（NASDAQ/NYSE）はGOOGLEFINANCEがそのまま使えるため、この対応は不要。[[投資関連/米国株売買/外国株ポートフォリオ_2026-07-20.md]]参照。*

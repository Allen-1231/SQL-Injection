# Querying the database type and version on Oracle

### 目標

該購物網站的產品類別篩選器有 SQLi 漏洞，使用 UNION 攻擊執行注入語句來取得資料庫的種類與版本。

> [TIP]
> Hint:
> 
> Oracle 資料庫中，每個 `SELECT` 語句都必須要搭配 `FROM` 與後面指定的表名。然而如果 `UNION SELECT` 攻擊沒有從任何表查詢，仍然需要包含 `FROM` 語句，後面跟著有效的表名。
>
> Oracle 有一個內建表格為 `dual`，即可用在沒有從任何表查詢的狀況。
> 
> 其餘資訊可從 [PortSwigger 網站中的 SQLi cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) 中，查找對各種資料庫有什麼不同的寫法。

---

### Solution
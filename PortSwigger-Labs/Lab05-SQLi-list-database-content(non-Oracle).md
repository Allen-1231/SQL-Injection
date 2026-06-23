# Listing the database contents on non-Oracle databases

![alt text](image-24.png)

### 目標

該購物網站的產品類別篩選器有 SQLi 漏洞，使用 UNION 攻擊來從其他表中獲取資料。

應用程式具有登入功能，資料庫中包含一個存放 usernames 與 passwords 的表，找出這張表的表名與它具有的欄位，然後透過該表的內容來獲得所有使用者的帳號與密碼，並以使用者為 `administrator` 的身份來登入該購物網站。

> [!TIP]
> Hint:
> 
> 可從 [PortSwigger 網站中的 SQLi cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) 中，查找對各種資料庫有什麼不同的寫法。

---

### Solution

隨意選擇一種類別標籤，並使用 `UNION` 語句獲取欄位數量：

```sql
Accessories' UNION SELECT NULL,NULL--
```

![alt text](image-25.png)
> 有兩欄位。

將類別清空改為 0，如此只會傳回想要嘗試獲取的資訊，隱藏該類別的文章：

```sql
0' UNION SELECT NULL,NULL--
```

![alt text](image-26.png)

> 由目前注入的語句來看，也能判定此資料庫絕對不是 MySQL。


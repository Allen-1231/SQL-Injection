# SQL Injection

當應用程式將未經處理 (清洗與過濾) 的使用者輸入資料，直接拼接進 SQL 查詢指令中，導致惡意資料被資料庫視為指令的一部分執行時，就會發生 SQL 注入。

### SQLi 長相

假設有一個部落格，每篇文章都有唯一的編號，文章可被設為公開或私人狀態，網址如：
`https://website.thm/blog?id=1`

上面網址可看出，所選的部落格文章是透過查詢字串中的 id 參數來取得，網路應用程式要從資料庫中取得該文章，會使用以下 SQL 查詢語句：
`SELECT * FROM blog WHERE id=1 AND private=0 LIMIT 1;`

private 值為 0 代表公開可讀。以上的語句即代表在 blog table 中尋找 id 為 1 且 private 為 0 的文章，並且限制結果只回傳 1 個匹配到的。以此例而言，id 參數是直接被用在 SQL 查詢語句中的。

假設編號 2 的文章處於私人狀態，那若將 URL 改成 `https://website.thm/blog?id=2;--`，那麼相當於 SQL 查詢語句為 `SELECT * FROM blog WHERE id=2;-- and private=0 LIMIT 1;`，又由於分號 `;` 表示語句結束且 `--` 或 `#` 後視為註釋，因此查詢的其實是 `SELECT * FROM blog WHERE id=2;`，如此即使文章編號 2 設為私人，這樣操作後變成能看到 id=2 的文章。

---

### In-Band SQLi

最容易被檢測和利用的 SQLi 類型。In-Band 指的是用同一個通訊管道 (同一個 HTTP 請求與回應) 去利用漏洞，發起攻擊並獲得結果，例如：在一個網頁上發現 SQL 注入漏洞，然後能夠從資料庫提取資料到同一網頁上。

> 也就是攻擊者在網頁的輸入框或 URL 注入惡意指令 (發送請求)，資料庫找到的機密資料會直接顯示在同一個網頁的回傳畫面上 (接收回應)。

##### Error-Based SQL Injection

##### Union-Based SQL Injection

---

### Blind SQLi

##### Boolean Based

##### Time Based

---

### Out-of-Band SQLi

---

### Remediation 補救、預防措施

開發者有些方法可以保護它們的網路應用程式：
- Prepared Statements (With Parameterized Queries) 預處理語句、參數化查詢

    把 SQL 指令的架構先寫好，再把使用者輸入的資料作為參數填進語句中。這種方式可以確保 SQL 程式碼結構保持不變，資料庫也能夠區分查詢和數據。

    ```sql
    # 字串拼接 - 不安全
    SELECT * FROM users WHERE username = ' ' OR 1=1 --';

    # 參數化查詢 - 安全
    SELECT * FROM users WHERE username = ?;
    # 資料庫先編譯這行，將問號鎖定為純資料欄位，隨後填入參數
    ```
    即使攻擊者輸入 `' OR 1=1 --`，資料庫也只會尋找有沒有人的名字真的叫 `' OR 1=1 --`

- Input Validation 輸入驗證

    使用允許清單 (allow list) 可以將輸入限制為特定的字串，或是使用程式語言中字串替換方法來過濾允許或禁止的字串。輸入驗證檢查資料的格式與型態，過濾掉不合規則的請求。

    白名單驗證 (Whitelist Validation / Allow-list)：只允許符合特定規則的資料通過，其餘一律拒絕。例如使用者名稱只能包含字母或數字、年齡限制只能輸入 1 到 3 位數等。

- Escaping User Input 使用者輸入轉義

    一些特殊字元如 `'`、`"`、`$` 等能導致 SQL 語句改變，就能變成注入攻擊，使用者輸入轉義就是在這些字元前加上 `\` 來轉義，使它們解析為普通的字元。

    > 轉義並非萬能，它存在被繞過的風險。
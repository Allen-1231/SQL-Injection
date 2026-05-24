# SQL Injection

當應用程式將未經處理 (清洗與過濾) 的使用者輸入資料，直接拼接進 SQL 查詢指令中，導致惡意資料被資料庫視為指令的一部分執行時，就會發生 SQL 注入。

### SQLi 長相

假設有一個部落格，每篇文章都有唯一的編號，文章可被設為公開或私密狀態，網址如：
`https://website.thm/blog?id=1`

上面網址可看出，所選的部落格文章是透過查詢字串中的 id 參數來取得，網路應用程式要從資料庫中取得該文章，會使用以下 SQL 查詢語句：
`SELECT * FROM blog WHERE id=1 AND private=0 LIMIT 1;`
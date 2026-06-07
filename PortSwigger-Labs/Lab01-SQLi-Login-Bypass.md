# Login Bypass

### 目標

以 `administrator` 使用者的身份登入該購物網站。

![alt text](image.png)

---

### Solution

點擊 My account 到登入頁面，在 Username 欄位輸入 `administrator'--`，密碼隨意輸入，即可登入。
![alt text](image-1.png)
相當於 SQL 查詢：
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = '<任意填>';
```

> [!NOTE]
> 為什麼不用分號，也就是 `administrator';--`？
>
> 分號是來分隔多條語句的，它是一句語句的結束，若在此使用 `administrator';--`，分號後面 (`--' AND password = '<任意填>'`) 會被視為第二條語句，註解句。
>
> 而後端程式在驗證登入時，正常的語法只有一條 `SELCET * FROM users WHERE username = '<輸入>' AND password = '<輸入>';`，中間分兩句的話，相當於強迫資料庫在原本預期一條指令的管道中執行了多條語句、堆疊語句 (stacked queries)，許多後端網頁驅動程式預設在內部看到分號，為了安全起見就會報錯。
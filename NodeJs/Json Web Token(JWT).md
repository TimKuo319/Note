

FLow :

User登入 -> 將user資料與secret key做簽章，並設定到期日 -> 回傳token給使用者，讓他儲存在瀏覽器作為token ， 後續在進行API存取時就藉由驗證token確定使用者身分。

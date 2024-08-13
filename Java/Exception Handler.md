
## @ControllerAdvice and @RestControllerAdvice

+ 預設對所有controller有效
+ 通常用來進行例外處理

```java
@ControllerAdvice  
public class LoginHandler {  
    @ExceptionHandler(BindException.class)  
    public String handleBindException(Throwable e, Model model) {  
        BindingResult results = ((BindException) e).getBindingResult();  
  
        String msg = results.getAllErrors().get(0).getDefaultMessage();  
  
        model.addAttribute("msg", msg);  
        return "index";  
    }  
}
```
 
+ `ExceptionHandler` - 用來接收特定Exception
+ `Throwable` - 所有Error及Exception的Super set，在這裡可以替換成BindException
+ `getAllErrors` - 取得所有錯誤，包含`Object Error及Field Error`
+ `BindingResult` - 在處理表單相關驗證的時候常會用到的class
	+ 用來捕獲驗證錯誤
	+ 儲存錯誤訊息
	+ 檢查綁定狀態
	+ 常用方法
		+ hasErrors()
		+ getAllErrors
		+ ....
## Object Error vs Field Error(todo)

+ `Object Error` - 通常用於多字段的驗證，或更複雜的邏輯驗證
+ `Field Error` - 通常用於單一字段的驗證

## Summary

+ `針對可預期行為，使用class來封裝即可`
	+ 登入時，成功跟失敗是都是常見狀況，應透過一個`AuthResult`來回傳狀況
+ `針對不可預期行為拋出例外，交給Exceptoin Handler處理`
	+ 預設應用程式執行時資料庫連線本身就應該正常，連線失敗 -> 拋出例外

## Reference

[Day 17 - Spring Boot 例外處理 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10275702)




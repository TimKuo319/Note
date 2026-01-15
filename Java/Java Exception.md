
主要分為兩種類型的 exception : 

- Checked Exception
	- 會在編譯期被檢查，需要透過 catch 處理，或是在 function 直接指定會拋出給上層做處理
	- 所有 Checked Exception 的父類別是 `Exception`
	- 常見 exception
		- `IOException`
		- `SQLException`
		- `ParseException`
- Unchecked Exception
	- 不會在編譯期被檢查，所以不需要像 Checked Exception 一樣明確的指定
	- 所有 Unchecked Exception 的父類別是 `Runtime Exception`
	- `NullPointerException, ArrayIndexOutOfBoundsExceptoin`


- 使用時機
>If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception.”

## Reference

- [Checked and Uncecked Exceptions in Java | Baeldung](https://www.baeldung.com/java-checked-unchecked-exceptions)
- [Java筆記 — Exception 與 Error. 這是個很老梗的問題了, 但每個階段回來看, 都會有不同的體會… | by Carl | Medium](https://medium.com/@clu1022/java%E7%AD%86%E8%A8%98-exception-%E8%88%87-error-dbdf9a9b0909)
---
date : 2023-05-30  13:58 Tue
alias : []
---

---

## Verification vs Validation

+ `Verification` 
	+ 指同一個set的task用來確定軟體在執行特定function上是否正常
	+ Are we building the product right?
+ `Validation`
	+ 指許多不同的task set用來確保軟體是符合顧客需求的
	+ Are we building the right prodcut?

## Testing Strategy

+ testing-in-the-small to testing-in-the-large
+ For conventional software
	+ module(component) is initial focus
	+ Intergration of module follows
+ For OO software
	+ 相對傳統來說，將測試的focus從module轉為包含屬性及operation的OO class

## Unit Testing

+ Driver
	+ 用來模擬呼叫單元
	+ 原先的呼叫單元(caller)啟動時會花費很長時間時，就可以將他寫成較簡單的形式，來進行test，而這個較簡單的可控系統就會被稱作driver，stub的道理亦同。
+ Stub
	+ 用來模擬被呼叫的單元

## Intergration Testing Strategies

+ Big bang
	+ 大雜燴，將所有component結合在一起
+ Incremental construction
	+ Top-down
		+ depth first or breadth first
		+ stubs一次換一個
		+ 有些module會re-run
	+ Bottom-up
		+ worker modules會被集成一個cluster
		+ 每次換一個driver
	+ Sandwich
		+ 
## Regression Testing

+ 再做一次相同的task set
	+ 用來確保軟體上的改動沒有造成其他side effect
	+ 隨著intergration test的進行，regression test的次數也會相對應的成長

## High Order Testing
+ Validation testing
+ System testing
+ Alpha/Beta testing
+ Recovery testing
+ Security testing
+ Stress testing
+ Performance testing

## White Box Testing

+ goal
	+ 針對程式設計結構面進行測試
		+ 靜態測試
		+ 資料流程面(Data flow coverage)
		+ 控制流程面(Control flow coverage)
	+ all statement and conditions have been executed at least once

+ Exhaustive Testing
	+ 所有可能都跑過一次
+ Selective Testing
	+ 選擇特定路徑做測試
	+ 明確定義測試目標
+ Basis Path Testing
+  計算cyclomatic complexity
	+ number of simple decision + 1
	+ E-N+2
		+ E is edge
		+ N is node

## Black Box Testing

+ goal
	+ 針對程式設計安全面進行測試
		+ 模擬以使用者或駭客身分系統
		+ 
	+ Focus on the functional requirements of a module
	+ 找出軟體上的錯誤
		+ Incorrect or missing function
		+ Interface error
		+ Data structure external database accessing error
		+ behavior or performance error
		+ Initialization or termination error





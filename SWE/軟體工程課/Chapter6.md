---
date : 2023-04-25
alias : []
---

---

## Analysis Modeling

+ 描述使用者的需要
+ 作為軟體設計的基礎
+ 定義一連串的需求，使得他們能夠作為軟體完成之後的驗證。

其中包含四種元素
+ Scenario-based models
	+ [[SWE/軟體工程課/Chapter5|Chapter5]] 中提到的 `Interception` and `Elicitation` 
	+ Requirements gathering meetings
	+ QFD
	+ Requirement engineering mechanisms
+ Class models
+ Behavioral models
+ Flow models

## Domain Analysis

+ 定義要調查領域
+ 蒐集在這些領域中較有代表性的樣本
+ 分析樣本中的app
+ 為物件開發分析模型

## Analysis Classes

+ External entities
+ Things
+ Occurrences or events
+ Roles
+ Organizational units
+ Places

## Selecting Classes - Criteria

+ retained information
+ needed services
+ multiple attributes
+ common attributes
+ common operations
+ essential requirements

## CRC Modeling 

CRC指的是Class responsibility and Collaboration, Responsibility代表的是class本身所具備的屬性以及operations , 而Collaboration指的則是class與其他class之間的互助關係。

+ CRC card
	+ Class name
	+ Resonponsibility
	+ Collaborator
+ Class Type
	+ Entity class
	+ Boundary class
	+ Contoller class
+ Responsibility
+ Collaborations
	+ 代表class之間的關係，主要分為三類
		+ is-part-of   (aggregation)
			+ ex : 車子與輪子的關係，儘管車子壞了，輪子的存在依舊有意義
		+ has-knowledage-of (association)
			+ ex: client與server之間的關係，互相合作達成目標
		+ depends-upon (類似composite)
			+ ex: folder與file之間的關係，每個file都是獨立的個體，但若是folder消失，則所有file消失
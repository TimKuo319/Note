---
date : 2023-03-07 15:27 Thu
alias : []
---

---
## Manifesto
+ Individuals and interactions `over processes and tools`
+ Working software over `comprehensive documentation`
+ `Customer collaboration over contract negotiation`
+ Response to change over `following a plan`

## Agility

+ Effecitive response to change
+ Effective communication among all stakeholders
+ Drawing the customer onto the team
+ Organizing a team so that it is `in conrtorl` of the work performed
+ `Rapid, incremental devlivery of software`
+ customer and developer `work together as a team`


## Agile Process Characteristics
+ Adaptable
	Agile Process能夠快速應對不可預測的變化。
+ Responsive
	Heavily rely on feedback
+ Incremental
	Focus on delivery of software increments

## Agile Process Models
+ `Extreme Programming (XP)`
	+ XP Planing
		由User stories(註1)開始，團隊在著手於各個不同的Story，再將各個story結合成第一版，以利之後其他新版本的更動。
	+ XP Design
		+ KIS principle
		+ CRC cards(?)
		+ spike solutions (註2)
		+ Encourage `refactor`
	+ XP Coding
		+ Recommends the `construction of a unit test` for a story `before` coding commences
		+ Pair programming(註3)
	+ XP Testing
		+ Unit test with framwork enabling automation
		+ Regression test
		+ Acceptance test
+ Adaptive Software Development (ASD)
+ Dynamic System Development Method(DSDM)
	+ Principle
		+ Active user involvement is imperative
		+ DSDM teams must be empowered to make decisions.
		+ The focus is on frequent delivery of products.
		+ 適合商業目的是驗收的基本標準
		+ Iterative and incremental development is necessary to converge on anaccurate business solution.
		+ All changes during development are reversible
		+ Requirements are baselined at a high level
		+ Testing is integrated throughout the life-cycle
	+ Life Cycle
		+ Feasibility study (建立基本需求)
		+ Business study (建立功能及基本架構)
		+ Fucntional model iteration (建立多種原型版本，並收集使用者反饋再做改進)
		+ Design and build iteration ()
		+ Implementation
	![DSDM_lifecycle](DSDM_lifecycle.png)
+ `Scrum`
	+ Development work is partitioned into `packets`
	+ `Testing and documentation is ongoing` as product is constructed
	+ Work occurs in `sprints(註4)` and is derived from a `backlog(註5)` of existing requirements
	+ Shrot Meetings
	+ Demos with `time-box`
	+ Scrum meeting 
		+ 15minutes daliy meeting
		+ Teams member response to basics:
			+ What did you do since last meeting?
			+ Do you have any obstacles?
			+ What will you do before next meeting?
+ Crystal
+ Feature Driven Development(FDD)
+ Agile Modeling(AM)
	+ Model with purpose
	+ Use multiple models
	+ Travel light
	+ Content is more important than representation
	+ Know the models and the tools you use to create them
	+ Adapt locally


*註1:*
*註2: 藉由快速簡單的實作來測試解決方法的可行性，有助於快速找到解決問題的方向*
*註3: Two programmers work together for coding story*
*註4: A short, time-boxed period when a scrum team works to complete a set amout of work*
*註5: A list of all thins that need to be done within the project*

*KIS principle : 系統的設計應該保持簡約單純，不應參雜不必要的功能增加複雜性*
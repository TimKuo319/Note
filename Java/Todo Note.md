

- [ ] Java supplier 用法 -> wits-calendar service

- [ ] webconfigurer vs security

	- [ ] webConfigurer 
		- [ ] allowedHeaders
		- [ ] allowCredentials

- [ ] jdk 的 docker image 選項

	- [ ] maven - open-jdk ( already deprecated )
	
	- [ ] eclipse 

- [ ] maven wrapper 的用途，以及在 dockerfile 中是否會使用它

- [ ] transactional annotation 

- [ ] synchornize

- [ ] what is object mapper

- [ ] JPA 理解
	- 基礎 CRUD 只針對 `primary key`
	- `findAll()`
	- `findById()`
	- `deleteById()`
	- ....

- [ ] 網址解析
	- UriComponents

- [ ] 從 server 端發送 api

	- http client vs restemplate

- [ ] 踩雷
	- [ ] restTemplate 在傳遞 `Map<String, Object>` 的時候會將 `Content-Type` 設為 `application/json`，如果是用 `Map<String, String>` 傳遞則會使用預設的 `application/x-www-form-urlencoded`


- [ ] java 的 design pattern `singleton`

- [ ] AutomicInteger 

	- [ ] 目前理解是不用自己手動 `synchronized` 來確保 `thread-safe`
#Frontend 

```yaml
name: Docker Build and Push

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'

    - name: Build with Maven
      run: mvn clean package

    - name: Build and push Docker image
      env:
        DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
        DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      run: |
        docker build -t your-dockerhub-username/your-image-name:${{ github.sha }} .
        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
        docker push your-dockerhub-username/your-image-name:${{ github.sha }}
```

上面是一個 github action 的 yaml 檔案，以下是參數的解析

- `name` - 定義 workflow 的名稱，以這裡來說就是==Docker Build and Push==

- `on` - 什麼狀況下會觸發這個 workflow  
	- 以上面的例子來說，會有兩種情況觸發這個 workflow
		1. push 到 `main` branch 的時候
		2. 有 pull_request 到 `main` branch 的時候

- `jobs` - 定義 job 要做什麼

	- `build` - 這個位置可以隨便取，代表的是 ==job 的名稱==

	- `runs-on` - 指定這個 job 要跑在什麼樣的環境上

	- `steps` - 定義 job 的各個 step，每一個 step 以 `-` 開頭
	
		- `name` - step 的名稱
		
		- `uses` - 使用預先定義好的 action (由別人寫好的)
		
		- `run` - 要執行的指令

		- `env` - 設定環境變數

		- `with` - 提供 action 需要的參數，像上面使用別人寫好的 action 時，就必須指定需要的 java 發行版以及版本
		
		

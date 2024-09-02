#Frontend 

```yaml
name: CI Pipeline

on:
  push:
    branches: ["main"]

jobs:
  build-and-push-backend:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          distribution: "temurin"
          java-version: "17"

      - name: Ensure resources directory exists
        run: mkdir -p src/main/resources

      - name: Set up application.properties
        run: echo "cors.allow.origins=${{ secrets.CORS_ALLOW_ORIGINS }}" >> src/main/resources/application.properties

      - name: Build backend Docker image
        run: |
          docker build -t james66689/wits_calendar_backend:latest -f ./Dockerfile .

      - name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Push backend Docker image
        run: |
          docker push james66689/wits_calendar_backend:latest

  build-and-push-frontend:
    needs: build-and-push-backend
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "22"

      - name: Build frontend Docker image
        run: |
          docker build -t james66689/wits_calendar_frontend:latest -f ./WitsCalendar_Fontend/Dockerfile ./WitsCalendar_Fontend

      - name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Push frontend Docker image
        run: |
          docker push james66689/wits_calendar_frontend:latest
```

上面是一個 github action 的 yaml 檔案，以下是參數的解析

- `name` - 定義 workflow 的名稱，以這裡來說就是==CI Pipeline==

- `on` - 什麼狀況下會觸發這個 workflow  
	- 以上面的例子來說 push 到 `main` branch 的時候就會觸發 workflow

- `jobs` - 定義 job 要做什麼

	- `build-and-push-backend` - 這個位置可以隨便取，代表的是 ==job 的名稱== 所以以上面的角度來說就是有兩個 job 

	- `runs-on` - 指定這個 job 要跑在什麼樣的環境上 
		- github 會自動分配一台 runner 給這個 job

	- `steps` - 定義 job 的各個 step，每一個 step 以 `-` 開頭
	
		- `name` - step 的名稱
		
		- `uses` - 使用預先定義好的 action (由別人寫好的)
		
		- `run` - 要執行的指令

		- `env` - 設定環境變數

		- `with` - 提供 action 需要的參數，像上面使用別人寫好的 action 時，就必須指定需要的 java 發行版以及版本
		


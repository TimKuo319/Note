---
date: 2024-01-08 Thu
---
#Test #Jest

---

- [ ] after_each、spyOn 🔽

這是第一次寫自動化測試，且因為Nest APP在建立的時候就會先安裝好測試所需的相關套件以及做了最基礎的jest設定，所以目前對於jest的配置都還不是很熟悉，這篇筆記主要在記錄測試程式碼的撰寫上。

## Before Test

```ts
describe('TodoService', () => {

  let service: TodoService;

  let todoRepo: Repository<todoEntity>;

  beforeEach(async () => {

    const module: TestingModule = await Test.createTestingModule({

      providers: [

        TodoService,

        {

          provide: getRepositoryToken(todoEntity),

          useValue: mockTodoRepo

        }

      ],  

    }).compile();

  

    service = module.get<TodoService>(TodoService);

    todoRepo = module.get<Repository<todoEntity>>(getRepositoryToken(todoEntity));

  });

  

  afterEach(async () => {

    jest.clearAllMocks()

  })

	...
	
```


在上面的程式碼中，`describe`用來將測試給group在一起，能夠讓程式碼更有組織性，`describe`的第一個參數是一段字串，可以用這段文字來說明test group面對的測試對象。而由於這個是`todoservice`的測試，所以上面程式碼中，字串參數放的位置就寫了`TodoService`。

```ts
beforeEach(async () => {

    const module: TestingModule = await Test.createTestingModule({

      providers: [

        TodoService,

        {

          provide: getRepositoryToken(todoEntity),

          useValue: mockTodoRepo

        }

      ],  

    }).compile();

  

    service = module.get<TodoService>(TodoService);

    todoRepo = module.get<Repository<todoEntity>>(getRepositoryToken(todoEntity));
```

緊接在後的是`beforeEach`，這部分的程式碼就是在每一個testcase被執行前都會進行的行為。在這個部分我們透過Nest內建的Test來建立測試模組。這個測試模組的用意在於他會去模擬NestAPP的執行階段，且我們也可以像開發時提供provider，controller等等，去讓模組加入DI機制中。

因為我們要測試的目標是`TodoService`，所以我們當然要在providers中加入`TodoService`來方便後續使用。而除了`TodoService`外，另外一個provider則是`TodoRepository`，因為我們在與資料庫互動時，是透過`TypeORM`利用repository去與資料庫做溝通。但在做單元測試時，我們並不會想要實際真的去與資料庫溝通，一來是因為這麼做會造成測試的不穩定性，好的測試就是要在===任何狀況下===都能夠執行測試，若是資料庫配置出問題或網路出問題，就會造成我們的測試失效。另一個點是，如果真的與資料庫進行連接，那在我們測試的過程中可能就會去不段新增、刪除、修改資料庫的資料，造成資料紊亂。

所以在這個地方的做法是利用custom provider的方式，去模擬(mock)我們開發時的`todoRepo`，provide利用`getRepositoryToken`來取得todoEntity的token，value則是我們自己mock的`mockTodoRepo`。

```ts
export const mockTodoRepo = {

    find: jest.fn(),

    create: jest.fn(),

    save: jest.fn(),

    findOneBy: jest.fn(),

    delete: jest.fn()

}
```

這五個function是我們在後面測試會用到的repo method，這裡就透過`jest.fn()`來模擬五個空函數。他們的實作及回傳值就留到後面測試做處理。

提供完provider後，就可以利用`testModule.get`去取得對應的instance(*static*)供後續做使用。

既然有`beforeEach`，那當然也有`afterEach`，而就如字面上的意思一樣，`afterEach`會在每一個testCase結束後執行。

```ts
afterEach(async () => {

    jest.clearAllMocks()

  })
```

這裡的`clearAllMocks`會去移除掉mock call以及mock instance的相關屬性，就可以確保我們在每一個testcase裡的mock都是獨立的。

```ts

describe('createTodo', () => {

    it('it should create new todo', async () => {

      const mockTodoEntity = new todoEntity();

      mockTodoEntity.id = 10;

      mockTodoEntity.task = mockTodoDTO.task;

      mockTodoEntity.deadline = mockTodoDTO.deadline;

      mockTodoEntity.completed = mockTodoDTO.completed;

  

      jest.spyOn(todoRepo, 'create').mockReturnValueOnce(mockTodoDTO as todoEntity)

      jest.spyOn(todoRepo, 'save').mockResolvedValueOnce(mockTodoEntity);

      const result = await service.createTodo(mockTodoDTO);

  

      expect(result).toBeInstanceOf(todoEntity);

      expect(result).toBe(mockTodoEntity);

      expect(todoRepo.create).toHaveBeenCalledTimes(1);
	  //確認save被呼叫一次
      expect(todoRepo.save).toHaveBeenCalledTimes(1);

  

    })

  })

  

  describe('updateTodo', () => {

  

    it('should return a update todo', async() => {

      jest.spyOn(todoRepo, 'findOneBy').mockResolvedValueOnce(mockTodoDTO as todoEntity);

      const updateTodo = await service.updateTodo(1, mockTodoDTO);

  

      expect(todoRepo.findOneBy).toHaveBeenCalledWith({ id: 1 });

      expect(todoRepo.save).toHaveBeenCalledTimes(1);
	//確認save被呼叫一次，但如果沒有利用clearAllMocks清除狀態，這裡的save會跨過testcase。所以呼叫次數會變成2次
      expect(todoRepo.save).toHaveBeenCalledWith(mockTodoDTO);

    })

  

    it('should throw an error if todo not found', async() => {

      jest.spyOn(todoRepo, 'findOneBy').mockResolvedValueOnce(undefined);

  

      await expect(service.updateTodo(10, mockTodoDTO)).rejects.toThrow(HttpException);

      expect(todoRepo.findOneBy).toHaveBeenCalledWith({ id: 10 });

      expect(todoRepo.findOneBy).toHaveBeenCalledTimes(1);

    })

  })
```

就以這兩個區塊來說，他們都用到了`toHaveBeenCalledTimes`去確認`todoRepo.save`被呼叫的次數。若是沒有在透過`clearAllMocks`來去清楚mock的狀態，在下方的testcase就會出現問題。

最後則是在過程中混淆我許久的`spyOn`，`spyOn`主要用來重現`DOC(Dependent-on Componenet)`邏輯，也就是會直接去呼叫被spy的function，當然也可以透過`mock..`相關的函數來模擬回傳值。而在我們的程式碼中，因為並不想要與資料庫實際進行連線，先透過`jest.fn`來模擬相關函數為空函數，等到特定testcase需要的時候，我們再利用`jest.spyOn`去修改相ㄍㄨㄢ

 
ref:
[Unit Testing NestJS Applications with Jest: A Beginner’s Guide | by Weerayut Teja | Medium](https://medium.com/@wteja/unit-testing-nestjs-applications-with-jest-a-beginners-guide-a78dfa78541e)

[單元測試之 mock/stub/spy/fake ? 傻傻搞不清楚 | by CraftsmanHenry | Medium](https://medium.com/@henry-chou/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E4%B9%8B-mock-stub-spy-fake-%E5%82%BB%E5%82%BB%E6%90%9E%E4%B8%8D%E6%B8%85%E6%A5%9A-ba3dc4e86d86)

[Jest | JOJO是你？我的替身能力是 Mock ！. Mock 在 Unit Test… | by 神Q超人 | Enjoy life enjoy coding | Medium](https://medium.com/enjoy-life-enjoy-coding/jest-jojo%E6%98%AF%E4%BD%A0-%E6%88%91%E7%9A%84%E6%9B%BF%E8%BA%AB%E8%83%BD%E5%8A%9B%E6%98%AF-mock-4de73596ea6e)
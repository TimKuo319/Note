---
date: 2024-01-08 Thu
---
#Test #Jest

---

- [ ] Jest note 🔽

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

緊接在後的是`beforeEach`，這部分的程式碼就是在每一個testcase被執行前都會進行的行為。在這個部分我們透過Nest內建的Test來建立測試模組。這個測試模組的用意在於他會去磨

jest.fn() jest.spyOn
 
ref:
[Unit Testing NestJS Applications with Jest: A Beginner’s Guide | by Weerayut Teja | Medium](https://medium.com/@wteja/unit-testing-nestjs-applications-with-jest-a-beginners-guide-a78dfa78541e)

[單元測試之 mock/stub/spy/fake ? 傻傻搞不清楚 | by CraftsmanHenry | Medium](https://medium.com/@henry-chou/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E4%B9%8B-mock-stub-spy-fake-%E5%82%BB%E5%82%BB%E6%90%9E%E4%B8%8D%E6%B8%85%E6%A5%9A-ba3dc4e86d86)

[Jest | JOJO是你？我的替身能力是 Mock ！. Mock 在 Unit Test… | by 神Q超人 | Enjoy life enjoy coding | Medium](https://medium.com/enjoy-life-enjoy-coding/jest-jojo%E6%98%AF%E4%BD%A0-%E6%88%91%E7%9A%84%E6%9B%BF%E8%BA%AB%E8%83%BD%E5%8A%9B%E6%98%AF-mock-4de73596ea6e)
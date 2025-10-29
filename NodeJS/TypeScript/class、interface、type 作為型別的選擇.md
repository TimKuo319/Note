
# Class vs Interface vs Type 差異速查筆記

## 📑 目錄

- [核心差異一覽表](##核心差異一覽表)
- [Class：真實存在的藍圖](<##Class：真實存在的藍圖/>)
- [Interface：物件結構定義](<##Interface：物件結構定義/>)
- [Type：強大的型別別名](<##Type：強大的型別別名/>)
- [Interface vs Type 比較](<##Interface vs Type 比較/>)
- [選擇指南](<##選擇指南/>)
- [實務推薦](<##實務推薦/>)
- [記憶口訣](<##記憶口訣/>)
- [編譯對比](<##編譯對比/>)
- [快速範例對比](<##快速範例對比/>)
- [重點提醒](<##重點提醒/>)

---

## 核心差異一覽表

|特性|Class|Interface|Type|
|---|---|---|---|
|**執行時存在**|✅ 是|❌ 否|❌ 否|
|**可實例化**|✅ `new`|❌|❌|
|**有實作**|✅ 有|❌ 無|❌ 無|
|**聯合型別**|❌|❌|✅|
|**宣告合併**|❌|✅|❌|
|**映射型別**|❌|❌|✅|

---

## Class：真實存在的藍圖

**特點：** 執行時存在、可實例化、有實作

```typescript
class User {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    greet() {
        console.log(`Hello, ${this.name}`);
    }
}

// ✅ 可以實例化
const user = new User("Alice");
user.greet();

// ✅ 可當型別使用
function process(u: User) { }

// ✅ 執行時可檢查
console.log(user instanceof User);  // true
```

**何時用：**

- 需要建立實例
- 需要封裝邏輯和狀態
- 需要執行時型別檢查

---

## Interface：物件結構定義

**特點：** 純型別定義、支援宣告合併、編譯後消失

```typescript
interface User {
    name: string;
    age: number;
}

// ✅ 可當型別使用
const user: User = {
    name: "Alice",
    age: 25
};

// ✅ 可被 class 實作
class Person implements User {
    name = "Bob";
    age = 30;
}

// ✅ 支援宣告合併
interface User {
    email: string;  // 自動合併到 User
}

// ❌ 不能實例化
// const u = new User();  // Error
```

**何時用：**

- 定義物件結構（最常見）
- 定義公開 API
- 需要宣告合併（擴展第三方庫）

---

## Type：強大的型別別名

**特點：** 更靈活、支援聯合型別、映射型別、編譯後消失

```typescript
// ✅ 定義物件（類似 Interface）
type User = {
    name: string;
    age: number;
};

// ✅ 聯合型別（Interface 做不到）
type Status = 'pending' | 'success' | 'error';

// ✅ 交集型別
type Admin = User & { role: 'admin' };

// ✅ 原始型別別名
type ID = string | number;

// ✅ 元組
type Point = [number, number];

// ✅ 映射型別
type Readonly<T> = {
    readonly [K in keyof T]: T[K];
};

// ❌ 不支援宣告合併
// type User = { email: string };  // Error: 重複識別碼
```

**何時用：**

- 需要聯合型別
- 需要元組
- 複雜的型別運算
- 原始型別別名

---

## Interface vs Type 比較

### 相同點

```typescript
// 兩者都能定義物件結構
interface IUser {
    name: string;
}

type TUser = {
    name: string;
};

// 兩者都能被 class 實作
class Person implements IUser { }
class Person2 implements TUser { }
```

### 差異點

```typescript
// ✅ 只有 Interface：宣告合併
interface User {
    name: string;
}
interface User {
    age: number;  // 合併成功
}

// ❌ Type 不行
// type Person = { name: string };
// type Person = { age: number };  // Error

// ✅ 只有 Type：聯合型別
type Status = 'active' | 'inactive';
type ID = string | number;

// ✅ 只有 Type：元組
type RGB = [number, number, number];

// ✅ 只有 Type：映射型別
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```

---

## 選擇指南

```
需要建立實例？
├─ 是 → 用 Class
└─ 否 ↓

需要聯合型別？（如 'a' | 'b'）
├─ 是 → 用 Type
└─ 否 ↓

需要複雜型別運算？（映射型別、條件型別）
├─ 是 → 用 Type
└─ 否 ↓

需要宣告合併？（擴展第三方庫）
├─ 是 → 用 Interface
└─ 否 ↓

定義物件結構 → 優先用 Interface（團隊慣例）
```

---

## 實務推薦

### ✅ 常見的良好實踐

```typescript
// 1. 物件結構：優先 Interface
interface User {
    id: string;
    name: string;
}

// 2. 聯合型別：只能 Type
type Status = 'idle' | 'loading' | 'success' | 'error';

// 3. 需要實例化：用 Class
class UserService {
    getUser() { }
}

// 4. 元組：用 Type
type Coordinates = [number, number];

// 5. 工具型別：用 Type
type Nullable<T> = T | null;
type ReadOnly<T> = { readonly [K in keyof T]: T[K] };
```

---

## 記憶口訣

- **Class** = 需要 `new`，有實作
- **Interface** = 定義形狀，可合併
- **Type** = 更強大，做不到合併

---

## 編譯對比

```typescript
// TypeScript 編譯前
class User {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}

interface Person {
    name: string;
}

type Animal = {
    name: string;
};

// ↓ 編譯成 JavaScript 後

// ✅ Class 還在
class User {
    constructor(name) {
        this.name = name;
    }
}

// ❌ Interface 消失
// ❌ Type 消失
```

---

## 快速範例對比

```typescript
// 場景：定義使用者資料

// 方式 1: Interface（最常見）
interface User {
    id: string;
    name: string;
}
const user: User = { id: '1', name: 'Alice' };

// 方式 2: Type（也可以）
type User2 = {
    id: string;
    name: string;
};
const user2: User2 = { id: '1', name: 'Alice' };

// 方式 3: Class（需要實例化時）
class User3 {
    constructor(
        public id: string,
        public name: string
    ) {}
}
const user3 = new User3('1', 'Alice');

// 場景：狀態管理

// ✅ 用 Type（聯合型別）
type LoadingState = 
    | { status: 'idle' }
    | { status: 'loading' }
    | { status: 'success'; data: any }
    | { status: 'error'; error: string };

// 場景：擴展第三方庫

// ✅ 用 Interface（宣告合併）
declare global {
    interface Window {
        myApp: { version: string };
    }
}
```

---

## 重點提醒

1. **預設策略**：物件結構用 Interface，其他特殊需求用 Type
	- 當需要用到 union type 之類的格式，或是格式不太會變動時，可以使用 type
2. **Class vs Interface/Type**：需要實例化就用 Class，否則用 Interface/Type
3. **Interface vs Type**：
    - 需要宣告合併 → Interface
    - 需要聯合型別 → Type
    - 一般物件結構 → Interface（慣例）
4. **Type 更強大**，但 Interface 更適合定義物件結構（語意清楚）

---

現在目錄連結應該正確指向文件內的標題了！在 Markdown 編輯器（如 VS Code、Obsidian、Notion 等）中，點擊目錄項目就能跳到對應章節。

## Reference

- Claude

- [【Day 19】TypeScript 介面(Interface) v.s. 型別別名(Type Alias) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10224646)



# Class vs Interface vs Type 差異速查筆記

## 📑 目錄

- [核心差異一覽表](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#%E6%A0%B8%E5%BF%83%E5%B7%AE%E7%95%B0%E4%B8%80%E8%A6%BD%E8%A1%A8)
- [1️⃣ Class：真實存在的藍圖](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#1%EF%B8%8F%E2%83%A3-class%E7%9C%9F%E5%AF%A6%E5%AD%98%E5%9C%A8%E7%9A%84%E8%97%8D%E5%9C%96)
- [2️⃣ Interface：物件結構定義](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#2%EF%B8%8F%E2%83%A3-interface%E7%89%A9%E4%BB%B6%E7%B5%90%E6%A7%8B%E5%AE%9A%E7%BE%A9)
- [3️⃣ Type：強大的型別別名](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#3%EF%B8%8F%E2%83%A3-type%E5%BC%B7%E5%A4%A7%E7%9A%84%E5%9E%8B%E5%88%A5%E5%88%A5%E5%90%8D)
- [🔄 Interface vs Type 比較](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-interface-vs-type-%E6%AF%94%E8%BC%83)
- [📋 選擇指南（決策樹）](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E9%81%B8%E6%93%87%E6%8C%87%E5%8D%97%E6%B1%BA%E7%AD%96%E6%A8%B9)
- [💡 實務推薦](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E5%AF%A6%E5%8B%99%E6%8E%A8%E8%96%A6)
- [🎯 記憶口訣](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E8%A8%98%E6%86%B6%E5%8F%A3%E8%A8%A3)
- [📊 編譯對比](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E7%B7%A8%E8%AD%AF%E5%B0%8D%E6%AF%94)
- [🔍 快速範例對比](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E5%BF%AB%E9%80%9F%E7%AF%84%E4%BE%8B%E5%B0%8D%E6%AF%94)
- [⚡ 重點提醒](https://claude.ai/chat/6ab2bbdb-9280-442f-af66-c0ac4a0f1f3c#-%E9%87%8D%E9%BB%9E%E6%8F%90%E9%86%92)

---

## 🎯 核心差異一覽表

|特性|Class|Interface|Type|
|---|---|---|---|
|**執行時存在**|✅ 是|❌ 否|❌ 否|
|**可實例化**|✅ `new`|❌|❌|
|**有實作**|✅ 有|❌ 無|❌ 無|
|**聯合型別**|❌|❌|✅|
|**宣告合併**|❌|✅|❌|
|**映射型別**|❌|❌|✅|

---

## 1️⃣ Class：真實存在的藍圖

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

## 2️⃣ Interface：物件結構定義

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

## 3️⃣ Type：強大的型別別名

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

## 🔄 Interface vs Type 比較

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

## 📋 選擇指南（決策樹）

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

## 💡 實務推薦

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

## 🎯 記憶口訣

- **Class** = 需要 `new`，有實作
- **Interface** = 定義形狀，可合併
- **Type** = 更強大，做不到合併

---

## 📊 編譯對比

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

## 🔍 快速範例對比

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

## ⚡ 重點提醒

1. **預設策略**：物件結構用 Interface，其他特殊需求用 Type
2. **Class vs Interface/Type**：需要實例化就用 Class，否則用 Interface/Type
3. **Interface vs Type**：
    - 需要宣告合併 → Interface
    - 需要聯合型別 → Type
    - 一般物件結構 → Interface（慣例）
4. **Type 更強大**，但 Interface 更適合定義物件結構（語意清楚）

---

## Reference

- Claude

- [【Day 19】TypeScript 介面(Interface) v.s. 型別別名(Type Alias) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10224646)


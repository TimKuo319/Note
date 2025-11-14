
# JPA 分頁與排序筆記

## 一、基本概念介紹

在處理大量資料時，我們不可能一次將所有資料都載入記憶體，因此需要：
- **分頁（Pagination）**：將資料分成多個頁面，每次只查詢一頁的資料
- **排序（Sorting）**：按照特定欄位對資料進行排序

Spring Data JPA 提供了強大且簡單的分頁和排序功能。

---

## 二、排序（Sorting）

### 2.1 Sort 類別介紹

`Sort` 是 Spring Data 提供的排序類別，位於 `org.springframework.data.domain.Sort`。

#### 建立 Sort 物件的方法
```java
import org.springframework.data.domain.Sort;

// 方法一：單一欄位升序排序
Sort sort1 = Sort.by("username");

// 方法二：單一欄位降序排序
Sort sort2 = Sort.by(Sort.Direction.DESC, "createdDate");

// 方法三：多欄位排序
Sort sort3 = Sort.by("userType").ascending()
                 .and(Sort.by("createdDate").descending());

// 方法四：使用 Order 物件
Sort sort4 = Sort.by(
    Sort.Order.asc("username"),
    Sort.Order.desc("createdDate")
);

// 方法五：不區分大小寫的排序
Sort sort5 = Sort.by(Sort.Order.asc("username").ignoreCase());
```

#### Sort 的主要方法

| 方法 | 說明 | 範例 |
|------|------|------|
| `Sort.by(String... properties)` | 根據欄位名稱升序排序 | `Sort.by("name")` |
| `Sort.by(Direction direction, String... properties)` | 指定方向排序 | `Sort.by(Sort.Direction.DESC, "age")` |
| `ascending()` | 轉換為升序 | `Sort.by("name").ascending()` |
| `descending()` | 轉換為降序 | `Sort.by("name").descending()` |
| `and(Sort sort)` | 組合多個排序條件 | `sort1.and(sort2)` |
| `Order.ignoreCase()` | 忽略大小寫 | `Sort.Order.asc("name").ignoreCase()` |

### 2.2 排序實際範例

#### 範例 1：基本排序
```java
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;
import lombok.RequiredArgsConstructor;
import java.util.List;

@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 範例 1.1：按照使用者名稱升序排序
    public List<User> getUsersSortedByUsername() {
        Sort sort = Sort.by("username");
        return userRepository.findAll(sort);
    }
    
    // 範例 1.2：按照建立日期降序排序（最新的在前）
    public List<User> getUsersSortedByCreatedDateDesc() {
        Sort sort = Sort.by(Sort.Direction.DESC, "createdDate");
        return userRepository.findAll(sort);
    }
    
    // 範例 1.3：先按使用者類型升序，再按建立日期降序
    public List<User> getUsersWithMultiSort() {
        Sort sort = Sort.by("userType").ascending()
                       .and(Sort.by("createdDate").descending());
        return userRepository.findAll(sort);
    }
}
```

#### 範例 2：在 Repository 中使用排序
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 方法命名查詢 + 排序參數
    List<User> findByActive(Boolean active, Sort sort);
    
    // 查詢特定類型的使用者並排序
    List<User> findByUserType(UserType userType, Sort sort);
    
    // 搜尋使用者名稱並排序
    List<User> findByUsernameContaining(String keyword, Sort sort);
}
```

**使用範例：**
```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 查詢啟用的使用者，按建立日期降序排序
    public List<User> getActiveUsersSorted() {
        Sort sort = Sort.by(Sort.Direction.DESC, "createdDate");
        return userRepository.findByActive(true, sort);
    }
    
    // 查詢管理員，按使用者名稱升序排序
    public List<User> getAdminsSorted() {
        Sort sort = Sort.by("username");
        return userRepository.findByUserType(UserType.ADMIN, sort);
    }
    
    // 搜尋使用者，按相關性（假設）和建立日期排序
    public List<User> searchUsersSorted(String keyword) {
        Sort sort = Sort.by("username").ascending()
                       .and(Sort.by("createdDate").descending());
        return userRepository.findByUsernameContaining(keyword, sort);
    }
}
```

#### 範例 3：動態排序
```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 根據參數動態決定排序欄位和方向
    public List<User> getUsersWithDynamicSort(String sortBy, String direction) {
        Sort.Direction sortDirection = direction.equalsIgnoreCase("desc") 
            ? Sort.Direction.DESC 
            : Sort.Direction.ASC;
        
        Sort sort = Sort.by(sortDirection, sortBy);
        return userRepository.findAll(sort);
    }
}
```
```java
// 在 Controller 中使用
@GetMapping("/users")
public ResponseEntity<List<User>> getUsers(
    @RequestParam(defaultValue = "username") String sortBy,
    @RequestParam(defaultValue = "asc") String direction
) {
    List<User> users = userService.getUsersWithDynamicSort(sortBy, direction);
    return ResponseEntity.ok(users);
}
```

---

## 三、分頁（Pagination）

### 3.1 Pageable 介面介紹

`Pageable` 是 Spring Data 提供的分頁介面，包含了頁碼、每頁大小、排序等資訊。

#### 建立 Pageable 物件

通常使用 `PageRequest` 類別來建立 `Pageable` 物件：
```java
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;

// 方法一：基本分頁（頁碼, 每頁大小）
Pageable pageable1 = PageRequest.of(0, 10);  // 第 0 頁，每頁 10 筆

// 方法二：分頁 + 排序
Pageable pageable2 = PageRequest.of(0, 10, Sort.by("username"));

// 方法三：分頁 + 降序排序
Pageable pageable3 = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "createdDate"));

// 方法四：分頁 + 複雜排序
Sort sort = Sort.by("userType").ascending()
                .and(Sort.by("createdDate").descending());
Pageable pageable4 = PageRequest.of(0, 10, sort);
```

#### PageRequest.of() 方法參數說明
```java
PageRequest.of(int page, int size)
PageRequest.of(int page, int size, Sort sort)
PageRequest.of(int page, int size, Sort.Direction direction, String... properties)
```

| 參數 | 說明 | 注意事項 |
|------|------|----------|
| `page` | 頁碼 | **從 0 開始**（第一頁是 0，不是 1） |
| `size` | 每頁資料筆數 | 必須大於 0 |
| `sort` | 排序物件 | 可選，不提供則不排序 |
| `direction` | 排序方向 | ASC（升序）或 DESC（降序） |
| `properties` | 排序欄位 | 可以多個欄位 |

### 3.2 Page 介面介紹

當使用分頁查詢時，返回的是 `Page<T>` 物件，它包含了分頁資料和分頁資訊。

#### Page 物件的主要方法

| 方法 | 返回類型 | 說明 |
|------|---------|------|
| `getContent()` | `List<T>` | 取得當前頁的資料列表 |
| `getTotalElements()` | `long` | 總資料筆數 |
| `getTotalPages()` | `int` | 總頁數 |
| `getNumber()` | `int` | 當前頁碼（從 0 開始） |
| `getSize()` | `int` | 每頁大小 |
| `getNumberOfElements()` | `int` | 當前頁的實際資料筆數 |
| `isFirst()` | `boolean` | 是否為第一頁 |
| `isLast()` | `boolean` | 是否為最後一頁 |
| `hasNext()` | `boolean` | 是否有下一頁 |
| `hasPrevious()` | `boolean` | 是否有上一頁 |
| `getSort()` | `Sort` | 取得排序資訊 |
| `nextPageable()` | `Pageable` | 取得下一頁的 Pageable |
| `previousPageable()` | `Pageable` | 取得上一頁的 Pageable |

通常會先透過 `PageRequest` 建立 `Pageable`，這個 `Pageable` 物件就是 pagination 的條件，像是第幾頁，page 的大小等等，將這個條件丟到 JPA Repository 的參數後，JPA 就會回傳對應的 `Page<T>` 資料。

### 3.3 分頁實際範例

#### 範例 1：基本分頁
```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;
import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 範例 1.1：基本分頁查詢
    public Page<User> getUsers(int page, int size) {
        // 建立 Pageable 物件（頁碼從 0 開始）
        Pageable pageable = PageRequest.of(page, size);
        
        // 執行分頁查詢
        return userRepository.findAll(pageable);
    }
    
    // 範例 1.2：分頁 + 排序
    public Page<User> getUsersSorted(int page, int size) {
        // 按建立日期降序排序
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by(Sort.Direction.DESC, "createdDate")
        );
        
        return userRepository.findAll(pageable);
    }
    
    // 範例 1.3：查看分頁資訊
    public void displayPageInfo() {
        Pageable pageable = PageRequest.of(0, 10);
        Page<User> page = userRepository.findAll(pageable);
        
        System.out.println("總資料數: " + page.getTotalElements());
        System.out.println("總頁數: " + page.getTotalPages());
        System.out.println("當前頁碼: " + page.getNumber());
        System.out.println("每頁大小: " + page.getSize());
        System.out.println("當前頁資料數: " + page.getNumberOfElements());
        System.out.println("是否第一頁: " + page.isFirst());
        System.out.println("是否最後一頁: " + page.isLast());
        System.out.println("是否有下一頁: " + page.hasNext());
        System.out.println("是否有上一頁: " + page.hasPrevious());
        
        // 取得實際資料
        List<User> users = page.getContent();
        users.forEach(user -> System.out.println(user.getUsername()));
    }
}
```

#### 範例 2：在 Repository 中使用分頁
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 方法命名查詢 + 分頁
    Page<User> findByActive(Boolean active, Pageable pageable);
    
    // 查詢特定類型 + 分頁
    Page<User> findByUserType(UserType userType, Pageable pageable);
    
    // 搜尋 + 分頁
    Page<User> findByUsernameContaining(String keyword, Pageable pageable);
    
    // @Query + 分頁
    @Query("SELECT u FROM User u WHERE u.active = true AND u.userType = :type")
    Page<User> findActiveUsersByType(@Param("type") UserType type, Pageable pageable);
    
    // 複雜查詢 + 分頁
    @Query("SELECT u FROM User u WHERE " +
           "LOWER(u.username) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(u.email) LIKE LOWER(CONCAT('%', :keyword, '%'))")
    Page<User> searchUsers(@Param("keyword") String keyword, Pageable pageable);
}
```

**使用範例：**
```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 查詢啟用的使用者（分頁 + 排序）
    public Page<User> getActiveUsers(int page, int size) {
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by(Sort.Direction.DESC, "createdDate")
        );
        return userRepository.findByActive(true, pageable);
    }
    
    // 搜尋使用者（分頁 + 排序）
    public Page<User> searchUsers(String keyword, int page, int size) {
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by("username").ascending()
        );
        return userRepository.searchUsers(keyword, pageable);
    }
    
    // 查詢特定類型的使用者（分頁 + 複雜排序）
    public Page<User> getUsersByType(UserType type, int page, int size) {
        Sort sort = Sort.by("username").ascending()
                       .and(Sort.by("createdDate").descending());
        Pageable pageable = PageRequest.of(page, size, sort);
        
        return userRepository.findByUserType(type, pageable);
    }
}
```

#### 範例 3：Controller 層的分頁實作
```java
import org.springframework.data.domain.Page;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import lombok.RequiredArgsConstructor;

@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
public class UserController {
    
    private final UserService userService;
    
    // 範例 3.1：基本分頁查詢
    @GetMapping
    public ResponseEntity<Page<User>> getUsers(
        @RequestParam(defaultValue = "0") int page,        // 預設第 0 頁
        @RequestParam(defaultValue = "10") int size        // 預設每頁 10 筆
    ) {
        Page<User> users = userService.getUsers(page, size);
        return ResponseEntity.ok(users);
    }
    
    // 範例 3.2：分頁 + 動態排序
    @GetMapping("/sorted")
    public ResponseEntity<Page<User>> getUsersSorted(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(defaultValue = "username") String sortBy,
        @RequestParam(defaultValue = "asc") String direction
    ) {
        Page<User> users = userService.getUsersWithDynamicSort(page, size, sortBy, direction);
        return ResponseEntity.ok(users);
    }
    
    // 範例 3.3：搜尋 + 分頁
    @GetMapping("/search")
    public ResponseEntity<Page<User>> searchUsers(
        @RequestParam String keyword,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> users = userService.searchUsers(keyword, page, size);
        return ResponseEntity.ok(users);
    }
    
    // 範例 3.4：返回自訂格式的分頁資料
    @GetMapping("/custom")
    public ResponseEntity<PageResponse<UserDTO>> getUsersCustomFormat(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> userPage = userService.getUsers(page, size);
        
        // 轉換為 DTO
        List<UserDTO> content = userPage.getContent().stream()
            .map(this::convertToDTO)
            .collect(Collectors.toList());
        
        // 建立自訂回應格式
        PageResponse<UserDTO> response = new PageResponse<>(
            content,
            userPage.getNumber(),
            userPage.getSize(),
            userPage.getTotalElements(),
            userPage.getTotalPages(),
            userPage.isLast()
        );
        
        return ResponseEntity.ok(response);
    }
    
    private UserDTO convertToDTO(User user) {
        UserDTO dto = new UserDTO();
        dto.setId(user.getId());
        dto.setUsername(user.getUsername());
        dto.setEmail(user.getEmail());
        return dto;
    }
}
```
```java
// 自訂分頁回應格式
@Data
@AllArgsConstructor
class PageResponse<T> {
    private List<T> content;
    private int currentPage;
    private int pageSize;
    private long totalElements;
    private int totalPages;
    private boolean last;
}
```

#### 範例 4：前端分頁導航邏輯
```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 取得使用者列表及分頁導航資訊
    public PaginationInfo<User> getUsersWithNavigation(int page, int size) {
        Pageable pageable = PageRequest.of(page, size, Sort.by("createdDate").descending());
        Page<User> userPage = userRepository.findAll(pageable);
        
        return new PaginationInfo<>(
            userPage.getContent(),           // 資料列表
            userPage.getNumber(),            // 當前頁碼
            userPage.getSize(),              // 每頁大小
            userPage.getTotalElements(),     // 總資料數
            userPage.getTotalPages(),        // 總頁數
            userPage.isFirst(),              // 是否第一頁
            userPage.isLast(),               // 是否最後一頁
            userPage.hasNext(),              // 是否有下一頁
            userPage.hasPrevious()           // 是否有上一頁
        );
    }
}
```
```java
@Data
@AllArgsConstructor
class PaginationInfo<T> {
    private List<T> content;
    private int currentPage;
    private int pageSize;
    private long totalElements;
    private int totalPages;
    private boolean first;
    private boolean last;
    private boolean hasNext;
    private boolean hasPrevious;
}
```

---

## 四、完整應用範例

### 4.1 Entity 定義
```java
@Entity
@Table(name = "users")
@Data
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true)
    private String username;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(name = "user_type")
    private UserType userType;
    
    private Boolean active;
    
    @CreationTimestamp
    @Column(name = "created_date")
    private LocalDate createdDate;
}
```
```java
public enum UserType {
    ADMIN, CUSTOMER, GUEST
}
```

### 4.2 Repository 定義
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 基本查詢 + 分頁
    Page<User> findByActive(Boolean active, Pageable pageable);
    
    // 類型查詢 + 分頁
    Page<User> findByUserType(UserType userType, Pageable pageable);
    
    // 搜尋 + 分頁
    Page<User> findByUsernameContainingIgnoreCase(String keyword, Pageable pageable);
    
    // 複雜搜尋 + 分頁
    @Query("SELECT u FROM User u WHERE " +
           "LOWER(u.username) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(u.email) LIKE LOWER(CONCAT('%', :keyword, '%'))")
    Page<User> searchUsers(@Param("keyword") String keyword, Pageable pageable);
    
    // 日期範圍查詢 + 分頁
    Page<User> findByCreatedDateBetween(
        LocalDate startDate, 
        LocalDate endDate, 
        Pageable pageable
    );
}
```

### 4.3 Service 實作
```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    /**
     * 查詢所有使用者（分頁 + 排序）
     */
    public Page<User> getAllUsers(int page, int size, String sortBy, String direction) {
        Sort.Direction sortDirection = direction.equalsIgnoreCase("desc") 
            ? Sort.Direction.DESC 
            : Sort.Direction.ASC;
        
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sortBy));
        return userRepository.findAll(pageable);
    }
    
    /**
     * 查詢啟用的使用者（分頁 + 預設排序）
     */
    public Page<User> getActiveUsers(int page, int size) {
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by(Sort.Direction.DESC, "createdDate")
        );
        return userRepository.findByActive(true, pageable);
    }
    
    /**
     * 根據類型查詢（分頁 + 複雜排序）
     */
    public Page<User> getUsersByType(UserType type, int page, int size) {
        // 先按使用者名稱升序，再按建立日期降序
        Sort sort = Sort.by("username").ascending()
                       .and(Sort.by("createdDate").descending());
        
        Pageable pageable = PageRequest.of(page, size, sort);
        return userRepository.findByUserType(type, pageable);
    }
    
    /**
     * 搜尋使用者（分頁 + 排序）
     */
    public Page<User> searchUsers(String keyword, int page, int size) {
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by("username").ascending()
        );
        return userRepository.searchUsers(keyword, pageable);
    }
    
    /**
     * 日期範圍查詢（分頁 + 排序）
     */
    public Page<User> getUsersByDateRange(
        LocalDate startDate, 
        LocalDate endDate, 
        int page, 
        int size
    ) {
        Pageable pageable = PageRequest.of(
            page, 
            size, 
            Sort.by(Sort.Direction.DESC, "createdDate")
        );
        return userRepository.findByCreatedDateBetween(startDate, endDate, pageable);
    }
    
    /**
     * 取得分頁統計資訊
     */
    public PageStatistics getPageStatistics(int page, int size) {
        Pageable pageable = PageRequest.of(page, size);
        Page<User> userPage = userRepository.findAll(pageable);
        
        return PageStatistics.builder()
            .totalElements(userPage.getTotalElements())
            .totalPages(userPage.getTotalPages())
            .currentPage(userPage.getNumber() + 1)  // 轉換為從 1 開始
            .pageSize(userPage.getSize())
            .numberOfElements(userPage.getNumberOfElements())
            .isFirst(userPage.isFirst())
            .isLast(userPage.isLast())
            .hasNext(userPage.hasNext())
            .hasPrevious(userPage.hasPrevious())
            .build();
    }
}
```
```java
@Data
@Builder
class PageStatistics {
    private long totalElements;
    private int totalPages;
    private int currentPage;
    private int pageSize;
    private int numberOfElements;
    private boolean isFirst;
    private boolean isLast;
    private boolean hasNext;
    private boolean hasPrevious;
}
```

### 4.4 Controller 實作
```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
public class UserController {
    
    private final UserService userService;
    
    /**
     * GET /api/users?page=0&size=10&sortBy=username&direction=asc
     * 查詢所有使用者（支援分頁和動態排序）
     */
    @GetMapping
    public ResponseEntity<Page<User>> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(defaultValue = "username") String sortBy,
        @RequestParam(defaultValue = "asc") String direction
    ) {
        Page<User> users = userService.getAllUsers(page, size, sortBy, direction);
        return ResponseEntity.ok(users);
    }
    
    /**
     * GET /api/users/active?page=0&size=10
     * 查詢啟用的使用者
     */
    @GetMapping("/active")
    public ResponseEntity<Page<User>> getActiveUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> users = userService.getActiveUsers(page, size);
        return ResponseEntity.ok(users);
    }
    
    /**
     * GET /api/users/type/ADMIN?page=0&size=10
     * 根據類型查詢使用者
     */
    @GetMapping("/type/{type}")
    public ResponseEntity<Page<User>> getUsersByType(
        @PathVariable UserType type,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> users = userService.getUsersByType(type, page, size);
        return ResponseEntity.ok(users);
    }
    
    /**
     * GET /api/users/search?keyword=alice&page=0&size=10
     * 搜尋使用者
     */
    @GetMapping("/search")
    public ResponseEntity<Page<User>> searchUsers(
        @RequestParam String keyword,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> users = userService.searchUsers(keyword, page, size);
        return ResponseEntity.ok(users);
    }
    
    /**
     * GET /api/users/statistics?page=0&size=10
     * 取得分頁統計資訊
     */
    @GetMapping("/statistics")
    public ResponseEntity<PageStatistics> getStatistics(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        PageStatistics stats = userService.getPageStatistics(page, size);
        return ResponseEntity.ok(stats);
    }
}
```

### 4.5 測試範例
```java
@SpringBootTest
@Transactional
public class UserPaginationTest {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private UserService userService;
    
    @BeforeEach
    void setUp() {
        // 建立測試資料
        for (int i = 1; i <= 25; i++) {
            User user = new User();
            user.setUsername("user" + i);
            user.setEmail("user" + i + "@example.com");
            user.setUserType(i % 3 == 0 ? UserType.ADMIN : UserType.CUSTOMER);
            user.setActive(i % 2 == 0);
            userRepository.save(user);
        }
    }
    
    @Test
    void testBasicPagination() {
        // 第一頁，每頁 10 筆
        Page<User> page1 = userService.getAllUsers(0, 10, "username", "asc");
        
        assertEquals(10, page1.getContent().size());
        assertEquals(25, page1.getTotalElements());
        assertEquals(3, page1.getTotalPages());
        assertTrue(page1.isFirst());
        assertFalse(page1.isLast());
        assertTrue(page1.hasNext());
        assertFalse(page1.hasPrevious());
        
        // 第二頁
        Page<User> page2 = userService.getAllUsers(1, 10, "username", "asc");
        
        assertEquals(10, page2.getContent().size());
        assertFalse(page2.isFirst());
        assertFalse(page2.isLast());
        assertTrue(page2.hasNext());
        assertTrue(page2.hasPrevious());
        
        // 第三頁（最後一頁）
        Page<User> page3 = userService.getAllUsers(2, 10, "username", "asc");
        
        assertEquals(5, page3.getContent().size());
        assertFalse(page3.isFirst());
        assertTrue(page3.isLast());
        assertFalse(page3.hasNext());
        assertTrue(page3.hasPrevious());
    }
    
    @Test
    void testSortingWithPagination() {
        // 按使用者名稱升序
        Page<User> ascPage = userService.getAllUsers(0, 5, "username", "asc");
        List<User> ascUsers = ascPage.getContent();
        
        assertEquals("user1", ascUsers.get(0).getUsername());
        assertEquals("user10", ascUsers.get(1).getUsername());
        
        // 按使用者名稱降序
        Page<User> descPage = userService.getAllUsers(0, 5, "username", "desc");
        List<User> descUsers = descPage.getContent();
        
        assertEquals("user9", descUsers.get(0).getUsername());
    }
    
    @Test
    void testSearchWithPagination() {
        Page<User> searchResult = userService.searchUsers("user1", 0, 10);
        
        // 應該找到 user1, user10, user11, ..., user19 共 11 筆
        assertTrue(searchResult.getTotalElements() >= 11);
        
        // 驗證搜尋結果
        searchResult.getContent().forEach(user -> 
            assertTrue(user.getUsername().contains("user1"))
        );
    }
}
```

---

## 五、最佳實踐與注意事項

### 5.1 頁碼從 0 開始
```java
// ⚠️ 注意：JPA 的頁碼從 0 開始
PageRequest.of(0, 10);  // 第 1 頁
PageRequest.of(1, 10);  // 第 2 頁
PageRequest.of(2, 10);  // 第 3 頁

// 如果前端傳入的頁碼從 1 開始，需要轉換
public Page<User> getUsers(int pageFromFrontend, int size) {
    int page = pageFromFrontend - 1;  // 轉換為從 0 開始
    Pageable pageable = PageRequest.of(page, size);
    return userRepository.findAll(pageable);
}
```

### 5.2 合理設定每頁大小
```java
@GetMapping("/users")
public ResponseEntity<Page<User>> getUsers(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "10") int size
) {
    // 限制每頁大小，避免一次查詢過多資料
    if (size > 100) {
        size = 100;
    }
    
    Page<User> users = userService.getUsers(page, size);
    return ResponseEntity.ok(users);
}
```

### 5.3 動態排序的安全性
```java
@Service
public class UserService {
    
    private static final Set<String> ALLOWED_SORT_FIELDS = Set.of(
        "username", "email", "createdDate", "userType"
    );
    
    public Page<User> getUsersSafely(int page, int size, String sortBy, String direction) {
        // 驗證排序欄位，防止 SQL 注入
        if (!ALLOWED_SORT_FIELDS.contains(sortBy)) {
            sortBy = "username";  // 使用預設欄位
        }
        
        Sort.Direction sortDirection = direction.equalsIgnoreCase("desc") 
            ? Sort.Direction.DESC 
            : Sort.Direction.ASC;
        
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sortBy));
        return userRepository.findAll(pageable);
    }
}
```

### 5.4 效能考量
```java
// ✅ 好的做法：只查詢需要的欄位
@Query("SELECT new com.example.demo.dto.UserDTO(u.id, u.username, u.email) " +
       "FROM User u")
Page<UserDTO> findAllUserDTOs(Pageable pageable);

// ❌ 避免：查詢過多關聯資料導致 N+1 問題
// 如果有關聯查詢，考慮使用 JOIN FETCH
@Query("SELECT u FROM User u LEFT JOIN FETCH u.courses")
Page<User> findAllWithCourses(Pageable pageable);
```

### 5.5 前端整合範例
```javascript
// 前端 API 呼叫範例（使用 Axios）
async function fetchUsers(page = 1, size = 10, sortBy = 'username', direction = 'asc') {
    try {
        const response = await axios.get('/api/users', {
            params: {
                page: page - 1,  // 後端從 0 開始，前端從 1 開始
                size: size,
                sortBy: sortBy,
                direction: direction
            }
        });
        
        const pageData = response.data;
        console.log('資料列表:', pageData.content);
        console.log('總資料數:', pageData.totalElements);
        console.log('總頁數:', pageData.totalPages);
        console.log('當前頁:', pageData.number + 1);  // 轉換為從 1 開始
        
        return pageData;
    } catch (error) {
        console.error('查詢失敗:', error);
    }
}

// 使用範例
fetchUsers(1, 10, 'createdDate', 'desc');  // 第 1 頁，每頁 10 筆，按建立日期降序
```

---

## 六、總結

### 排序（Sort）重點

- 使用 `Sort.by()` 建立排序物件
- 支援單一或多欄位排序
- 可指定升序（ASC）或降序（DESC）
- 使用 `and()` 串接多個排序條件

### 分頁（Pageable）重點

- 使用 `PageRequest.of()` 建立分頁物件
- 頁碼**從 0 開始**
- 返回 `Page<T>` 物件，包含資料和分頁資訊
- 可與排序結合使用

### 實務應用技巧

1. 在 Repository 方法中加入 `Pageable` 參數
2. 在 Service 層建立 Pageable 物件
3. 在 Controller 層接收分頁參數
4. 注意頁碼轉換（前端通常從 1 開始）
5. 限制每頁最大資料量
6. 驗證排序欄位避免注入攻擊


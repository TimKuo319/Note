## What is JPA Repository 

JPA Repository 是 Spring Data JPA 提供的一個**資料存取層（DAO）介面**，它讓你可以用**極少的程式碼**來操作資料庫，不需要自己寫 SQL 或 JPQL。

## Repository 階層架構 

Repository -> CrudRepository -> PagingAndSortingRepository -> JpaRepository 

### Crud Repository 

提供最基本的 CRUD 操作

```java
import org.springframework.data.repository.CrudRepository;

public interface UserRepository extends CrudRepository<User, Long> {
    // User: Entity 類型
    // Long: Primary Key 的類型
    
    // 不需要寫任何方法，就已經有以下功能：
    // - save(entity)           儲存
    // - findById(id)           根據 ID 查詢
    // - findAll()              查詢全部
    // - count()                計算總數
    // - deleteById(id)         根據 ID 刪除
    // - delete(entity)         刪除實體
    // - existsById(id)         檢查是否存在
}
```


#### 基本使用範例

```java
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    // 儲存（新增或更新）
    public User saveUser(User user) {
        return userRepository.save(user);
    }
    
    // 根據 ID 查詢
    public User getUserById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("User not found"));
    }
    
    // 查詢全部
    public List<User> getAllUsers() {
        return (List<User>) userRepository.findAll();
    }
    
    // 檢查是否存在
    public boolean userExists(Long id) {
        return userRepository.existsById(id);
    }
    
    // 計算總數
    public long countUsers() {
        return userRepository.count();
    }
    
    // 刪除
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

### JpaRepository

`JpaRepository` 是最常用的，它繼承了 `CrudRepository` 和 `PagingAndSortingRepository`，提供了最完整的功能：

```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    // 繼承了所有 CrudRepository 的方法
    // 額外增加了：
    // - flush()                        立即同步到資料庫
    // - saveAndFlush(entity)           儲存並立即同步
    // - deleteInBatch(entities)        批次刪除
    // - deleteAllInBatch()             批次刪除全部
    // - getOne(id)                     取得參考（延遲載入）
    // - findAll(Sort sort)             排序查詢
    // - findAll(Pageable pageable)     分頁查詢
}
```

### Query Methods

從上面的 repository 基本功能可以看到，repository 幾乎只有針對整個 entity 或是 primary key 的操作。

因此 JPA 也提供了 Query Methods 的方式，只需要**按照命名規則定義方法名稱**，Spring 會自動實作查詢邏輯。

| 關鍵字                  | 範例                          | 對應的 SQL                        |
| -------------------- | --------------------------- | ------------------------------ |
| **And**              | `findByNameAndEmail`        | `WHERE name = ? AND email = ?` |
| **Or**               | `findByNameOrEmail`         | `WHERE name = ? OR email = ?`  |
| **Is, Equals**       | `findByName`                | `WHERE name = ?`               |
| **Between**          | `findByDateBetween`         | `WHERE date BETWEEN ? AND ?`   |
| **LessThan**         | `findByAgeLessThan`         | `WHERE age < ?`                |
| **LessThanEqual**    | `findByAgeLessThanEqual`    | `WHERE age <= ?`               |
| **GreaterThan**      | `findByAgeGreaterThan`      | `WHERE age > ?`                |
| **GreaterThanEqual** | `findByAgeGreaterThanEqual` | `WHERE age >= ?`               |
| **After**            | `findByDateAfter`           | `WHERE date > ?`               |
| **Before**           | `findByDateBefore`          | `WHERE date < ?`               |
| **IsNull**           | `findByNameIsNull`          | `WHERE name IS NULL`           |
| **IsNotNull**        | `findByNameIsNotNull`       | `WHERE name IS NOT NULL`       |
| **Like**             | `findByNameLike`            | `WHERE name LIKE ?`            |
| **NotLike**          | `findByNameNotLike`         | `WHERE name NOT LIKE ?`        |
| **StartingWith**     | `findByNameStartingWith`    | `WHERE name LIKE '?%'`         |
| **EndingWith**       | `findByNameEndingWith`      | `WHERE name LIKE '%?'`         |
| **Containing**       | `findByNameContaining`      | `WHERE name LIKE '%?%'`        |
| **OrderBy**          | `findByNameOrderByAgeDesc`  | `ORDER BY age DESC`            |
| **Not**              | `findByNameNot`             | `WHERE name <> ?`              |
| **In**               | `findByNameIn(List)`        | `WHERE name IN (?)`            |
| **NotIn**            | `findByNameNotIn(List)`     | `WHERE name NOT IN (?)`        |
| **True**             | `findByActiveTrue`          | `WHERE active = true`          |
| **False**            | `findByActiveFalse`         | `WHERE active = false`         |
| **IgnoreCase**       | `findByNameIgnoreCase`      | `WHERE LOWER(name) = LOWER(?)` |

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 根據欄位查詢
    User findByUsername(String username);
    
    // 根據多個欄位查詢
    User findByUsernameAndEmail(String username, String email);
    
    // 模糊查詢
    List<User> findByUsernameLike(String username);
    List<User> findByUsernameContaining(String username);
    List<User> findByUsernameStartingWith(String prefix);
    List<User> findByUsernameEndingWith(String suffix);
    
    // 忽略大小寫
    User findByUsernameIgnoreCase(String username);
    
    // 排序
    List<User> findByUsernameOrderByCreatedDateDesc(String username);
    
    // 範圍查詢
    List<User> findByCreatedDateBetween(LocalDate start, LocalDate end);
    
    // 大於、小於
    List<User> findByIdGreaterThan(Long id);
    List<User> findByIdLessThan(Long id);
    
    // In 查詢
    List<User> findByUsernameIn(List<String> usernames);
    
    // 布林值查詢
    List<User> findByActiveTrue();
    List<User> findByActiveFalse();
    
    // Null 查詢
    List<User> findByEmailIsNull();
    List<User> findByEmailIsNotNull();
    
    // 限制結果數量
    User findFirstByUsername(String username);
    List<User> findTop5ByOrderByCreatedDateDesc();
    
    // 檢查是否存在
    boolean existsByUsername(String username);
    boolean existsByEmail(String email);
    
    // 計算數量
    long countByActive(Boolean active);
    long countByUserType(UserType userType);
    
    // 刪除
    void deleteByUsername(String username);
    long deleteByActiveFlase();  // 返回刪除的數量
}
```


### `@Query` 自訂查詢

JPA 最大的優點就是能透過各種簡化的定義來達到滿足許多基本查詢的方式，而取而代之的當然就是在面對複雜查詢的時候會比較吃力。

當遇到基本功能與 Query  Methods 都沒辦法滿足操作的時候，就可以透過 `@Query` 來自己撰寫 query 進行查詢。

而撰寫 query 也同樣分成兩種方式

1. 使用 JPA 提供的 JPQL
	- JPQL 說明可以參考 [[JPA Introduction]]
2. 使用原生 SQL 語法查詢


```java
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 基本 JPQL 查詢
    @Query("SELECT u FROM User u WHERE u.username = ?1")
    User findByUsernameJPQL(String username);
    
    // 使用命名參數（推薦）
    @Query("SELECT u FROM User u WHERE u.username = :username AND u.email = :email")
    User findByUsernameAndEmailJPQL(
        @Param("username") String username, 
        @Param("email") String email
    );
    
    // 模糊查詢
    @Query("SELECT u FROM User u WHERE u.username LIKE %:keyword%")
    List<User> searchByUsername(@Param("keyword") String keyword);
    
    // 複雜查詢
    @Query("SELECT u FROM User u WHERE u.active = true AND u.userType = :type ORDER BY u.createdDate DESC")
    List<User> findActiveUsersByType(@Param("type") UserType type);
    
    // 聯表查詢（假設 User 有 courses 關聯）
    @Query("SELECT u FROM User u JOIN u.courses c WHERE c.name = :courseName")
    List<User> findUsersEnrolledInCourse(@Param("courseName") String courseName);
    
    // 計算
    @Query("SELECT COUNT(u) FROM User u WHERE u.active = true")
    long countActiveUsers();
    
    // 只查詢部分欄位
    @Query("SELECT u.username, u.email FROM User u WHERE u.active = true")
    List<Object[]> findActiveUserInfo();
    
    // 使用 DTO 投影
    @Query("SELECT new com.example.demo.dto.UserDTO(u.username, u.email) FROM User u")
    List<UserDTO> findAllUserDTOs();
}

```

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 原生 SQL 查詢
    @Query(value = "SELECT * FROM users WHERE username = :username", nativeQuery = true)
    User findByUsernameNative(@Param("username") String username);
    
    // 複雜的原生 SQL
    @Query(value = """
        SELECT u.* 
        FROM users u
        LEFT JOIN student_course sc ON u.id = sc.student_id
        GROUP BY u.id
        HAVING COUNT(sc.course_id) > :minCourses
        """, nativeQuery = true)
    List<User> findUsersWithMinCourses(@Param("minCourses") int minCourses);
    
    // 更新查詢
    @Modifying
    @Query("UPDATE User u SET u.active = :active WHERE u.id = :id")
    int updateUserActive(@Param("id") Long id, @Param("active") Boolean active);
    
    // 刪除查詢
    @Modifying
    @Query("DELETE FROM User u WHERE u.active = false")
    int deleteInactiveUsers();
}
```



## Reference

- [JPA Query Methods :: Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html#jpa.query-methods.query-creation)

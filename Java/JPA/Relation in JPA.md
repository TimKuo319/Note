---
tags:
  - Java
  - ORM
---
## Startup

透過 JPA，我們可以透過一些常見的 annotation 去對 entity 做關聯，以下是基本的使用方式。

## Usage

### OneToOne

下面是 OneToOne 的程式碼，從兩個 entity 中可以看到，上面個別加上了 `@OneToOne` 的 annotation，代表的是兩者是一對一的關聯關係。`@OneToOne` 的 `mappedBy` 屬性是指 *受到哪一個 entity 管理* ，被管理的 entity 稱為 *被控端* ，而管理的則稱為 *主控端* ，以下面的例子來說，`User` 就是主控端，`Profile` 就是被控端。

`@JoinColumn` 的話則是會在 User entity 中加上名為 `profile_id` 的欄位，參考的是 `Profile` table 中的 `id` 欄位。

>[!info] 
>1. mappedBy 要填寫的是在 owing entity 中的屬性名稱
>2. @JoinColumn 會使得 entity 在資料庫中加上該外鍵欄位，通常是加在被控端上管理


```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id", referencedColumnName = "id")
    private Profile profile;

    // Getters and Setters
}
```

```java
@Entity
public class Profile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String address;

    @OneToOne(mappedBy = "profile")
    private User user;

    // Getters and Setters
}

```

>[!info] 
>mappedby 填入的對象是在 owner 的屬性名稱。
>
>以上面的狀況來說，Profile entity 裡面要填寫的是在 User 中的屬性名稱 “profile"。如果 User entity 裡的名字被改成 "profiles" ，則在 Profile Entity 中 user 上方的 mappedBy 屬性也要填寫 "profiles"





## Owing Side(控制端) vs Inverse Side(被控端)

在 JPA 的關聯關係中，存在著 Owing Side 與 Inverse Side。

owing Side 是持有外鍵屬性，負責更新 join table、 join column 的一端。而 inverse Side 則是作為關聯查詢用途，並不會參與 join table 的更動，通常會以 `@mappedBy` 來指向 owing side。(上面程式碼中的 Profile 就是 inverse side)

而為何需要理解 owing side 與 inverse side 呢？

就如同前面所提到的，只有 owing side 本身會參與 join table 或 join column 的一端，所以在程式語言上如果在更新資料時是更新 inverse side 的話，實際改變的只是記憶體中的物件狀態，並不會去影響到 DB 中彼此 join column 的欄位，進而導致資料出錯。是使用上需要注意的部分。

**資料庫結構：**
```sql
-- User table
CREATE TABLE user (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    profile_id BIGINT UNIQUE,  -- 外鍵，參考 profile.id
    FOREIGN KEY (profile_id) REFERENCES profile(id)
);

-- Profile table
CREATE TABLE profile (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    address VARCHAR(255) NOT NULL
);
```

### OneToMany / ManyToOne

這是最常見的關聯關係，例如一個部門有多個員工（Department ↔ Employee）。這種關係通常會同時使用 `@OneToMany` 和 `@ManyToOne` 來建立雙向關聯。

#### 規則說明
- **ManyToOne 端（多的那方）是主控端（Owning Side）**：因為「多」的那端持有外鍵
- **OneToMany 端（一的那方）是被控端（Inverse Side）**：使用 `mappedBy` 指向主控端的屬性名稱
- `@JoinColumn` 通常放在 `@ManyToOne` 端，指定外鍵欄位名稱
```java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    // OneToMany 端是被控端，使用 mappedBy
    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Employee> employees = new ArrayList<>();
    
    // 便利方法：維護雙向關聯
    public void addEmployee(Employee employee) {
        employees.add(employee);
        employee.setDepartment(this);
    }
    
    public void removeEmployee(Employee employee) {
        employees.remove(employee);
        employee.setDepartment(null);
    }
    
    // Getters and Setters
}
```
```java
@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    // ManyToOne 端是主控端，持有外鍵
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id", nullable = false)
    private Department department;
    
    // Getters and Setters
}
```

>[!info]
>1. `@OneToMany` 端的 `mappedBy` 填寫的是 `@ManyToOne` 端的屬性名稱
>2. 外鍵欄位 `department_id` 會建立在 Employee table 中
>3. 建議在 `@OneToMany` 端提供便利方法來維護雙向關聯的一致性

**資料庫結構：**
```sql
-- Department table
CREATE TABLE department (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL
);

-- Employee table
CREATE TABLE employee (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    department_id BIGINT NOT NULL,  -- 外鍵，參考 department.id
    FOREIGN KEY (department_id) REFERENCES department(id)
);
```

**使用範例：**
```java
@Service
public class DepartmentService {
    @Autowired
    private DepartmentRepository departmentRepository;
    
    public Department createDepartmentWithEmployees() {
        Department dept = new Department();
        dept.setName("Engineering");
        
        Employee emp1 = new Employee();
        emp1.setName("Alice");
        
        Employee emp2 = new Employee();
        emp2.setName("Bob");
        
        // 使用便利方法維護雙向關聯
        dept.addEmployee(emp1);
        dept.addEmployee(emp2);
        
        // 因為有 cascade = CascadeType.ALL，儲存 Department 時會連同 Employee 一起儲存
        return departmentRepository.save(dept);
    }
}
```

### ManyToMany

多對多關係需要一個中間表（Join Table）來維護關聯，例如學生與課程的關係（一個學生可以選修多門課程，一門課程可以被多個學生選修）。

#### 規則說明
- **必須指定一方為主控端（Owning Side）**：持有 `@JoinTable`
- **另一方為被控端（Inverse Side）**：使用 `mappedBy`
- 主控端決定中間表的結構和名稱
- 中間表包含兩個外鍵，分別參考兩個實體的主鍵
```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    // Student 是主控端，定義中間表
    @ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
    @JoinTable(
        name = "student_course",  // 中間表名稱
        joinColumns = @JoinColumn(name = "student_id"),  // 當前實體的外鍵
        inverseJoinColumns = @JoinColumn(name = "course_id")  // 對方實體的外鍵
    )
    private Set<Course> courses = new HashSet<>();
    
    // 便利方法：維護雙向關聯
    public void enrollCourse(Course course) {
        courses.add(course);
        course.getStudents().add(this);
    }
    
    public void dropCourse(Course course) {
        courses.remove(course);
        course.getStudents().remove(this);
    }
    
    // Getters and Setters
}
```
```java
@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    // Course 是被控端，使用 mappedBy
    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
    
    // Getters and Setters
}
```

>[!info]
>1. `@ManyToMany` 關係通常使用 `Set` 而非 `List`，以避免重複和提升效能
>
>2. `通常會在最開始先初始化一個空的 set 來避免撈取的時候出現問題，當 db 在讀取到資訊後會將這個空的 set 替換成實際撈取出來的資料`
>
>3. `mappedBy` 填寫的是主控端的集合屬性名稱
>
>4. 中間表由 JPA 自動管理，不需要建立對應的 Entity
>
>5. 通常不使用 `CascadeType.ALL`，避免誤刪關聯的實體


**資料庫結構：**
```sql
-- Student table
CREATE TABLE student (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL
);

-- Course table
CREATE TABLE course (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL
);

-- 中間表（Join Table）
CREATE TABLE student_course (
    student_id BIGINT NOT NULL,
    course_id BIGINT NOT NULL,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(id),
    FOREIGN KEY (course_id) REFERENCES course(id)
);
```

**使用範例：**
```java
@Service
public class StudentService {
    @Autowired
    private StudentRepository studentRepository;
    
    @Autowired
    private CourseRepository courseRepository;
    
    public void enrollStudentInCourse(Long studentId, Long courseId) {
        Student student = studentRepository.findById(studentId)
            .orElseThrow(() -> new EntityNotFoundException("Student not found"));
        Course course = courseRepository.findById(courseId)
            .orElseThrow(() -> new EntityNotFoundException("Course not found"));
        
        // 使用便利方法維護雙向關聯
        student.enrollCourse(course);
        
        // 只需要儲存主控端（Student）
        studentRepository.save(student);
    }
    
    public List<Course> getStudentCourses(Long studentId) {
        Student student = studentRepository.findById(studentId)
            .orElseThrow(() -> new EntityNotFoundException("Student not found"));
        return new ArrayList<>(student.getCourses());
    }
}
```

### ManyToMany 進階：使用獨立的關聯實體

當中間表需要額外的屬性（如註冊日期、成績等）時，建議將中間表建立為獨立的 Entity，並使用兩個 `@ManyToOne` 關聯。
```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @OneToMany(mappedBy = "student", cascade = CascadeType.ALL, orphanRemoval = true)
    private Set<Enrollment> enrollments = new HashSet<>();
    
    // Getters and Setters
}
```
```java
@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @OneToMany(mappedBy = "course", cascade = CascadeType.ALL, orphanRemoval = true)
    private Set<Enrollment> enrollments = new HashSet<>();
    
    // Getters and Setters
}
```
```java
@Entity
@Table(name = "enrollment")
public class Enrollment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "student_id", nullable = false)
    private Student student;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "course_id", nullable = false)
    private Course course;
    
    @Column(nullable = false)
    private LocalDateTime enrollmentDate;
    
    private Integer grade;  // 成績
    
    // Getters and Setters
}
```

**資料庫結構：**
```sql
CREATE TABLE enrollment (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    student_id BIGINT NOT NULL,
    course_id BIGINT NOT NULL,
    enrollment_date DATETIME NOT NULL,
    grade INTEGER,
    FOREIGN KEY (student_id) REFERENCES student(id),
    FOREIGN KEY (course_id) REFERENCES course(id),
    UNIQUE (student_id, course_id)  -- 防止重複註冊
);
```


>[!info]
> 在上面的做法中，在有關聯的 entity 兩端都加上 annotation 的做法是雙向關聯，如果只有某一方會需要經常做兩個表之間的查詢，其實只要在一個 entity 加上關聯即可，同樣可以達到增加 foreign key 的效果
## Reference

[Demystifying JPA Many-to-Many Relationships: Focus on Owning and Inverse Sides | by Rashid Mammadli | Medium](https://medium.com/@devrashid/demystifying-jpa-many-to-many-relationships-focus-on-owning-and-inverse-sides-54ba0a21af0b)


# Comprehensive Guide on Creating Entities, VOs, and DTOs using Lombok

In this guide, we’ll explore how to utilize Lombok annotations effectively while creating Entities, Value Objects (VOs), and Data Transfer Objects (DTOs) in Java applications. This guide will include the use of annotations such as `@Data`, `@Builder`, `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor`, and features from Hibernate including auto-generated IDs, composite IDs, lazy relationships with `EntityGraph`, and optional Envers auditing.

## 1. Introduction to Lombok
Lombok is a Java library that helps to reduce boilerplate code by generating getters and setters, among other things, through annotations. This allows developers to focus on the logic rather than repetitive code.

### 1.1 Key Annotations
- `@Data`: This annotation combines `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, and `@RequiredArgsConstructor` in one.
- `@Builder`: Implements the builder pattern, allowing for more readable object creation.
- `@Getter` and `@Setter`: Used to generate getter and setter methods for fields.
- `@NoArgsConstructor`: Generates a no-argument constructor.
- `@AllArgsConstructor`: Generates a constructor with all fields as parameters.

## 2. Creating Entities with Hibernate
Entities in Hibernate map to database tables. Below is a simple entity example:
```java
import lombok.*;
import javax.persistence.*;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;

    private String password;
}
```

### 2.1 Auto-Generated IDs
The `@GeneratedValue` annotation automatically generates primary keys for us upon insertion.

### 2.2 Composite IDs
For composite keys, we often use the `@IdClass` or `@EmbeddedId` annotations. Here’s how you can create a composite ID:
```java
@Embeddable
@Data
public class UserId implements Serializable {
    private String username;
    private String domain;
}

@Entity
public class User {
    @EmbeddedId
    private UserId id;

    // additional properties
}
```

## 3. Value Objects (VOs)
Value Objects are immutable and should be created without setters. Here's how you can create a VO:
```java
import lombok.*;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Address {
    private String street;
    private String city;
}
```

## 4. Data Transfer Objects (DTOs)
DTOs are used to transfer data between layers. Here's a simple DTO example:
```java
import lombok.*;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class UserDTO {
    private String username;
    private String password;
}
```

## 5. Relationships with Hibernate
### 5.1 Lazy Loading with EntityGraph
You can use `EntityGraph` to specify fetch plans for entity relationships and control lazy loading:
```java
@Entity
@NamedEntityGraph(name = "User.detail", attributePaths = {"address"})
public class User {
    // fields and methods
}
```

### 5.2 Optional Envers Auditing
If you wish to track changes in your entities, you can use Hibernate Envers. Simply annotate your entity:
```java
@Entity
@Audited
public class User {
    // fields
}
```

## Conclusion
Utilizing Lombok annotations alongside Hibernate features can lead to cleaner, more maintainable code. This guide serves as a base for implementing such practices in your Java applications.
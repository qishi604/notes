# GreenDao 3.0

## Gradle plugin and DAO code generation

```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'org.greenrobot:greendao-gradle-plugin:3.2.0'
    }
}
 
apply plugin: 'com.android.application'
apply plugin: 'org.greenrobot.greendao'
 
dependencies {
    compile 'org.greenrobot:greendao:3.2.0'
}
```

### ProGuard

```
-keepclassmembers class * extends org.greenrobot.greendao.AbstractDao {
public static java.lang.String TABLENAME;
}
-keep class **$Properties
 
# If you do not use SQLCipher:
-dontwarn org.greenrobot.greendao.database.**
# If you do not use Rx:
-dontwarn rx.**
```

## Modelling entities

[link](http://greenrobot.org/greendao/documentation/modelling-entities/)

### Schema

```
// In the build.gradle file of your app project:
android {
...
}
 
greendao {
	// 设置数据库版本号
    schemaVersion 2
}
```

### Entities and Annotations

```java
@Entity
public class User {
    @Id
    private Long id;
 
    private String name;
 
    @Transient
    private int tempUsageCount; // not persisted
 
   // getters and setters for id and user ...
}

@Entity
public class User {
    @Id(autoincrement = true)
    private Long id;
 
    @Property(nameInDb = "USERNAME")
    private String name;
 
    @NotNull
    private int repos;
 
    @Transient
    private int tempUsageCount;
 
    ...
}
```

```annotation
@Entity(
        // If you have more than one schema, you can tell greenDAO
        // to which schema an entity belongs (pick any string as a name).
        schema = "myschema",
        
        // Flag to make an entity "active": Active entities have update,
        // delete, and refresh methods.
        active = true,
        
        // Specifies the name of the table in the database.
        // By default, the name is based on the entities class name.
        nameInDb = "AWESOME_USERS",
        
        // Define indexes spanning multiple columns here.
        indexes = {
                @Index(value = "name DESC", unique = true)
        },
        
        // Flag if the DAO should create the database table (default is true).
        // Set this to false, if you have multiple entities mapping to one table,
        // or the table creation is done outside of greenDAO.
        createInDb = false,
 
        // Whether an all properties constructor should be generated.
        // A no-args constructor is always required.
        generateConstructors = true,
 
        // Whether getters and setters for properties should be generated if missing.
        generateGettersSetters = true
)
public class User {
  ...
}
```

### Primary key restrictions


```
@Id
private Long id;
 
@Index(unique = true)
private String key;
```

## Relations

[link](http://greenrobot.org/greendao/documentation/relations/)

### Modelling To-One Relations

```
@Entity
public class Order {
    @Id private Long id;
 
    private long customerId;
 
    @ToOne(joinProperty = "customerId")
    private Customer customer;
}
 
@Entity
public class Customer {
    @Id private Long id;
}
```

### Modelling To-Many Relations

```
@Entity
public class Customer {
    @Id private Long id;
 
    @ToMany(referencedJoinProperty = "customerId")
    @OrderBy("date ASC")
    private List<Order> orders;
}
 
@Entity
public class Order {
    @Id private Long id;
    private Date date;
    private long customerId;
}
```

## DaoMaster and DaoSession

[link](http://greenrobot.org/greendao/documentation/sessions/)

```java
daoMaster = new DaoMaster(db);
daoSession = daoMaster.newSession();

// Clear the identity scope
daoSession.clear();

noteDao = daoSession.getNoteDao();
noteDao.detachAll();
```

## Queries

[link](http://greenrobot.org/greendao/documentation/queries/)

### QueryBuilder

```
List joes = userDao.queryBuilder()
  .where(Properties.FirstName.eq("Joe"))
  .orderAsc(Properties.LastName)
  .list();
  
QueryBuilder qb = userDao.queryBuilder();
qb.where(Properties.FirstName.eq("Joe"),
qb.or(Properties.YearOfBirth.gt(1970),
qb.and(Properties.YearOfBirth.eq(1970), Properties.MonthOfBirth.ge(10))));
List youngJoes = qb.list();
```

### Limit, Offset, and Pagination

### Raw queries

```java
Query query = userDao.queryBuilder().where(
  new StringCondition("_ID IN " +
    "(SELECT USER_ID FROM USER_MESSAGE WHERE READ_FLAG = 0)")
).build();

Query query = userDao.queryRawCreate(
  ", GROUP G WHERE G.NAME=? AND T.GROUP_ID=G._ID", "admin"
);
```

### log

```java
QueryBuilder.LOG_SQL = true;
QueryBuilder.LOG_VALUES = true;
```

## Core Initialization

```java
// do this once, for example in your Application class
helper = new DaoMaster.DevOpenHelper(this, "notes-db", null);
db = helper.getWritableDatabase();
daoMaster = new DaoMaster(db);
daoSession = daoMaster.newSession();
// do this in your activities/fragments to get hold of a DAO
noteDao = daoSession.getNoteDao();
```

## Custom Types

[link](http://greenrobot.org/greendao/documentation/custom-types/)

## Join

[link](http://greenrobot.org/greendao/documentation/joins/)

## Encryption

[link](http://greenrobot.org/greendao/documentation/database-encryption/)
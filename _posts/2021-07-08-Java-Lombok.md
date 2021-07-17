---
layout: post
title:  "Java Lombok"
date:   2021-06-27 +0900
categories: Java Spring
---

# Lombok 정리

## Lombok??
Java Library의 일종으로, Getter,Setter 그리고 toString ... 여러가지 로직들을 따로 작성하지 않고 어노테이션으로 처리하도록 만들어진 라이브러리이다.

## 사용방법
### Original JAVA
Lombok.jar 다운로드 후  
```javac -cp lombok.jar -p lombok.jar ...```
### Maven
pom에 추가  
```XML
<dependencies>
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.20</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```
### Gradle
build.gradle에 추가  
```gradle
repositories {
	mavenCentral()
}

dependencies {
	compileOnly 'org.projectlombok:lombok:1.18.20'
	annotationProcessor 'org.projectlombok:lombok:1.18.20'
	
	testCompileOnly 'org.projectlombok:lombok:1.18.20'
	testAnnotationProcessor 'org.projectlombok:lombok:1.18.20'
}
```

## 주로 사용하는 어노테이션들
### @Getter/@Setter
클래스의 Getter/Setter를 자동으로 작성  
클래스나 해당 클래스의 속성에 추가  
```JAVA
public class notLombok {
    private int test1;
    private int test2;

    public int getTest1() {
        return test1;
    }
    public int getTest2() {
        return test2;
    }
    public void setTest1(int test1) {
        this.test1 = test1;
    }
    public void setTest2(int test2) {
        this.test2 = test2;
    }
}
```

```Java
@Getter
@Setter
public class UsingLombok {
    private int test1;
    private int test2;
}
```
### @ToString
toString() 메소드를 오버라이드해서 각각의 필드를 출력    
  
```includeFieldNames= [ true| false] (기본값 : true)```   
일반적으로 각 필드에 대한 이름도 출력하나 false시 출력하지 않는다.  

```doNotUseGetters= [ true| false] (기본값 : false)```  
필드 직접 호출, 해당 옵션이 false면 getter를 이용  
  
```callSuper= [ call| skip| warn] (기본값 : 건너 뛰기)```  
슈퍼 클래스의 필드까지 포함 여부 확인  

__Vanila Java__   
```Java
import java.util.Arrays;

public class ToStringExample {
  private static final int STATIC_VAR = 10;
  private String name;
  private Shape shape = new Square(5, 10);
  private String[] tags;
  private int id;
  
  public String getName() {
    return this.name;
  }
  
  public static class Square extends Shape {
    private final int width, height;
    
    public Square(int width, int height) {
      this.width = width;
      this.height = height;
    }
    
    @Override public String toString() {
      return "Square(super=" + super.toString() + ", width=" + this.width + ", height=" + this.height + ")";
    }
  }
  
  @Override public String toString() {
    return "ToStringExample(" + this.getName() + ", " + this.shape + ", " + Arrays.deepToString(this.tags) + ")";
  }
}
```

__Using Lombok__  
```java
import lombok.ToString;

@ToString
public class ToStringExample {
  private static final int STATIC_VAR = 10;
  private String name;
  private Shape shape = new Square(5, 10);
  private String[] tags;
  @ToString.Exclude private int id;
  
  public String getName() {
    return this.name;
  }
  
  @ToString(callSuper=true, includeFieldNames=true)
  public static class Square extends Shape {
    private final int width, height;
    
    public Square(int width, int height) {
      this.width = width;
      this.height = height;
    }
  }
}
```

### @No/@Required/@All ArgsConstructor
__@NoArgsConstructor__: 매개변수가 없는 생성자를 생성 force = true 설정시 0,false로 설정   
__@RequiredArgsConstructor__: 특별한 처리가 필요한 각 필드에 대해 1개의 매개변수가 있는 생성자를 생성 즉, final이나 @NonNull인 필드 값만 받음
__@AllArgsConstructor__: 모든 필드 값을 받음

```java
@NoArgsConstructor
@RequiredArgsConstructor
@AllArgsConstructor
public class User {
  private Long id;
  @NonNull
  private String username;
  @NonNull
  private String password;
  private int[] scores;
}
```

```java
User user1 = new User(); //No
User user2 = new User("asdf", "1234"); //Required
User user3 = new User(1L, "asdf", "1234", null); //All
```

### @Builder
생성자를 생성할 때, Builder 패턴을 이용하여 생성  
```java
import lombok.Builder;
import lombok.Singular;
import java.util.Set;

@Builder
public class BuilderExample {
  @Builder.Default private long created = System.currentTimeMillis();
  private String name;
  private int age;
  @Singular private Set<String> occupations;
}
```
  
```java
import java.util.Set;

public class BuilderExample {
  private long created;
  private String name;
  private int age;
  private Set<String> occupations;
  
  BuilderExample(String name, int age, Set<String> occupations) {
    this.name = name;
    this.age = age;
    this.occupations = occupations;
  }
  
  private static long $default$created() {
    return System.currentTimeMillis();
  }
  
  public static BuilderExampleBuilder builder() {
    return new BuilderExampleBuilder();
  }
  
  public static class BuilderExampleBuilder {
    private long created;
    private boolean created$set;
    private String name;
    private int age;
    private java.util.ArrayList<String> occupations;
    
    BuilderExampleBuilder() {
    }
    
    public BuilderExampleBuilder created(long created) {
      this.created = created;
      this.created$set = true;
      return this;
    }
    
    public BuilderExampleBuilder name(String name) {
      this.name = name;
      return this;
    }
    
    public BuilderExampleBuilder age(int age) {
      this.age = age;
      return this;
    }
    
    public BuilderExampleBuilder occupation(String occupation) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }
      
      this.occupations.add(occupation);
      return this;
    }
    
    public BuilderExampleBuilder occupations(Collection<? extends String> occupations) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }

      this.occupations.addAll(occupations);
      return this;
    }
    
    public BuilderExampleBuilder clearOccupations() {
      if (this.occupations != null) {
        this.occupations.clear();
      }
      
      return this;
    }

    public BuilderExample build() {
      // complicated switch statement to produce a compact properly sized immutable set omitted.
      Set<String> occupations = ...;
      return new BuilderExample(created$set ? created : BuilderExample.$default$created(), name, age, occupations);
    }
    
    @java.lang.Override
    public String toString() {
      return "BuilderExample.BuilderExampleBuilder(created = " + this.created + ", name = " + this.name + ", age = " + this.age + ", occupations = " + this.occupations + ")";
    }
  }
}
```



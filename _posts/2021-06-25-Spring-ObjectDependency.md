---
layout: post
title:  "토비의 스프링 정리 IoC(1)"
date:   2021-06-25 +0900
categories: Java Spring
---
# 제어의 역전(IoC)
## Object Factory
```java
public class UserDaoTest {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        ConnectionMaker connectionMaker = new MysqlConnectionMaker();
        UserDao dao = new UserDao(connectionMaker);
        //...
    }
}
```
앞 구조의 DAO를 인터페이스로 만들어 분리하는 리팩토링 하는 과정을 거쳤다.  
하지만 main을 구성하는 UserDaoTest 클래스에서 문제가 발생하는데,  
기존 UserDao가 담당하는 ConnectionMaker를 결정하는 기능을 맡게 되었다.  
하지만 UserDaoTest는 그저 작동하는지에 대한 테스트의 책임만 맡으므로 단일 책임 원칙은 위반한다고 볼 수 있다.  
__그러므로 분리해줄 필요가 있다.__ 분리될 기능은 UserDao와 ConnectionMaker 구현 클래스의 오브젝트를 만들고 연결시키도록 관계를 맺도록 한다.

---

## Factory
 분리시킬 기능을 담당할 클래스, 객체의 생성 방법을 결정하고 그렇게 만들어진 오브젝트를 반환하는 기능을 가진다.  
디자인 패턴의 Abstract Factory, Factory Method 패턴과는 다르다.  
오브젝트를 생성하는 쪽과 생성된 오브젝트를 사용하는 쪽의 역할과 책임을 깔끔하게 분리하는 목적  
  
* UserDao의 생성 책임을 맡는 팩토리 클래스
```java
public class DaoFactory {
    public UserDao mysqlUserDao() {
        ConnectionMaker connectionMaker = new MysqlConnectionmaker(); // Connectionmaker - MySqlConnectionMaker생성
        UserDao userDao = new UserDao(connectionMaker); //UserDao 생성
        return userDao;
    }
}
```
* UserDao
```java
public class UserDao {
    private ConnectionMaker connectionMaker;
    public UserDao(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }
    private final String sql1 = "insert into users(id,name,password)values(?,?,?)";
    private final String sql2 = "select * from users where id = ?";
    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection(); //사용
        PreparedStatement ps = c.prepareStatement(sql1);
        ps.setString(1,user.getId());
        ps.setString(2,user.getName());
        ps.setString(3,user.getPassword());
        ps.executeUpdate();
        ps.close();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException,SQLException {
        Connection c = connectionMaker.makeConnection();
        PreparedStatement ps = c.prepareStatement(sql2);
        ps.setString(1,id);

        ResultSet rs = ps.executeQuery();
        rs.next();
        User user = new User();
        user.setId(rs.getString("id"));
        user.setName(rs.getString("name"));
        user.setPassword(rs.getString("password"));

        rs.close();
        ps.close();
        c.close();

        return user;
    }
}
```


* UserDaoTest
```java
public class UserDaoTest {
    public static void main(String[] args) throws Exception {
        UserDao dao = new DaoFactory().mysqlUserDao(); //userDao 사용
        User user = new User();
        user.setId("whiteship2");
        user.setName("Song");
        user.setPassword("Spring");

        dao.add(user);

        System.out.println(user.getId()+" ADD Complete");

        User user2 = dao.get(user.getId());
        System.out.println(user2.getName());
        System.out.println(user2.getPassword());

        System.out.println(user2.getId()+" Find");

    }
}
```
## 설계도로서의 팩토리
client 단계에서 접근하면 client는 UserDao를 사용하고 UserDao는 ConnectionMaker를 사용한다.  
이 때 UserDao와 ConnectionMaker의 생성은 DaoFactory에서 생성한다.  
client는 DaoFactory에서 생성 요청만 하면 끝

### 장점
ConnectionMaker 구현 클래스로 변경이 필요하다면 DaoFactory를 수정해주면 된다.  
핵심 기술인 UserDao의 변경이 필요없어진다.  

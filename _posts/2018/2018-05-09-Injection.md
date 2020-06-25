---
layout  : post
title   : Field Injection vs Constructor Injection in @Autowired
summary : spring injection
date    : 2018-05-09 13:50:53 +0900
updated : 2018-05-09 14:41:38 +0900
tags    : spring
toc     : true
public  : true
---
* TOC
{:toc}

# 개요
Intelli J 의 어떤 warning 도 놓지고 싶지않아서 warning 수정중 
@Autowired 관련 Constructor을 추천한다는 사실은 인지했다.
그래서 왜 그런지 궁금하여 찾아보았다.

## 서론
Dependency injection은 spring의 핵심 & 기본 기술로 모두 인지하고 있다.
spring에서는 xml 또는 @Autowired 를 이용해 의존관계를 주입한다.


```java 
public interface Pusher {
     boolean push(String message);
}

public class ApplePusher implements Pusher {
    @Override
    public boolean push(String message) {
        // send push notification to iOS device
    }
}

public class GooglePusher implements Pusher {
    @Override
    public boolean push(String message) {
        // send push notification to Android device
    }
}
```

## Field Injection
```java
@Autowired
List<Pusher> pusher;
```

> Field injections is evil… hides dependencies, instead of making them explicit.

이러한 필드 주입 방식은 간단해 보이긴 하지만 나쁜 스타일이라 여겨진다.
class들 끼리의 coupling이 높아지고 Spring Framework에 대한 의존성이 높아진다.
이로 인해 unit testing 이 힘들어진다는 단점이 있다.

pusher 들을 사용하는 클래스를 정의해보고 테스트해보자

```java
//service
@Service
public class AdminService {
    @Autowired
    private List<Pusher> pushers;

    public doSomething() {
        // 여기서 무엇인가를 하고 pusher들을 사용해 푸시 메시지를 보내야한다고 가정합니다.

        for (Pusher pusher : pushers) {
            pusher.push("did something");
        }
    }
}
```

```java
//test
public class AdminServiceTest {
    @Test
    public void doSomethingTest() {
        AdminService adminService = new AdminService();
        adminService.doSomething();

        Assert.assertEquals(adminService.didSomething, true);
    }
}
```
위와 같은 테스트 방식으로는 테스트 대상 class의 @Autowired를 사용한 필드들이 모두 null 이므로 테스트는 실패한다.

따라서 아래와 같이 테스트들을 Spring context 안에서 돌리고 있다.
아주 간단한 유닛 테스트 임에서 Spring 설정 파일을 읽어서 모든 bean 설정이 끝나야지만 테스트가 시작하게 된다.
혹은 Mockito library를 이용해서 Autowired filed injection 을 흉내내어 테스트 할 수 있다.[http://java.dzone.com/articles/use-mockito-mock-autowired ](http://java.dzone.com/articles/use-mockito-mock-autowired)
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:/spring/spring-context.xml"})
public class AdminServiceTest extends AbstractJUnit4SpringContextTests {
  // same test cases
}
```

## constructor injection
어떤 객체가 올바로 동작하기 위해 반드시 위존관계가 필요한겨우 필드 주입방식을 사용할 이유가 전혀없다.

생성자 주입방식을 사용할 경우 올바른 의존관계가 넘어오지 않으면 compile 에러가 나므로 미리 위험요소를 없앨수 있다.
또한 테스트를 작성할 경우에도 테스트 코드 자체에서 필요한 의존관계를 만들어 넘겨줄수 있게 된다.

```java
@Service
public class AdminService {
    private List<Pusher> pushers;

    @Autowired
    public AdminService(List<Pusher> pushers) {
        // spring context안에서는 필드 주입 방식과 똑같이 알맞은 빈들이 생성되서 주입될 것입니다.
        this.pushers = pushers;
    }

    public doSomething() {
        // 여기서 무엇인가를 하고 pusher들을 사용해 푸시 메시지를 보내야한다고 가정합니다.

        for (Pusher pusher : pushers) {
            pusher.push("did something");
        }
    }
}

```

테스트에서도 pusher을 직접 만들거나 Mockito등의 library를 사용해 넘겨줄수 있으므로 원하는 방식으로 테스트 할수 있고
spring context안에서 돌리지 않아도 되므로 더빠르게 테스트를 돌릴 수 있다.

옵셔널한 의존관계일 경우 null 값을 던지던가 아니면 생성자에 올바른 기본값으로 설정해두는 방식으로 코드를 짜고
곡 필요한 의존관계의 경우 생성자에서 인자로 받는 방법을 사용하는 것을 추천합니다.

# 출처
[Field Injection vs Constructor Injection With Java Spring's @Autowired](http://crossbreeze.github.io/blog/2014/04/08/field-injection-vs-constructor-injection-with-java-springs-at-autowired/ )


# 링크
[왜 필드주입이 나쁜 코드스타일인지 포스트](http://www.petrikainulainen.net/software-development/design/why-i-changed-my-mind-about-field-injection/ )


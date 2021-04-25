<!-- @format -->

# 컴포넌트 스캔

## 컴포넌트 스캔과 의존관계 자동 주입

### 컴포넌트 스캔이란?

- `@Bean` 하나 하나 붙이는 거 너무 귀찮음
- 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공함

### 사용 방식

```java
@Configuration
@ComponentScan(
excludeFilters = @Filter(type = FilterType.ANNOTATION, classes =
Configuration.class))
public class AutoAppConfig {
}
```

- 이렇게 `@Configuration` 을 붙이고
- 아래에 `@ComponentScan` 붙이면 자동 의존 관계 주입 시작
- 이름 그대로 `@Component`라는 어노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록

### 컴포넌트 스캔 기본 대상

> 컴포넌트 스캔은 @Component뿐만 아니라 아래도 추가로 스캔 대상에 포함한다.

1. @Component : 컴포넌트 스캔에서 사용
2. @Controlller : 스프링 MVC 컨트롤러에서 사용, 스프링 MVC 컨트롤러로 인식
3. @Service : 스프링 비즈니스 로직에서 사용, 특별한 처리 안하고 그냥 개발자가 알아볼 수 있도록 도움
4. @Repository : 스프링 데이터 접근 계층에서 사용, 데이터 계층의 예외를 스프링 예외로 변환해서 서비스까지 안가게
5. @Configuration : 스프링 설정 정보에서 사용

### 의존 관계 자동 주입

- 생성자에 `@Autowired`를 지정하면 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 의존 관계를 주입한다.
- 기본 조회 전략은 타입이 같은 빈을 찾아서 주입
- getBean(Member.class)와 같은 기능을 함
- 생성자 의존 관계가 1개일 때 Autowired 안써도 자동으로 주입됨

## 중복 등록, 충돌 상황

> 컴포넌트 스캔에서 같은 빈 이름을 등록하면?

1. 자동 빈끼리 충돌하면 스프링에서 오류를 발생시킴
2. 수동 빈과 자동 빈끼리 충돌하면 수동 빈이 우선권을 가짐(수동 빈이 자동 빈을 오버라이딩)

- 이건 최근 설정에서 그냥 오류 반환하도록 바뀜

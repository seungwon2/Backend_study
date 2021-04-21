<!-- @format -->

# 스프링 컨테이너 생성

## 스프링 컨테이너란?

```java
//스프링 컨테이너 생성
ApplicationContext applicationContext =
new AnnotationConfigApplicationContext(AppConfig.class);
```

- `ApplicationContext` 를 스프링 컨테이너라 한다.
- `ApplicationContext` 는 인터페이스이다.
- `new AnnotationConfigApplicationContext(AppConfig.class)` 는 인터페이스의 구현체이다.
- 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다. (여기서는 애노테이션 기반)

## 스프링 컨테이너 생성 과정

> 💡 빈 생성 과정과 의존관계 주입하는 단계가 개념적으로 분리되어있으나 실행은 스프링이 한번에 한다.

### 1, 스프링 컨테이너 만들기

- 스프링 컨테이너를 생성할 때는 구성 정보를 지정해야한다.
- 여기서는 `AppConfig.class`를 구성 정보로 지정했다.

### 2. 스프링 빈 등록

- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.

**빈 이름**

- 빈 이름은 메소드 이름을 사용한다.
- 빈 이름을 직접 부여할 수도 있다.

### 3. 스프링 빈 의존관계 설정 - 준비

- 스프링 컨테이너는 설정 정보를 참고해서 의존 관계를 주입(DI) 한다.
- 단순히 자바 코드를 호출하는 것과 비슷해보이지만 차이가 있다.

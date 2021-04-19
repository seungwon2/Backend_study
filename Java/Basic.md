<!-- @format -->

> 🐺 자바 언어 복습 노트

## 1. enum 개념

```java
package firstProject.core.member;

public enum Grade {
    BASIC, VIP
}

enum DevType {
    MOBILE, WEB, SERVER
}
```

- 이렇게 선언해놓고 다른 클래스 파일에서 [`Grade.VIP`](http://grade.VIP) 이런 식으로 사용했다.

---

### Enum이란?

> Enum이란 Enumeration의 앞 글자로, 열거라는 의미를 갖는다

1. 클래스처럼 보이게 하는 상수
2. 서로 관련있는 상수들끼리 모아 상수들을 대표할 수 있는 이름으로 타입을 정의하는 것
3. Enum 클래스형을 기반으로 한 클래스형 선언
4. 별도의 .java로 분리 가능, 클래스 내부에서 선언도 가능, 외부에서 선언도 가능
5. 상속은 지원하지 않음
6. 클래스와 같은 문법 체계를 따름

### Enum 활용

1. 데이터의 그룹화 및 관리에 용이
2. 자바의 익명함수 인터페이스 및 커스터마이징 익명 함수를 이용하면서 활용 가능

## 2. final을 쓰는 이유는??

```java
package firstProject.core.member;

public class MemberServiceImpl implements MemberService{

    //앞에는 인터페이스, new한 후에는 구현체로 객체 생성!
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    //회원가입
    //memberRepository 안에 있는 save 메소드를 이렇게 이용
    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    //회원 검색
    //memberRepository 내부 findById 메소드를 이렇게 이용
    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

- 왜 굳이 저기서 final을 썼을까..?

---

## final 정의

final 은 오직 한 번만 할당할 수 있는 entity를 정의할 때 사용.

1. final로 선언하면 변수는 항상 같은 값을 가진다.
2. 만약 final 변수가 객체를 참조하고 있다면, 그 객체의 상태가 바뀌어도 final 변수는 매번 동일한 내용을 참조한다.

## final 사용

1. final이 붙어있는 클래스는 상속할 수 없다.
2. final이 붙어있는 메소드는 오버라이딩으로 수정할 수 없다.
3. final이 붙어있는 변수는 상수로 선언되어 수정할 수 없다.
4. final이 붙어있는 객체는 다른 객체로 변경할 수 없으나 객체 필드는 변경할 수 있다.

### final을 위 예시에서 사용한 이유

- 아마도 메모리로 사용되는 memory repository 객체니까, 계속 다른 객체로 불러와서 여러 메모리를 사용하면 안되고 한 메모리에 계속 값을 써야하니까 final을 붙인 것 같다.

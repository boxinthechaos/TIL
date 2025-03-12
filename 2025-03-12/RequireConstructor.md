# @RequiredArgsConstructor란?
Spring에서 의존성을 주입하기 위해 Constructor, Setter, Field 타입의 방식을 사용한다 하지만 `lombok`의 `@RequiredArgsConstructor` 어노테이션을 사용하면 생성자의 주입을 간단히 할 수 있다

# @RequiredArgsConstructor 사용법
이 어노테이션은 초기화 되지 않은 `field`필드나, `@NonNull`이 붙은 필드에 대해서 생성자를 생성해주는 역활을 한다 

한마디로 이 어노테이션을 사용하면 새로운 필드를 추가할때 번거롭게 생성자를 다시 만들지 않아도 된다

## 예시
간단한 예시로 알아보자
```
public class AuthService {

    private UserRespository userRespository;
    private BCryptPasswordEncoder passwordEncoder;
    private TokenProvider tokenProvider;

    public AuthController(UserRespository userRespository, BCryptPasswordEncoder passwordEncoder, TokenProvider tokenProvider){
        this.passwordEncoder = passwordEncoder;
        this.tokenProvider = tokenProvider;
        this.userRespository = userRespository;
    }
    ...
}
```
위의 예시는 `@RequiredArgsConstructor`을 사용하지 않아 생성자를 주입해준 모습이다

하지만 `@RequiredArgsConstructor`를 사용하면
```
public class AuthService {

    private final UserRespository userRespository;
    private final BCryptPasswordEncoder passwordEncoder;
    private final TokenProvider tokenProvider;
    ...
}
위의 코드같이 생성자를 직접 주입해주지 않아도 된다
```
## 사용시 주의점
이 어노테이션은 부모를 상속하는 클레스에서 사용하는 것을 권장하지 않는다 

왜냐하면 부모에서 상속받은 필드들은 자동으로 생성자를 주입해주지 않기 때문이다 
이 경우에서는 직접 생성자를 선언해서 상속 받은 필드들도 `super` 키워드로 의존성을 주입시켜야 합니다
# SPR이란?
개발하면서 SPR에 대해서 잊어버린거 같아서 잊어버리지 않기 위해서 다시한번더 공부 해본다

SPR이란 단일 책임 원칙(Single Responsibility Principle)을 의미한다 
SPR은 SOLID원칙중 한가지로 하나의 클래스는 하나의 책임을 가져야 한다는 뜻이다

다시 말하자면 컨트롤러 객층은 요청을 받아서 처리하고 서비스 객층은 비지니스 로직을 처리하는 역활을 맡아야 한다는 뜻이다 

컨트롤러가 SPR를 위반하게 된다면 변경에 따른 영향도가 매우 커지게 되므로 비지니스 로직은 서비스 객층으로 이동하여 코드의 응집력을 높이고 역활을 명확히 해주는 것이 중요하다

## 나쁜 예시

```
@RestController
@RequestMapping("/api/v1/auth")
public class AuthController {

    private UserRespository userRespository;
    private BCryptPasswordEncoder passwordEncoder;
    private TokenProvider tokenProvider;

    public AuthController(UserRespository userRespository, BCryptPasswordEncoder passwordEncoder, TokenProvider tokenProvider){
        this.passwordEncoder = passwordEncoder;
        this.tokenProvider = tokenProvider;
        this.userRespository = userRespository;
    }

    @PostMapping("/signup")
    public ResponseEntity<String> signup(@RequestBody AuthRequest authRequest){
        Optional<User> existingUser = userRespository.findByUsername(authRequest.getUsername());
        if(existingUser.isPresent()){
            return ResponseEntity.badRequest().body("이미 존재하는 아이디입니다");
        }

        String encodedPassword = passwordEncoder.encode(authRequest.getPassword());
        User user = new User(authRequest.getUsername(), encodedPassword);
        userRespository.save(user);

        return ResponseEntity.status(201).body("회원가입이 완료되었습니다");
    }
}
```

## 좋은 예시
```
@RequiredArgsConstructor
@Service
public class AuthService {

    private final UserRespository userRespository;
    private final BCryptPasswordEncoder passwordEncoder;
    private final TokenProvider tokenProvider;

    public ResponseEntity<String> signup(@RequestBody AuthRequest authRequest){
        Optional<User> existingUser = userRespository.findByUsername(authRequest.getUsername());
        if(existingUser.isPresent()){
            return ResponseEntity.badRequest().body("이미 존재하는 아이디입니다");
        }

        String encodedPassword = passwordEncoder.encode(authRequest.getPassword());
        User user = new User(authRequest.getUsername(), encodedPassword, MemberRole.ROLE_USER);
        userRespository.save(user);

        return ResponseEntity.status(201).body("회원가입이 완료되었습니다");
    }
}

----------------------
@RequiredArgsConstructor
@RestController
@RequestMapping("/api/v1/auth")
public class AuthController {

    private final AuthService authService;

    @PostMapping("/signup")
    private ResponseEntity<String> signup(@RequestBody AuthRequest authRequest){
        return authService.signup(authRequest);
    }
}
```
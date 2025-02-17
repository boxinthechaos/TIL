# JWT란?
JWT는 Json Web Token의 약자로 json객체를 이용해서 토큰 자체의 정보를 저장하고 있는 웹 토큰이다

암호화된 토큰으로 복잡하고 읽을 수 없는 string형태로 저장되어 있다

## JWT 구성요소
![alt text](image-13.png)
- 헤더:(어떤 알고리즘을 사용하여 토큰화 할 것인지) 해싱을 위한 알고리즘 정보
- 페이로드 (Payload) ( 클레임 )
    - 페이로드는 토큰에 담을 클레임(claim) 정보를 포함하고 있다
    - Payload 에 담는 정보의 한 ‘조각’ 을 클레임이라고 부른다
    - 클레임들은 name / value 의 한 쌍으로 이뤄져있다
    - 토큰에는 여러개의 클레임 들을 넣을 수 있습니다
- 시그니쳐 (서명) 토큰의 유효성을 검사하는 문자열 헤더와 페이로드를 ‘.’ 으로 합치고 헤더의 알고리즘과 비밀키로 해시값을 구하면 그게 서명이 된다

## JWT의 복호화 인증 과정
1. JWT 토큰을 클라이언트가 서버로 요청과 동시에 전달
2. 서버는 가지고 있는 개인키로 Signature를 복호화
3. 헤더의 해시값, 페이로드의 해시값이 JWT의 것과 일치한지 확인후 인증을 허용
**서버가 기존에 발급했던 JWT와 비교를 한다**

## Refresh Token
JWT를 탈취 당하면 다른 유저가 조작이 가능하기 때문에 JWT에서는 유효시간을 짧게 가지고
Refresh token을 이용한다

1. 클라이언트가 ID,PW로 서버에게 인증을 요청, 서버는 이를 확인하여 Access Token과 Refresh Token을 발급
2. 클라이언트는 Refresh Token을 본인이 잘 저장하고 Access Token으로 서버에게 자유롭게 요청
3. 서비스를 이용중 Access Token이 만료되어 사용할수 없다는 오류를 서버로 부터 전달 받음
4. 클라이언트는 Access Token의 만료 사실을 인지하고 본인이 갖고있던 Refresh Token으로 서버에게 전달하여 새로운 Access Token의 발급을 요청
5. 서버는 Refresh Token을 받아 서버의 Refresh Token Storage에 토큰의 존재 여부를 확인후 있다면 새로운 Access Token을 생성하여 전달
6. 이후 2번으로 돌아가서 동일한 작업 진행

# jjwt 라이브러리를 이용한 JWT 생성

## 의존성 설정
```
implementation 'io.jsonwebtoken:jjwt-api:0.12.3'
runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.12.3'
runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.12.3'
```
서명키 (비밀키) 설정

`private SecretKey key = Jwts.SIG.HS256.key().build();`

## Jwt 토큰 생성 
```
String jwt = Jwts.builder()                     // (1)
        
    .header()                                   // (2) optional
        .keyId("aKeyId")
        .and()
        
    .subject("Bob")                             // (3) JSON Claims, or
    //.content(aByteArray, "text/plain")        //     any byte[] content, with media type
        
    .signWith(signingKey)                       // (4) if signing, or
    //.encryptWith(key, keyAlg, encryptionAlg)  //     if encrypting
        
    .compact();                                 // (5)
```
## 페이로드의 두가지 방법
1. Content 방법
   Jwt의 페이로드는 텍스트, 이미지, 문서등 여러가지로 설정 가능하다
그럴때 사용하는것이 Content 방식이다
```
byte[] content = "Hello World".getBytes(StandardCharsets.UTF_8);

String token = Jwts.builder()
        .content(content, "text/plain")
        .signWith(key)
        .compact();
```
2. Claim 방법
   Json Object로 페이로드를 구성 하고 싶을때 사용한다
표준 클레임들이 있다
### 표준 클레임의 종류
```
String jws = Jwts.builder()
    .issuer("me")
    .subject("Bob")
    .audience("you")
    .expiration(expiration) //a java.util.Date
    .notBefore(notBefore) //a java.util.Date 
    .issuedAt(new Date()) // for example, now
    .id(UUID.randomUUID().toString()) //just an example id
```
1. iss: 토큰 발급자
2. sub: 토큰 제목
3. aud: 토큰 대상자
4. exp: 토큰 만료시간
5. nbf: 토큰 활성 날짜(이 날자 이전의 토큰은 활성화 되지 않음을 보장)
6. iat: 토큰 발급 시간
7. jti: JWT 토큰 식별자(iss 가 여러 명일 때 이를 구분하기 위한 값)<br>
json Object이기 때문에 커스텀 key/value 값을 클레임으로 넣을 수 있다

```
String jws = Jwts.builder()

    .claims(claims)
    
    // ... etc ...
```

## JWT 토큰 파싱하기 (읽기)
보통 JWS 방식을 많이 사용하므로 (Claim 방식) JWS방식만 적는다

임의의 jwt (key를 이용해) 생성
```
SecretKey key =
                Jwts.SIG.HS256.key().build();

String token = Jwts.builder()
								// userId : userId값 으로 페이로드에 넣는다.
                .claim("userId", userId)
								// 위에서 만든 키로 서명한다.
                .signWith(key)
								// 만들기 (압축한다고 한다.)
                .compact();
```
try catch문을 위해 변수 생성및 파싱
```
Jws<Claims> jwt;

try {
    jwt = Jwts.parser()
						// JWS일때는 같은 키를 사용해야 한다.
            .verifyWith(key)
						// parser를 빌드한다.
            .build()
						// 서명된 클레임(token)을 파싱한다.
            .parseSignedClaims(token);

		// 페이로드를 가져온다.
    Object payload = jwt.getPayload();
		// 페이로드를 출력한다. 출력 => "payload = {userId=1}"
    System.out.println("payload = " + payload);

// JwtException 예외를 캐치한다. (읽기에 실패한다면!, 유효하지 않다면!)
} catch (JwtException e) {
    throw new IllegalStateException("Invalid Token. " + e.getMessage());
}
```
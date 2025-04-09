# Credentialed Request의 3가지 옵션
저번 시간에 배웠 듯이 Credentialed는 총 3가지의 옵션이 있다
- same-origin(기본값) : 같은 출처 간 요청에만 인증 정보를 담는다
- include : 모든 요청에 인증 정보를 담는다
- omit : 모든 요청에 인증 정보를 담지 않는다

same-origin이나 include와 같은 옵션을 사용하여 리소스에 요청에 인증정보를 포함 하게 된다면 Access-Control-Allow-Origin만 확인하는 것이 아닌 좀더 엄격하게 검사하게 된다

서버측(`http://localhost:4000/api`)에 Access-Control-Allow-Origin이 모든 출처를 허용한다는 *로 설정이 되어 있다고 가정을 해보면 다른 출처에서 해당 서버에   리소스를 요청해도 CORS정책 위반으로 인한 제약을 받지 않는다 그렇기 때문에 `http://localhost:3000`와 같은 클라이언트 로컬 개발 환경에서도 fetch API를 사용하여 리소스를 요청하고 받아올 수 있다

구글 크롬 브라우저의 credentials 기본값은 same-origin이기 때문에, 포트 3000에서 4000으로 보내는 리소스 요청에는 브라우저의 쿠키와 같은 인증 정보가 포함되지 않는다 그렇기 때문에 브라우저는 Access-Control-Allow-Origin: * 이라는 값만 보고 해당 요청이 안전하다는 결론을 내리게 된다

하지만 credentials을 include로 변경 후 요청을 보내게 된다면
```
fetch('http://localhost:4000/api', {
  credentials: 'include', 
});
```
이번에는 동일 출처 여부와 상관없이 무조건 요청에 인증 정보가 포함되도록 설정했으므로 브라우저의 쿠키 정보가 담겨진다

하지만 서버는 이전과 동일한 응답을 보내주는 반면 브라우저는 아래와 같은 반응을 보인다
> Access to fetch at ’http://localhost:4000/api’ from origin ’http://localhost:3000’ has been blocked by CORS policy: The value of the ‘Access-Control-Allow-Origin’ header in the response must not be the wildcard ’*’ when the request’s credentials mode is ‘include’.
이는 요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 룰에 다음 두가지를 추가하여 검사하기 때문이다

이는 요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 룰에 다음 두가지를 추가하여 검사하기 때문이다

1. Access-Control-Allow-Origin에는 *를 사용할 수 없으며, 명시적인 URL이어야함
2. 응답 헤더에는 반드시 Access-Control-Allow-Credentials: true가 존재해야함

이 경우 서버측에서 CORS 설정시 허용할 origin 값을 *가 아닌 http://localhost:3000으로 지정하고, 응답 헤더에 Access-Control-Allow-Credentials: true 를 명시하여 해결할 수 있다
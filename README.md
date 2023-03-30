# web-api-starter

## JSON
### Google JSON Style Guide
> Comments: JSON 에 주석은 사용하지 않는다.  
> Double Quotes: 따옴표가 아닌 쌍따옴표를 사용한다.  
> Flattened data: 내장 객체의 경우 편의를 위한 객체 사용이 아닌 관련된 정보를 묶어서 객체로 사용한다.  
> 
> Property Name Format  
> 1.semantic 한 이름으로 사용한다.  
> 2.camel-case 를 사용한다.  
> 3.첫글자는 문자로 시작해야하며, _(underscore) 혹은 $ 를 첫글자로 사용하지 않는다.  
> 4.Javascript 예약어는 사용하지 않는다.  
> 
> Singular vs Plural Property Names  
> 기본적으로 Property name 은 단수형으로 사용한다.    
> 배열, 맵 형태인 경우 복수형으로 사용한다.  
> 
> Naming Conflicts  
> 기존에 사용하고 있는 Property 의 자료형이 다음 버전에서 변경되는 경우 `apiVersion` 프로퍼티를 통해서 구분한다.
> ex)    
> ```JSON
> // 기존 isPublic 이 'Y', 'N' 값으로 사용된 경우 
> { 
>   "apiVersion": "1.0",
>   "isPublic": "N" 
> }
> 
> 신규 버전에서 isPublic 이 boolean 으로 값이 변경되는 경우
> { 
>   "apiVersion": "2.0",
>   "isPublic": false 
> }
> ```
> 
> Property Value Format: null, boolean, number, string, object, array 만 허용  
> Enum value: Java 의 Enum value 를 JSON 에서 표현할 시 string 으로 모든 글자를 대문자로 표시  
> Date value: Date 는 RFC 3339 포맷을 따른 문자열로 표시한다.  
> ex) `{ "lastUpdate": "2023-03-14T16:33:43.000Z" }`  
> 
> Latitude/Longitude Property Values: Latitude/Longitude 는 ISO 6709 포맷을 따른 `±DD.DDDD±DDD.DDDD` 형태의 문자열로 표시한다.  
> ex) `{ "companyLocation": "+37.5386+126.9474" }`  

### 참고사이트
> [Google JSON Style Guide](https://google.github.io/styleguide/jsoncstyleguide.xml?showone=Property_Name_Format#Property_Name_Format)

---

## API 키를 통한 인증
### API 키
> API 키란 특정 사용자만 알 수 있는 일종의 문자열이다.

### 주의점
> 모든 클라이언트가 같은 API 키를 공유하기 때문에, 한번 API 키가 노출되면 전체 API가 뚫려버리는 문제가 있으므로 높은 보안 인증이 필요할 때에는 권장하지 않는다.

---

## Stateless Token 을 통한 인증
### JWT
> 특징  
> JWT는 Claim 기반이라는 방식을 사용하는데, Claim은 사용자에 대한 프로퍼티나 속성을 말한다.
>
> 장점  
> 1.이 토큰을 이용해서 요청을 받는 서버나 서비스 입장에서는 이 서비스를 호출한 사용자에 대한 추가 정보는 이미 토큰 안에 다 들어가 있기 때문에, 다른 곳에서 가져올 필요가 없다는 장점이 있다.  
> 2.토큰을 생성하는 단계에서는 생성된 토큰을 별도로 서버에서 유지할 필요가 없으며,
> 토큰을 사용하는 API 서버 입장에서는 API 요청을 검증하기 위해서 토큰을 가지고 사용자 정보를 별도로 계정 시스템 등에서 조회할 필요가 없다는 것이다.  
> 3.Claim 기반의 토큰은 토큰 자체가 정보를 담음으로써 토큰으로 서비스나 API 접근을 제어할 때, 별도의 작업이 서버에서 필요하지 않으며,
> 토큰 자체를 서버에서 관리할 필요가 없어서 구현이 상대적으로 단순해진다.
>
> 단점  
> 한번 발급된 토큰은 값을 수정하거나 폐기가 불가 : JWT는 토큰 내에 모든 정보를 다 가지고 있기 때문에 한 번 발급된 토큰에 대한 변경은 서버에서는
> 더는 불가능하다. 예를 들어서, 토큰을 잘못 발행해서 삭제하고 싶더라도 서명만 맞으면 맞는 토큰으로 인식하기 때문에 서버에서는 한번 발급된 토큰의 정보를 바꾸는 일이 불가능하다.

### Access token
> access token 은 사용자가 서버에 로그인(인증) 시 발급하는 token 이다.  
> 클라이언트에서는 access token 을 로컬에 저장하여 API 호출 시 사용한다.  
> JWT(JSON Web Token)의 단점은, 한 번 발급된 경우 유효기간이 지날 때까지 계속 사용할 수 있기 때문에 만약 토큰이 악의적으로 활용될 경우
> access token의 유효기간이 다 될 때까지 정보가 계속 털릴 수 있다.
> 그리고 이러한 JWT의 문제점을, access token의 유효기간을 줄이고, refresh token이라는 새로운 토큰을 발급함으로써 해결할 수 있다.

### Refresh token
> JWT 의 단점으로 인하여 access token 이 제 3자에게 탈취당할 경우 유효기간이 끝날 때 까지 계속 정보가 털릴 수가 있기 때문에
> access token 의 유효기간을 길게 가져가선 안된다. 하지만 그렇다고 보안을 위해서 유효기간을 너무 짧게 가져가자니 그만큼 사용자가 로그인을 자주 해서
> token 을 다시 발급받아야 하므로 여간 불편하고 귀찮은 일이 아닐 수 없다. access token 의 짧은 유효기간으로 인한 단점을 보완하기 위해 등장한 것이
> refresh token 이다.
>
> **형태**  
> refresh token 을 JWT 로 구현할 수도 있고 UUID 로 구현할 수도 있다. 각각의 특징은 다음과 같다.  
> refresh token dmf JWT 로 구현할 경우 토큰 자체에 데이터를 담을 수 있으며, token 의 유효성을 검증하기 위해 DB 에 엑세스할 필요가 없다.
> 따라서 서버의 부하가 상대적으로 적다.  
> 하지만 access token 과 마찬가지로 refresh token 을 서버에서 제어할 수 없다. 즉 refresh token 을 탈취당했을 때 해당 token 을 무효화 시킬 방법이 없다.
>
> refresh token 을 UUID 혹은 random string 으로 구현할 경우 그 토큰을 사용자와 맵핑 되도록 DB 저장해야한다.
> 이런 경우 refresh token 유효성 검증시 DB 에 엑세스가 필요하다. 하지만 비정상 동작이 의심되는 사용자를 강제로 로그아웃 시키거나 차단이 가능해진다.
> 또한 JWT 에서는 불가능한 refresh token 탈취기 해당 token 을 즉시 무효화 시킬 수 있다.
>
> **한계**  
> refresh token 으로 인하여 access token 의 유효기간을 짧게 가져갈 수 있지만, 탈취된 access token 이 유효한 그 짧은 시간 동안에 악용되는 것을 막을 수는 없다.  
> 또한 refresh token 을 탈취 당한 경우 마음껏 access token 을 발행할 수 있기 때문에 refresh token 을 탈취당하는 것은 access token 을 탈취당하는 것과
> 동일하다고 봐야한다.  
> 다만 refresh token 을 DB 에 저장할 시에는 access token 과 다르게 서버에서 즉시 무효화를 할 수 있다는 점이 있다.  
> 클라이언트에 저장하는 순간부터 access token 및 refresh token 모두 탈취가능성이 생긴다.
> 따라서 클라이언트는 refresh token 이 탈취되지 않도록 안전하게 보관해야한다.
>
> **의견**  
> 사견으로는 refresh token 을 DB 에 저장하지 않고 JWT 로 관리하는 것은 그저 access token 을 하나 더 만드는 것이라고 생각한다.
> refresh token 의 용도는 access token 의 보조 역할이며 필요에 따라서는 서버에서 무효화를 할 수 있어야 access token 을 보완할 수 있다고 생각든다.

### 참조사이트
> [rest-api-보안-및-인증-방식](https://dongwooklee96.github.io/post/2021/03/28/rest-api-%EB%B3%B4%EC%95%88-%EB%B0%8F-%EC%9D%B8%EC%A6%9D-%EB%B0%A9%EC%8B%9D.html)
> [Access Token의 문제점과 Refresh Token](https://hudi.blog/refresh-token/)

---

## Json Web Token(JWT)
### 개요
> JWT 는 유저를 인증하고 식별하기 위한 토큰(Token)기반 인증이다. RFC 7519 에 자세한 명세가 나와있다.  
> JWT 를 사용하면 RESTful 과 같은 무상태(Stateless)인 환경에서 사용자 데이터를 주고 받을 수 있게된다.  
> JWT는 Header, Payload, Signature 로 구성된다. 또한 각 요소는 . 으로 구분된다.  

### Header
> JWT 에서 사용할 타입과 해시 알고리즘 종류가 담겨있다.   
> 타입은 "JWT" 로 고정이다.  
> 알고리즘은 보통 "HS256" 혹은 "RSA" 가 사용된다.  

### Payload
> 서버에서 첨부한 사용자 권한 정보 및 데이터가 담겨있다.   
> 데이터는 Claims 에 저장한다. 

### Signature
> 개인키로 서명한 전자서명이 담겨있다. 
> 비대칭키를 통해서 개인키로 서명하게 되면 공개키를 통해 검증을 거치게 된다. 
> 서명은 헤더의 인코딩값과, 정보의 인코딩값을 합친후 주어진 비밀키로 해쉬를 하여 생성합니다.

### Claims
> **등록된 클레임(Reserved claims)**  
> 이미 예약된 Claim으로 필수는 아니지만 사용하길 권장된다.  
> `iss`: 발급자(issuer)  
> `sub`: 제목(subject)  
> `aud`: 대상자(audience)  
> `exp`: 만료시간(expiration), 밀리초 형식  
> `iat`: 발급시간(issued at), 밀리초 형식    
> `jti`: 고유 식별자, 중복처리 방지를 위해 사용되며 일회용 토큰에 사용하면 유용.  
> `nbf`: Not Before. 활성 날짜. 현재 시간이 nbf 로 지정한 시간을 지나지 않으면 유효하지 않음을 설정, 밀리초 형식    
> 
> **공개 클레임(Public claims)**  
> 공개 클레임들은 충돌이 방지된 (collision-resistant) 이름을 가지고 있어야 한다. 
> 충돌을 방지하기 위해서는, 클레임 이름을 URI 형식으로 짓는다.  
> ex: `{ "https://velopert.com/jwt_claims/is_admin": true }`  
> 
> **비공개 클레임(Private claims)**  
> 등록된 클레임도아니고, 공개된 클레임들도 아니다. 
> 양 측간에 (보통 클라이언트 <->서버) 협의하에 사용되는 클레임 이름들이다. 
> 공개 클레임과는 달리 이름이 중복되어 충돌이 될 수 있으니 사용할 때에 유의해야합니다.

### 참조사이트
> [JWT(JSON Web Token)의 개념부터 구현까지 알아보기](https://pronist.dev/143)
> [JWT(JSON Web Token) - 김종근](https://velog.io/@sproutt/JWTJSON-Web-Token-%EA%B9%80%EC%A2%85%EA%B7%BC)
> [[JWT] JSON Web Token 소개 및 구조](https://velopert.com/2389)

---

## CORS
### Cross-Origin Resource Sharing
> 교차 출처 리소스 공유  
> Cross 는 다른 출처라고 해석한다.  
> Origin 은 출처를 의미하며 출처란 protocol(https/http), host(www.google.com 등), port(443, 80 등) 을 합친 것이다.  
> 출처를 비교하는 로직은 서버에 구현되어 있는 것이 아닌 웹 브라우저(클라이언트)에 구현되어 있는 스펙이다.  

### SOP  
> Same-Origin Policy 는 말 그대로 같은 출처에서만 리소스를 공유할 수 있다는 규칙을 가진 정책이다.

### CORS 동작
> 브라우저는 요청 헤더에 `Origin` 이라는 필드에 요청을 보내는 출처를 함께 담아보낸다.  
> 이후 서버가 이 요청에 대한 응답을 할 때 응답 헤더의 `Access-Control-Allow-Origin` 이라는 값에 "이 리소스를 접근하는 것이 허용된 출처"를 내려주고,
> 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin 과 서버가 보내준 응답의 Access-Control-Allow-Origin 을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다. 

### 서버 셋팅
> 서버에서는 `Access-Control-Allow-Origin` 의 값을 설정한다.  
> 만약 서버가 모든 사용자에게 오픈된 API 를 제공하는 경우 `Access-Control-Allow-Origin` 의 값은 `*` 로 모든 출처를 허용하는 와일드카드를 설정한다.  
> 만약 서버가 특정 도메인에서만 사용되는 API 인 경우 `Access-Control-Allow-Origin` 의 값은 특정 도메인과 와일드카드(*)를 적절히 섞어서 설정한다.  
> 안드로이드의 경우 웹 앱을 통한 서버 호출인 경우가 아니라면 CORS 를 확인하지 않기 때문에 CORS 설정이 무의미할 수 있다.     

---

## OAuth 2.0
### 특징
> TODO

---

## GraphQL
### 특징
> TODO

---

## gRPC
### TODO
> TODO

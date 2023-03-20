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

## gRPC
### TODO
> TODO

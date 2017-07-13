(中文版请戳这:[中文版](https://github.com/GrayLand119/API-Security-Checklist/blob/master/README-zh.md))
(中文版请戳这:[中文版](https://github.com/GrayLand119/API-Security-Checklist/blob/master/README-zh.md))

# API 보안 체크리스트
API를 설계, 디자인, 테스트 및 출시할 때 가장 중요한 보안 대책에 대한 점검 목록 입니다.

------------------------------------------------------------------------------
## 인증 (Authentication)
- [ ] `Basic Auth` 를 사용하지말고 표준 인증을 사용해라 (e.g JWT, OAuth)
- [ ] `인증`, `토큰 생성`, `비밀번호 저장` 방법을 만들지말고 표준을 사용해라

### JWT (JSON Web Token)
- [ ] 랜덤 복합키 (`JWT Secret`) 를 사용해서 브루트 포싱이 어려운 토큰을 만들어라
- [ ] payload 에서 알고리즘을 추출하지 말고, 백엔드에서 알고리즘을 강제로 적용해라 (`HS256` 또는 `RS256`)
- [ ] 토큰의 만료기간을 가능한 짧게 설정해라 (`TTL`, `RTTL`)
- [ ] JWT payload는 [쉽게](https://jwt.io/#debugger-io) 복호화가 가능하므로 민감한 데이터는 저장하지마라

### OAuth
- [ ] 항상 `redirect_uri` 을 서버 사이드에서 허용된 URL 인지 검사해라
- [ ] 토큰 대신 항상 코드를 주고 받아라 ( `response_type=token` 을 허용하지 마라 )
- [ ] OAuth 인증 처리과정에서 CSRF를 방지하기 위해 랜덤 해쉬값을 가진 `state` 매개변수를 사용해라
- [ ] 기본 스코프를 정의하고, 각 어플리케이션에서 스코프 매개변수 유효성을 검사해라

## 접근 (Access)
- [ ] DDOS / 브루트포스 공격을 방지하기 위해 요청(Throttling)을 제한해라
- [ ] MITM (Man In The Middle Attack)을 방지하기위해 서버사이드에서 HTTPS 를 사용해라
- [ ] SSL Strip 공격을 방지하기 위해 `HSTS` 헤더를 SSL과 함께 사용해라

## 입력 (Input)
- [ ] 작업에 따라 알맞은 HTTP 메소드를 사용해라. `GET(조회, read)`, `POST(생성, create)`, `PUT(대체/갱신, replace/update)`, `DELETE (삭제, to delete a record)`.
- [ ] request 헤더에서 `content-type` 가 허용하는 포맷 (e,g `application/xml`, `application/json` ... etc) 인지 검사하고, 일치하지 않으면 `406 NOT Aceeptable` 을 반환해라
- [ ] 요청받은 데이터의 `content-type` 을 검사해라 (e.g `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` ... 등)
- [ ] 공통 취약점을 방지하기 위해 사용자의 입력값을 검사해라 (e.g `XSS`, `SQL-Injection`, `Remote Code Execution`... 등)
- [ ] URL에서 민감한 데이터 (`credentials`, `password`, `security tokens`, `API Keys`)를 포함하지말고, 이러한 것들은 표준 인증 방식의 헤더를 사용해라

## 처리 (Processing)
- [ ] 잘못된 인증을 방지하기 위해 모든 엔드포인트 뒤에 인증 프로세스가 보호하고 있는지 검사해라
- [ ] 사용자의 리소스 식별자를 사용하지마라. `/user/654321/orders` 대신 `/me/orders` 를 사용해라
- [ ] 자동 증가(auto increment) 식별자 대신 `UUID` 를 사용해라
- [ ] 만약 XML 파일을 파싱할경우, `XXE` (XML external entity attack) 를 피하기 위해 엔티티 파싱을 비활성화 해라
- [ ] 만약 XML 파일을 파싱할경우, `Billion Laughs/XML bomb` (기하 급수적인 엔티티 확장 공격) 을 방지하기 위해 엔티티 확장을 비활성화 해라
- [ ] 파일 업로드에는 CDN 을 사용해라
- [ ] 엄청난 양의 데이터를 처리할 때는 HTTP Blocking 을 방지하기 위해 Workers 와 Queue 를 사용해서 빨리 응답해라
- [ ] DEBUG 모드를 끄는것을 잊지 마라

## 출력 (Output)
- [ ] `X-Content-Type-Options: nosniff` 헤더를 전송해라
- [ ] `X-Frame-Options: deny` 헤더를 전송해라
- [ ] `Content-Security-Plicy: default-sc 'none'` 헤더를 전송해라
- [ ] fingerprinting 헤더를 제거해라 - `X-Powered-By`, `Server`, `X-AspNet-Version` 등
- [ ] 반드시 응답값에 `content-type` 를 포함해라
- [ ] `credentials`, `password`,`security tokens` 와 같은 민감한 정보는 반환하지마라
- [ ] 완료된 작업에 따라 알맞은 응답 코드를 반환해라 (`200 OK` `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` 등)


------------------------------------------------------------------------------

# Contribution
Feel free to contribute , fork -> edit -> submit pull request. For any questions drop us an email at team@shieldfy.io.

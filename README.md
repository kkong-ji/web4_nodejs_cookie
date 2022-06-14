# web4_nodejs_cookie
생활코딩 강의를 들으며 학습한 nodejs에서의 cookie와 Auth

<br>

## 웹이 등장한 이후, 개인화의 필요성 대두
> 💡 **개인화란?** <br>
> 모든 사람에게 똑같은 웹 페이지를 보여주는 것이 아닌 <br>
> 사람마다 그 사람의 선택과 취향에 맞는 웹 페이지를 보여주는 것 <br>

ex. 고객마다 다른 장바구니의 물건, 로그인 이후 재로그인 없이 웹 페이지를 이용

<br>

## Cookie의 도입
- 1994년. 넷스케이프 네비게이터 사의 엔지니어 루먼 톨리에 의해 고안됨.
- 이전에 접속했던 사용자의 정보를 웹 서버로 전송할 수 있게 됨.
- **사용자 식별**이 가능해짐

<br>

## Cookie 개요
- HTTP 프로토콜에 포함된 기술
- 쿠키의 주요한 3가지 기술
  - 세션 관리
  - 개인화
  - 트래킹

<br>

## Nodejs에서 cookie 생성 방법 예시

```
var http = require('http');
http.createServer(function(request, response) {
    response.writeHead(200, {
      'Set-Cookie' : ['yummy_cookie=choco', 'tasty_cookie=strawberry']
    });
    response.end('Cookie!!');
}).listen(3000);
```

<br>

## Npm cookie 모듈을 통해 객체로 파싱하기

### Installation
`npm install cookie`

### API
`var cookie = require('cookie');`

### 사용예시

```
var http = require('http');
http.createServer(function(request, response) {
    var cookies = cookie.parse(request.headers.cookie);
    console.log(cookies);
    response.writeHead(200, {
      'Set-Cookie' : ['yummy_cookie=choco', 'tasty_cookie=strawberry']
    });
    response.end('Cookie!!');
}).listen(3000);
```

<br> 

## session 쿠키 vs permanent 쿠키
- **session 쿠키**: 웹 브라우저를 껏다 키면 세션 정보가 사라짐.
- **permanent 쿠키** : 웹 브라우저를 껏다 켜도 세션 정보가 사라지지 않음.
- 위 사용 예시의 코드가 session 쿠키를 구현한 것

<br>

### permanent 쿠키

### 사용예시

```
var http = require('http');
http.createServer(function(request, response) {
    var cookies = cookie.parse(request.headers.cookie);
    console.log(cookies);
    response.writeHead(200, {
      'Set-Cookie' : ['yummy_cookie=choco', 
                      'tasty_cookie=strawberry',
                      `Permanent=cookies;
                      Max-Age=${60*60*24*30}` // 30일 이후 만료되는 쿠키 
       ]
    });
    response.end('Cookie!!');
}).listen(3000);
```

<br>

## Secure & HttpOnly
### Secure
- `Session id` 를 이용한 공격을 방지
- Https를 사용하는 경우에만 웹 브라우저가 웹 서버에게 데이터를 전송
- `Secure=Secure; Secure` 와 같이 사용

<br>

### HttpOnly
- 웹 브라우저가 웹 서버와 통신할 때만 쿠키를 허용
- `Session id` 와 같은 보안이슈를 불러일으킬 수 있는 정보는 <br>
  자바스크립트를 통해서 접근을 할 수 없게 만드는 옵션
- `HttpOnly=HttpOnly; HttpOnly` 와 같이 사용

<br>

## Path & Domain
- 쿠키를 제어하는 방법 중 하나

<br>

### Path
: 쿠키가 어떤 path에서만 동작할 것인가를 지정

<br>

### 예시

```
var http = require('http');
http.createServer(function(request, response) {
    var cookies = cookie.parse(request.headers.cookie);
    console.log(cookies);
    response.writeHead(200, {
      'Set-Cookie' : ['yummy_cookie=choco', 
                      'tasty_cookie=strawberry',
                      `Permanent=cookies;
                      Max-Age=${60*60*24*30}`,
                      'Path=Path; Path=/cookie'
       ]
    });
    response.end('Cookie!!');
}).listen(3000);
```

<br>

- 위와 같이 디렉토리를 지정하면 해당 디렉토리와 그 하위에 해당하는 쿠키만<br>
  서버에 전송

<br>

### domain
: 쿠키가 어떤 path에서만 동작할 것인가를 지정

<br>

### 예시

```
var http = require('http');
http.createServer(function(request, response) {
    var cookies = cookie.parse(request.headers.cookie);
    console.log(cookies);
    response.writeHead(200, {
      'Set-Cookie' : ['yummy_cookie=choco', 
                      'tasty_cookie=strawberry',
                      `Permanent=cookies;
                      Max-Age=${60*60*24*30}`,
                      'Path=Path; Path=/cookie',
                      'Domain=Domain; Domain=o2.org'
       ]
    });
    response.end('Cookie!!');
}).listen(3000);
```

<br>

- 위 코드의 경우 `o2.org/3000` or 서브 도메인(`test.o2.org/3000`) 에서만 쿠키가 살아있게 된다.

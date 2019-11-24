---
title: HTTP
date: 2019-11-10 18:11:51
category: tech
---

> CS50 Harvard - HTTP

## Client - Server 

Client is browser!

## How www.facebook.com 

### what happens when you type 'google.com' into a brower? 

> https://dev.to/antonfrattaroli/what-happens-when-you-type-googlecom-into-a-browser-and-press-enter-39g8



### why many websites use www as prefix? 

Wold-wide-web 임을 알려주는 것이다. 

### http://www.facebook.com

http는 프로토콜이다. 

\<Request>

```http
GET / HTTP/1.1
Host: www.facebook.com
...
```

`/`: default identifier

\<Response>

```http
HTTP/1.1 200 OK 
Content-Type: text/html
...
```

#### 다양한 HTTP 응답 

> https://developer.mozilla.org/ko/docs/Web/HTTP/Status

`20` : OK! 

`404` : Not Found

`301` : Moved Permanently

### TCP/IP 

> IP v4 vs. v6 
>
> Ipv6 주소는 기존의 Ipv4 주소 체계를 128비트로 확장한 IP 주소이다. 

#### TCP

TCP가 담당하는 것은 어디까지나 서버가 송신할 때와 서버가 수신한 후 애플리케이션에게 전달할 때로, 상대 서버까지 전송하는 부분은 하위 계층인 IP에 모두 위임한다. 

:smile: TCP의 역할 

- `포트 번호`를 이용한 데이터 전송 

  : 상대 서버에 데이터가 도착해도 어떤 애플리케이션에게 전달해야할 데이터인지 알 수 없기 때문에 포트 번호를 명시한다. 

- 연결 생성 

  : 3-way handshaking 

- 데이터 보증과 재전송 제어 

  : ACK 가 오지 않으면 재전송하는 방식 

  - 일정 시간 내에 ACK가 돌아오지 않으면 재전송한다. (타임아웃)
  - 중복 ACK : 특정 세그먼트가 늦게 도착할 수도 있으므로 몇 번 이상 중복 ACK가 오면 재전송한다. 

- 흐름 제어와 폭주 제어

  - 슬라이딩 윈도우  

#### IP

IP의 역할은 지정한 대상 서버까지 전달받은 데이터를 전해주는 것이다. 하지만 IP에서는 반드시 전달된다는 것을 보장하지 않는다. 

:smile: IP의 역할 

- IP주소를 이용해서 최종 목적지에 데이터 전송 
- 라우팅(routing)



#### TLS (Transport Layer Security)

인터넷에서 정보를 암호화해서 송수신하는 SSL(Secure Sockets Layer)에 기반한 기술이다. 서로의 신원을 확인하기 위해 핸드쉐이크 과정을 거친다. 

##### TLS Handshake 

![the TLS Handshake](https://www.cloudflare.com/resources/images/slt3lc6tev37/5aYOr5erfyNBq20X5djTco/3c859532c91f25d961b2884bf521c1eb/tls-ssl-handshake.png)



#### DNS? (Application layer)

> http://www.itworld.co.kr/news/108921

호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행할 수 있도록 개발되었다. 

`www.example.com` 같은 컴퓨터의 도메인 이름을 `192.168.1.0` 과 같은 IP주소로 변환하고 라우팅 정보를 제공하는 분산형 데이터베이스 시스템이다. 

```bash
$ nslookup google.com
Server:		172.20.10.1
Address:	172.20.10.1#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.161.78
```



#### 서브넷 마스크? 

IP 주소 체계의 Network ID와 Host ID를 서브넷 마스크를 통해 변경하여 네트워크 영역을 분리 또는 합치는 것이다. 

네트워크를 분리하는 것을 서브넷팅이라고 부르고, 합치는 것을 슈퍼넷팅이라고 한다. 



#### DHCP? (Dynamic Host Configuration Protocol)

동적으로 IP 주소를 획득할 수 있다. 

![img](https://t1.daumcdn.net/cfile/tistory/2519193C5715E03427)

클라이언트는 시스템이 시작되면 DHCP 서버에 IP주소를 요청한다. DHCP 서버로부터 IP주소를 부여 받으면 TCP/IP 통신을 할 수 있다. 

:smile: 장점 

1. ip관리가 편하다. DHCP 서버에서 ip를 자동적으로 할당해주므로 IP 충돌을 예방할 수 있다. 
2. 네트워크 연결 설정 시간 단축이 가능하다. 

:disappointed: 단점 

DHCP서버가 다운되면 ip할당이 안되므로 인터넷 사용이 불가능하다. 



#### CDN? (Content Delivery Network)

규모가 큰 정적 데이터를 전송하기 위해 DB 서버 외에 사용하는 데이터 전송 전용 서버이다. 대부분의 웹 시스템은 CDN을 이용하고 있다. 이는 웹 시스템 특징인 '하나의 시스템을 수많은 사용자가 이용한다', '대량의 데이터를 참조하는 업무가 많다'는 것에 기인한다. 

CDN은 대량의 데이터 전송에 특화된 것으로, 전 세계에 있는 데이터 복사본(캐시)를 배치하는 기술과 병렬 기술을 활용하여 처리를 효율화하고 있다. 

### HTML 

```html
<!DOCTYPE html>  
<html>					This says, Hey browser! that is it for the webpage
  <head>
    
  </head>
  <body>
    
  </body>
</html>				  This says, Hey browser! that is end for the webpage
```

:thinking: `<!DOCTYPE html>` 을 써야하는 이유가 뭘까? (Document Type Declartion)

> 참고: [What is the role of Doctype](https://www.quora.com/What-is-the-role-of-Doctype-HTML-in-making-an-HTML-page)
>
> html 문서에서 \<html> tag 전에 무조건 선언해야 한다. `<!DOCTYPE>` 은 웹브라우저에게 HTML의 버전을 알려준다.  
>
> `<!DOCTYPE html>` 은 HTML5를 말한다. 다른 방식을 선언하는 방식은 [여기](https://ko.wikipedia.org/wiki/문서_형식_선언) 를 참고. HTML5로 구성된 문서에서 문서 형식 선언은 불필요하지만, 웹 브라우저들의 표준 모드를 활성화하기 위해 최소한의 형태로 유지되었다. 

HTML의 symetric한 형태 덕분에 HTML을 트리 구조로 나타낼 수 있다. 브라우저는 HTML 문서를 트리 형태로 바꾸고 렌더링한다. 



### 쿠키와 세션 

HTTP가 Connectionless/Stateless 프로토콜이기 때문에 쿠키와 세션이 필요하다. 

#### 세션 

세션은 일정 시간 동안 같은 브라우저로부터 들어오는 요청을 하나의 상태로 보고 그 상태를 유지하는 기술이다. 이러한 세션이 존재하지 않으면 사용자는 항상 인증을 해야하는 불편함이 생긴다. 

1. 클라이언트가 서버에 접속하면 세션 ID를 발급한다. 
2. 서버는 클라이언트가 발급한 세션 ID를 쿠키에 저장한다. 
3. 클라이언트는 다시 접속할 때, 이 쿠키를 이용하여 세션 ID값을 서버에 전달한다. 

*쿠키,세션은 캐시와 다르다* 

> 캐시는 웹페이지가 빠르게 렌더링 할 수 있도록 도와주는 것으로 오디오, 이미지 파일 등을 클라이언트단에 저장한다. 

### HTTPS

TLS를 사용해 암호화된 연결을 하는 HTTP를 HTTPS라고 하며 443번 포트를 사용한다. 



### REST Api

> https://meetup.toast.com/posts/92

Representational State Transfer 

2000년도 로이 필딩이 최초로 소개했으며 HTTP의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로 REST를 발표한다. 

즉, REST는 HTTP기반으로 필요한 자원에 접근하는 방식을 정해놓은 아키텍처이다. 다음과 같은 구성을 가진다. 

1. 자원 - Resource (URI)
2. 행위 - Verbe (HTTP METHOD)
3. 표현 - Representation (Content-Type)

#### REST의 특징 

#### REST API 설계 규칙 

:star2: URI는 정보의 자원을 표현해야 한다. 

:star2: 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다. 

> URI와 URL의 차이점 
>
> `Uniform Resource Identifier` vs. `Uniform Resource Locator` 
>
> - URI는 고유하게 정보 리소스를 식별하고 위치를 지정한다. 
> - URL의 URI의 일종으로 특정 서버의 한 리소스에 대한 구체적인 위치를 서술한다. 


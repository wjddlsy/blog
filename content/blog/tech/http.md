---
title: HTTP
date: 2019-11-10 18:11:51
category: tech
---

> CS50 Harvard - HTTP

## Client - Server 

Client is browser!

## How www.facebook.com 

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

IP는 데이터가 목적지에 갈 수있도록 해준다. 

TCP는 데이터가 목적지에 도착하는 것을 보장한다. 

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


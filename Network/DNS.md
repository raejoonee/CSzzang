## DNS란?

DNS는 Domain Name System의 약자로, 호스트의 도메인 네임을 네트워크 주소로, 네트워크 주소를 도메인 네임으로 변환해주는 시스템이다.

그렇다면 도메인 네임은 무엇이고 네트워크 주소는 무엇인가 ..

<br>

## IP 주소

IP 주소란 특정한 TCP/IP 네트워크에 있는 호스트(컴퓨터, 프린터, 라우터와 같은 장치)를 고유하게 식별하는 32비트 숫자를 말한다. 즉, 각 **호스트에게 부여된 고유 주소**이다. 어디서 본 것 같은 이 숫자 → `192.168.123.132` ← 가 바로 IP 주소이다. 이렇게 4개의 10진수 숫자가 점으로 분리된 형태를 가진다. 이 IP 주소에서 앞의 세 숫자가 **네트워크 주소**이며, 마지막 하나가 **호스트 주소**이다. ***(이렇게 점으로 분리된 하나의 부분? 단위? 를 옥텟이라고 한다.)***

```markdown
IP 주소 - 192.168.123.132

192.168.123. : 네트워크 주소 / .132 : 호스트 주소
또는 
192.168.123.0 : 네트워크 주소 / 0.0.0.132 : 호스트 주소
```

<br>

## Domain Name

이러한 IP 주소는 사용자가 기억하기 어렵다. [naver.com](http://naver.com)에 접속하고자 하는데 매번 `210.89.164.90` 을 타이핑할 순 없는 노릇 .. 😇 그래서 등장한 것이 도메인이다. **IP 주소에 naver.com 이라는 이름을 부여하는 것과 같다.** 호스트에게 부여된 고유 주소가 IP이고, 이 주소에 부여된 이름이 도메인이므로 **호스트를 식별하기 위한 이름**이라고 생각해도 될 듯 하다. 

참고로 naver.com에서 com은 top-level domain, naver.com은 second-level domain이다. 그리고 우리가 알고있는 주소는 마지막 `.`이 생략된 형태이다. 사실은 `naver.com.`이라고 함.. **여기서 마지막 점이 바로 Root**

<br>

## Name Server

이렇게 IP ↔ Domain 간 변환을 수행하는, 즉 DNS를 제공하는 서버를 **네임 서버**라고 한다. 

<br>

<img width="897" alt="스크린샷 2020-10-03 오후 6 57 20" src="https://user-images.githubusercontent.com/37361629/94988844-28622800-05ab-11eb-9e77-6bdd29b99853.png">

<br>

사용자가 naver.com 을 입력하면 Local DNS에게 이 도메인에 대응하는 IP 주소를 물어본다. (이 Local DNS는 각 PC마다 가지고 있다.) 이 Local DNS가 해당하는 주소를 가지고 있지 않다면 이 주소를 알아내야 한다.

먼저 Root DNS에게 주소를 물어본다. Root DNS는 이 주소를 가지고 있지 않기 때문에 다른 서버에게 물어보라고 응답을 한다. ***(→ TLD가 com이구나? 그럼 걔를 관리하는 서버한테 ㄱ ㄱ)*** 그러면 top-level domain을 관리하는 서버에게 물어보아야 한다.

com 도메인, 즉 TLD를 관리하는 서버에게 주소를 물어보았을 때도 역시나 해당하는 주소를 가지고 있지 않다. 그러므로 한 번 더, 나는 이 주소가 없으니 다른 서버한테 물어보라고 응답을 한다. ***(→ naver.com을 관리하는 서버 알려줄게 거기로 ㄱ ㄱ)***

이번에는 naver.com 도메인을 관리하는 서버에게 주소를 물어본다. 이제 해당하는 IP 주소를 얻어올 수 있다. Local DNS는 이제 naver.com의 IP 주소를 알게 되었으므로 이 주소를 저장해둔다.(캐싱!) 이 다음부터는 사용자가 naver.com을 입력했을 경우 여기저기 거치지 않아도 바로 주소를 얻을 수 있다. 

이 상황에서 사용자가 google.com에 접속하고자 한다면 (Local에 이 주소가 없다는 가정 하에) Local DNS는 IP를 얻기 위해 어디로 갈까? 

root가 아닌 `TLD`로 간다. **naver.com에 대한 정보를 알고 있다는 뜻은 .com에 대한 정보도 알고 있다는 뜻이다.** 그러므로 root로 가지 않고 com을 관리하는 서버로 가서 google.com의 주소를 물어본다. 

<br>

IP 주소와 Domain name을 한 서버에 저장하기에는 양이 너무(x10000) 많다. 또한 한 서버에서 관리할 경우 이 서버가 다운된다면 ..? ㅇㅇ 큰일남. 그래서 이렇게 분산된, 계층적인 구조를 가진다고 한다. 

<br>
<br>

## Reference

[https://opentutorials.org/course/228/1450](https://opentutorials.org/course/228/1450)

[https://www.netmanias.com/ko/post/blog/5353/dns/dns-basic-operation](https://www.netmanias.com/ko/post/blog/5353/dns/dns-basic-operation)

[https://velog.io/@raejoonee/OSI-참조-모델-완전히-파헤치기](https://velog.io/@raejoonee/OSI-%EC%B0%B8%EC%A1%B0-%EB%AA%A8%EB%8D%B8-%EC%99%84%EC%A0%84%ED%9E%88-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)

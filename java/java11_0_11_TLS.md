회사 업무 중, 로컬에서 서버를 띄우는데 DataSource를 생성하는 단계에서 아래와 같은 에러가 났다. 

> Caused by: javax.net.ssl.SSLHandshakeException: No appropriate protocol (protocol is disabled or cipher suites are inappropriate)


내부망 DB에 접속을 시도하는데 적절한 프로토콜을 찾지 못한다는 이유로 실패하는 것이다. 

<br> 

원인은 간단(했지만 꽤 헤맸다..)했는데, 바로 **Java 버전**이었다. <br>
문제의 버전은 **JDK 11.0.11**이다.

해당 버전의 [Release Note](https://mail.openjdk.java.net/pipermail/jdk-updates-dev/2021-April/005860.html)를 확인해보면 이와 같은 내용이 있다. 

```
JDK-8256490: Disable TLS 1.0 and 1.1
====================================
TLS 1.0 and 1.1 are versions of the TLS protocol that are no longer
considered secure and have been superseded by more secure and modern
versions (TLS 1.2 and 1.3).

These versions have now been disabled by default. If you encounter
issues, you can, at your own risk, re-enable the versions by removing
"TLSv1" and/or "TLSv1.1" from the `jdk.tls.disabledAlgorithms`
security property in the `java.security` configuration file.
```

참고로 11.0.10은 괜찮다. 


 <br> 


Java 버전을 바꾸지 않고도 해결할 수 있는 방법이 있긴하다.   <br> 
DB 접속 URL에 아래와 같은 옵션을 추가하면 된다. 

> useSSL=false

 <br> 


---

생각해보면 이전에 DataGrip으로 같은 DB 서버에 접속하려 했을때도 같은 경험을 한적이 있었다. (동일한 JDK를 사용했으니..) <br> 
Test Connection 시, 위와 같은 에러 문구가 뜨며 접속에 실패했는데 그 때는 DataGrip에서 제공하는 속성값을 변경하여 해결했었다.

> Advanced > enbledTLSProtocols에 아래 내용 추가 

```
TLSv1, TLSv1.1, TLSv1.2, TLSv1.3
```
TLSv1.3은 없어도 된다.









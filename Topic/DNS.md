# IP 주소

```
192.0.26.68
```

이처럼 32비트(IPv4의 경우)로 이루어진 숫자들이다

* x.y.z.w 값을 가지며 한 칸당 8비트를 가진다
    * 0~255까지 가질 수 있다

<br>

이때 모든 장치(노트북, 스마트폰 등)은 숫자를 사용하여 서로를 찾고 통신한다

* 근데 브라우저로 이동할 때 이동하는 방법은 두 가지가 있다
    * 도메인을 입력하는 방법 : www.naver.com
    * 아이피를 입력하는 방법 : 123.456.789.0

<br>

위 두 가지 방법으로 접속을 하는데 이때 도메인이란 사람이 읽을 수 있는 이름을 IP 대신 사용하는 것이다.

* 123.456.789.0 -> www.naver.com
* 마치 전화번호부 같은 기능을 한다
* `쿼리` : DNS 서버는 이름을 IP 주소로 변환하여 도메인 이름을 웹 브라우저에 입력할 때 최종 사용자를 어떤 서버에 연결할 것인지 제어한다

<br>

# DNS를 사용하는 이유

> DNS가 없으면 이 숫자를 외워야 한다.

* IPv4 : 192.168.1.1
* IPv6 : 2400:cb00:2048:1::c629:d7a2

# DNS란?

> IP를 대신하여 온라인 정보에 액세스할 수 있게 해주는 것

* 웹 브라우저는 인터넷 프로토콜(IP)를 통해 상호작용 한다.
* DNS는 브라우저가 인터넷 자원을 가져올 수 있도록 도메인 이름을 IP 주소로 변환한다

# 작동 방법

```java

@RequiredArgsConstroctor
public class HowToOperateDNS {

  public static void main(String[] args) {
    WebBrowser webBrowser;
    ResolverServer resolverServer;
    String domain = "www.naver.com";

    DnsResolver dnsResolver = new DnsResolver();
    Ip ip = dnsResolver.requestDnsService(domain);
  }
}

public class DnsResolver {
  private DnsRootNameServer dnsRootNameServer;
  private TopLevelDomainNameServer topLevelDomainNameServer;
  private NameServer nameServer;

  public Ip requestDnsService(String domain) {
    getTopLevelDomainServer(domain);
    getNameServer(domain);
    return requestIp(domain);
  }

  private void getTopLevelDomainServer(String domain) {
    this.topLevelDomainNameServer = dnsRootNameServer.getTopLevelDomain(domain);
  }

  private void getNameServer(String domain) {
    this.nameServer = topLevelDomainNameServer.getNameServer(domain);
  }

  private Ip requestIp(String domain) {
    return nameServer.getIP(domain);
  }
}

public class DnsRootNameServer {
  public String getTopLevelDomain(String domain) {
    return ".com";
  }

}

public class TopLevelDomainNameServer {
  public getNameServer(String domain) {
    return new AmazonRoute53NameServer();
  }
}

public class NameServer {
  public Ip getIP(String domain) {
    return new Ip("192.168.0.1");
  }
}

class AmazonRoute53NameServer extends NameServer {

}
```
위 방식으로 사용자가 DNS를 이용하여 도메인을 IP로 변환하면 이를 사용하여 서버와 통신한다

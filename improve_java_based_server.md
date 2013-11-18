---
layout: bootstrap
title: 자바기반 서버 개선안
---
# 자바기반 서버 개선안

저는 SI를 위한 통합 보안시스템의 관리서버를 개발했습니다.
관리서버는 다양한 보안시스템의 이벤트를 수신하여 전파하고,
운영자들에게 자원을 분배하는 역할을 담당합니다.
자바로 작성했고 OSGi를 제외하고 특별한 프레임워크를 사용하지 않았습니다.
시스템을 구성하는 다양한 보안시스템들과 메시지큐를 사용하여 통신을 하고,
정보는 RDBMS에 저장했습니다.

다른 일을 담당하게 된 후 그동안 개발해온 관리서버에 대한
아쉬움과 개선할 부분을 기록으로 남겨봅니다.

## 통신

기존에는 메시지큐를 사용했고, 제가 관리서버 개발에서
빠지게 직전부터 RESTful 웹서비스로 이전을 시작하였습니다.
메시지큐와 웹서비스 중 어떤 것을 선택할지 물어보면 웹서비스 손을 들겠지만,
현재로는 한가지 단점이 있습니다.
서드파티 입장에서 메시지큐 특유의 라이브러리 대신
HTTP를 사용하는 웹서비스가 확실히 연동하기 쉬운게 사실이지만,
비동기 이벤트를 HTTP로 송수신하기위한 방법(WebSocket, Server-Sent Events,
STOMP, WebRTC, XMPP BOSH 등)이 아직 대중적이지 않다고 생각합니다.
이미 지원하는 메시지큐도 있지만, JMS 2.0의 표준참조구현인 Open MQ의 경우
2013년 연말에 예정된 5.0 버전부터 WebSocket을 지원할 예정입니다.

메시지 본문을 XML로 정하고, 메시지큐와 웹서비스 둘 모두
동일한 메시지를 통신하는 방법도 가능합니다.
메시지큐를 계속 사용한다면 구글 protobuf로 프로토콜 처리부를 변경하고 싶습니다.
다른 메시지큐의 성능과 안정성을 테스트하여 다른 메시지큐를 찾아보고 싶지만,
서드파티의 레거시 시스템들을 변경할 수 없는 점이 제약사항입니다.

## 모니터링과 테스트

처음 설계할 때는 없었지만 시스템을 운영하면서 개발자가 필요해서
그때 그때 시스템 현황을 보여주는 웹페이지를 더해갔습니다.
처음부터 이렇게 만들 줄 알았다면 Nagios나 Munin 같은 도구를 고려했겠지만,
PHP, Python, RRDtool, cron, SQLite 등으로 간단히 구현했습니다.
Yammer에서 공개한 Metrics 같은 라이브러리를 사용하여
체계적으로 운영 지표를 수집하고 싶습니다.

또, 실시간 시스템 모니터링 도구가 있었으면 좋겠습니다.
Graphite 같은 도구를 사용할 수 있지만,
Netflix에서 공개한 Hysterix 라이브러리도 좋아보입니다.
Hysterix는 화려한 대시보드에 추가로 시스템 과부하시 유연하게 상황을 처리하여
도미노식 장애를 피할 수 있습니다 (회로 차단기 디자인 패턴 참고).
그동안 시스템 사용량이 크지는 않지만 가끔 네트워크 장애가 발생할 때
부하가 한곳에 집중하여 다른 구성요소로 장애가 번지는 현상을 경험하였습니다.

여러 구성요소로 이루어진 시스템에 장애가 발생하면 원인을 찾아서 조치하기 힘듭니다.
개발자라도 다른 사람이 담당한 부분의 문제를 파악하기 힘든 경우가 흔합니다.
흔한 장애 여부를 파악하고 각 구성요소를 자가테스트하는 트러블슈팅 웹페이지를
고려했지만 오랫동안 할일 목록에 남아있었습니다.

부끄럽지만 단위테스트는 물론이고 소프트웨어 테스트를 고려하지 않고 개발하였습니다.
Spring DM 기부(포기) 이후 Spring과 OSGi의 관계가 밝아보이지 않지만,
테스트 편이를 위해 Spring 프레임워크를 적용하고 싶습니다.
또, Cucumber를 사용하여 요구사항에 기반한 BDD를 경험해보고 싶습니다.
Netflix의 Chaos Monkey 같이 구성요소를 무작위로 죽여서 시스템 회복력을
검사하는 방법이 멋져보이지만, 현재 서버 설계로는 실효가 없을 것 같습니다.

## 프레임워크

OSGi를 사용했지만 장점을 느끼지 못했습니다.
오히려 여러 라이브러리와 프레임워크를 적용하는 과정에서
클래스 로딩 문제 때문에 팀원들이 불필요하게 고생했습니다.
저의 OSGi에 대한 이해와 활용이 부족했기 때문이라고 생각합니다.
JRebel로 얻을 수 있음직한, 실행중 코드 변경 등은 전혀 시도해보지 못했습니다.
단, OSGi 보다 Guice (혹은 Spring) 같은 경량 컨테이너를 활용하는 대안은 어떨까 싶습니다.

## 기타

시스템의 병목점을 찾아야 할 상황에 몰리지는 않았지만
프로파일링을 통해 비효율적인 부분을 향상하고 싶었습니다.
Eclipse TPTP 프로젝트가 사라진 이후 쓸만한
오픈소스 자바 프로파일링 도구를 찾지 못했습니다.

마지막으로 개발팀의 선택 비용을 고려하지 않는다면
Scala (Akka)와 Node.js도 시도해보고 싶습니다.

## 문서 이력

* 2013-04-08: 작성
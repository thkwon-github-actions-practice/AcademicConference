## 7. 아키텍처 특성 범위

소프트웨어 아키텍처 세계에서는 전통적으로 아키텍처 특성의 범위를 시스템 레벨에 두는 것을 당연시했다.

10년전 모놀리식에서 마이크로서비스 등의 아키텍처 스타일이 가능해지면서 아키텍처 특성의 범위는 상당히 좁아졌다.

*소프트웨어 개발 생태계가 무서운 속도로 진화하면서 당연시 여겼던 공식들이 퇴화하고 있다.*

아키텍트는 운영 아키텍처 특성을 따져보고 아키텍처 특성에 영향을 미치는 코드베이스 외부의 컴포넌트를 잘 살펴봐야 한다.  

그래서 이런 종류의 의존성을 측정하기 위해 `아키텍처 퀀텀`이라는 용어를 정의했다.

이 용어를 이해하기 위해선 커네이선스라는 핵심 메트릭을 알아야 한다.

### 7.1 커네이선스  

두 컴포넌트 중 한쪽이 변경될 경우 다른 쪽도 변경해야 전체 시스템의 정합성이 맞는다면 이 들은 커네이선스를 갖고 있는 것이다.

*저번에도 생각했지만 의존성 역전의 원칙과 비슷한 결인지,, 디커플링과 비슷한 개념으로 소개된 적 있었다.*

예를 들어 마이크로 서비스 아키텍처의 두 서비스가 address라는 동일한 클래스를 공유한다면 두 서비스는 서로 정적인 커네이선스를 가진다고 볼 수 있다.  

다시 말해 address라는 공유 클래스를 변경하면 두 서비스 모두 변경해야 한다.

동적 커네이선스는 동기, 비동기 두 종류가 있다..  

분산 서비스끼리 동기 호출을 하면 호출부는 피호출부의 응답을 기다려야 하는 반면, 이벤트 기반 아키텍처의 비동기 호출은 파이어 앤드 포켓 방식이므로 운영 아키텍처에서 두 서비스는 개별적으로 작동시킬 수 있다.

### 7.2 아키텍처 퀀텀과 세분도

소프트웨어를 서로 엮는 것은 컴포넌트 레벨의 커플링만이 아니다.  

많은 비즈니스 개념이 의미상 여러 시스템 파트를 엮어 기능적으로 응집되어 있다..

성공적으로 소프트웨어를 설계, 분석, 발전시키기 위헤서 개발자는 문제가 될 만한 지점을 모두 살펴봐야 한다.

- 아키텍처 퀀텀
  - 높은 기능 응집도와 동기적 커네이선스를 가진, 독립적으로 배포 가능한 아티팩트
- 독립적으로 배포 가능
  - 아키텍처 퀀텀은 아키텍처의 다른 파트와 독립적으로 작동되는 모든 필수 컴포넌트를 포함한다.
- 높은 기능 응집도
  - 응집도는 컴포넌트 설계에 따라 구현된 코드가 얼마나 목적에 맞게 통합되어 있는지를 나타낸다.
- 동기적 커네이선스
  - 동기적 커네이선스는 아키텍처 퀀텀을 형성하는 애플리케이션 콘텍스트 내부 또는 분산 서비스간의 동기 호출을 의미한다.

아키텍처 퀀텀 개념은 아키텍처 특성의 새로운 범위를 제시한다. 현대 시스템에서 아키텍트는 시스템 레벨보다는 퀀텀 레벨의 아키텍처 특성을 정의한다.  

### 느낀점

커네이선스에 대해서 조금 어려웠는데 다시 한번 다루니까 개념이 확실히 잡힌 느낌..!

#### 논의사항
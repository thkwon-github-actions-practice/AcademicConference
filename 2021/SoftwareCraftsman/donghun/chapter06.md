## 6장. 동작하는 소프트웨어
이번 장은 소프트웨어가 왜 실패하는지에 대한 이야기부터 시작한다.
소프트웨어가 실패하는 원인은 정말 다양하다고 한다.
하지만, 그 중 소프트웨어 자체의 품질 즉, 코드의 품질에 대해서는 간과하는 경우가 많다고 한다.
단순히 잘 작성된 문서와 절차 그리고 관리자만 있으면 개발자는 공장 노동자 마냥 그 위해서 코드를 찍어내면 된다고 생각한다.
이번 장을 통해 왜 단순히 동작하는 소프트웨어만으로는 부족한지 알아보자.

### 자신이 만든 소프트웨어에 인질이 되는 상황
정말 큰 문제라고 생각한다.
자신이 만든 소프트웨어에 특정 기능을 추가하는 데 많은 시간을 들여야 한다면,
기존 코드를 건드리는 것이 두렵다면,
이것은 소프트웨어가 사업을 망치는 지름길과 같아 보인다.
사업의 관점에서 보면 빠르게 변화하는 시간의 요구에 대응하는 기능을 최대한 빠른 시일 내에 선보이는 것이 중요할텐데 저품질의 코드로 만들어진 소프트웨어라 빠르게 기능 추가가 어렵다고 한다면 이것이야 말로 자신이 만든 소프트웨어에 인질이 되는 상황인 것 같다.
따라서 코드의 품질이 사업에 있어서도 중요하고 이 중요성을 사업 담당자에게도 이야기할 기회가 있다면 꼭 이야기하고 싶다.

### 시간에 대한 잘못된 인식
> 단위 테스트 구현도 기술적 부채 백로그에 기록하겠습니다. 하지만 당장은 기능 구현을 끝내야 합니다.
위 대화 내용은 내가 이야기 했던 내용 그대로라라 너무 소름 돋았고 이것이 잘못된 생각이라는 것을 깨달았다.
우선 이런 테스트 코드를 작성하는 것 자체를 앞으로 해야할 일로 남겨 놓는 것 자체가 잘못된 생각이였다.
이렇게 남겨 놓으면 언젠가 할 것이라는 생각이 잘못되었기 때문이다.

그리고 사례를 통해 테스트 코드를 짜는 것에 대한 당위성을 다시 한번 느낄 수 있었다.
테스트 코드가 없는 상태에서 겪게 되는 무수한 디버깅 과정이 얼마나 시간 낭비인지를 깨달았다. 
만약 해당 기능에 테스트 코드가 있었다면? 
테스트 코드 위에서 해당 기능에서 확장을 하는 과정에서 버그가 발견 됐다면?
아마 곧 바로 어느 부분이 잘못되었는지 알 수 있었을 것이다.

그래서 기능을 구현하기 전에 오히려 테스트 코드를 먼저 짜는 것이 해당 기능을 완벽하게 구현하는 첫 걸음이라는 것을 깨달았다.

세삼 TDD가 얼마나 위대한 기술적 실행 관례인지 깨닫게 되었다.

### 레거시 코드
아마 누구나 동감할 것이다.
코딩을 하면서 가장 고통스러운 순간은 남이 짠 코드를 보는 것이 아닐까?
무튼 현재 맡고 있는 제품이 딱 이렇다.
정말 테스트 코드를 짜기 위해 객체를 생성하는 일부터가 난관이고 
하나를 테스트 하기 위해서 정말 많은 부분들이 얽혀 있어서 어디서부터 테스트를 해야 할지 잘 모르겠다.
그럼에도 한번 도전해보고 싶단 생각이 든다.
정말 작고 별 것 아닌 기능부터 테스트 코드를 짜는 것을 도전해보고 싶은 마음이다.
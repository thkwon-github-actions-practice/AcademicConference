# 3장  기본 도구

## 서문

장인에게 있어 도구는 땔래야 땔 수 없는 도구 그 이상의 무엇인 것 같고, 손에 딱 맞는 나만의 아이템인 것 같다. 처음부터 사용한 도구와 함께 하면서 이런저런 도구들도 사용을 하겠지만 그럼에도 가장 처음 사용했던 도구가 가장 익숙한 것은 사실일 것이다.

다만, 익숙함에 머물러 있으면 안 되겠다는 생각이 든다.

도구를 사용할 때 좀 더 좋은 방법이 없을까를 고민해 봐야겠다.

단순히 한 가지 도구를 쓰면 안 된다는 생각보다 현재 사용하고 있는 도구를 더 잘 사용하기 위해서는 어떻게 해야 할지 와 더 좋은 방법은 없는지를 고민해 봐야겠다.

그리고 해당 도구를 벗어나서 또 어떤 도구들이 있는지, 그 도구가 가지고 있는 장점은 또 무엇이 있는지 수시로 확인을 해보고 언제든 갈아탈 수 있다는 생각을 가져야겠다.

Q) 그동안 즐겨 사용하던 도구에서 새로운 도구로 갈아탔던 기억이 있을까요? 있었다면 어떤 이유로 갈아탔는지 공유해 주시면 감사하겠습니다.

## Topic 16 일반 텍스트의 힘

전 직장에서 있었던 일화가 요번 챕터를 읽으면서 기억이 났다.

유저가 앱을 사용하면서 수정한 설정 내용을 config.json 파일로 저장해 보는 것이 어떻겠냐란 조언을 들었었다. 당시 내가 알고 있던 json 포맷의 사용 이유는 그저 서버와 데이터를 주고받기 위함 그 이상도 이하도 아니었기 때문에 왜 굳이 이렇게 해야 하나란 생각이 가득했었다.

하지만 일단은 해보자란 생각으로 달려들어서 정말 허접한 config를 만들고 유저가 앱 세팅 화면을 얼었을 때 불러오는 로직으로 간단히 구현을 했었다.

책에서는 일반 텍스트로 데이터를 저장하면 앱보다 훨씬 오래 살아남을 수 있어서 좋다는 이야기가 나오는데 확실히 데이터가 앱에 종속적이지 않으면 다양한 환경, 상황에는 사용이 가능한 것 같다.

만일 위 일화에서 사용자가 저장한 설정을 유니티로 만들 앱이 아닌 안드로이드 또는 웹에서 사용을 한다고 했을 때 해당 config 파일만 공유를 하면 되는 것처럼 말이다.

더 나아가 “데이터는 단순히 특정 변수에서 살아 *숨 쉬는* 것은 아니다”라고 사고를 확장할 수 있게 된 것 같다.

## Topic 17 셀 가지고 놀기

비전공 개발자인 나에게 어쩌면 가장 높은 진입 장벽은 shell 이었다.

온통 검정 화면에 커서만 깜박깜박 거리고 있는 화면을 보고 있으면 여기서 대체 무엇을 해야 하는 걸까란 생각에 마냥 무서웠다.

저자가 이야기하는 Shell에 대한 애정은 이미 멘토님을 통해서도 동일하게 느꼈다.

당시 나는 git을 GUI를 통해서만 사용을 하고 있었다. 그러다 멘토링을 하던 도중 command line으로 브랜치를 생성하고 check out을 한 다음 staging에 올리고 commit, push, pull request까지 오직 shell만 사용해서 끝내는 모습을 보고 속으로 우와를 외쳤다.

먼가 진짜 개발자스럽다고 해야 할까?

아무튼 그 후로 조금씩 shell를 사용해 보고는 있다.

확실한 건 예전에 gui만 사용했던 때 보다 더 다양한 기능을 사용하고 있는 것이 사실이고 특히 어떤 버그의 경우는 command 입력으로만 해결이 가능한 경우가 있어서 더욱이 shell에 익숙해질 필요성을 느끼고 있다.

그리고 끝으로 요즘 batch 파일을 만들어서 생산성을 높이는 재미도 찾고 있다.

안드로이드 빌드 후 생성한 aab(스토어에 올리기 위해서는 aab로 뽑아내야 하기 때문에)를 곧바로 apks로 변경해서 각자 자리에 배포하는 것을 테스트하고 있는데 이때도 batch를 적극적으로 활용해서 자동화에 박차를 가하는 중이다.

## Topic 18 파워 에디팅

> 에디터를 유창하게 쓸 수 있게 하라
> 

다른 건 몰라도 진짜 IDE는 어떻게 사용하느냐에 따라 생산성이 극과 극인 것 같다.

Visual Studio 2019에서 특정 소스 파일을 찾고 싶을 때 프로젝트 폴더 리스트 위에 있는 검색을 통해 찾을 수 있지만 “ctrl + shift + t”를 통해 검색하면 조금 더 스마트하게 검색이 가능하다.

또한 여러 줄을 동시에 수정하고, 특정 단어를 검색 후 수정하는 등 일일이 찾는 것보다 시간을 많이 아낄 수 있었다.

다만 책에서 제안한 챌린지는 쉽지 않아 보인다.. 마우스 없이 오직 타이핑으로 수정을 하는 건 좀..

하지만 이렇게 쓰다 보면 확실히 그동안 몰랐던 단축키를 배우고 또 숙지할 수 있을 것 같다.

Q) 각자 즐겨 사용하는 에디터에서 공유해 주고 싶은 꿀 팁이 있을까요??

## Topic 19 버전 관리

여러 개발자와 협업을 하면서 가장 많이 배운 것이 요 버전 관리 프로그램이 아닐까 싶다.

가장 많이 사용하고 또 편했던 툴은 git + git hub desktop인데 딱 협업을 하기 위한 기능은 정도는 제공을 해주고 있고 가볍고 필수적으로 필요한 기능은 모두 제공해 주고 있어서 즐겨 사용하고 있다.

개인적으로 작업 관리를 위해 jira와 git hub를 연동했던 기능이 참 좋았다.

PR 타이틀에 jira 카드에 있는 number(?)를 붙이기만 해도 해당 jira 카드에 PR이 연동되는 신기한 경험을 하면서 작업 관리의 편리함을 새삼 느낄 수 있었다.

다만, git bub에도 칸반 기능을 제공을 해주고 있어서 작업 관리를 꼭 jira로 해야 하는지는 아직도 의문이긴 하다.

## Topic 20 디버깅

> 당황하지 말라.
> 

이것이 디버깅에 있어서 가장 중요한 1원칙이라고 한다.

이게 왜 1원칙인지 진짜 너무 동감이 된다.

막상 버그를 만나면 이건 불가능해란 생각이 머릿속에 둥둥 떠다니기 때문이다.

그래서 저자도 단호박으로 이야기를 해주는 것 같다. 이런 쓸데없는 생각에 시간 낭비하지 말고 일단 디버깅부터 시작해 보라고.

그리고 한 가지 조언이 더 눈에 들어온다.

> 디버깅할 때 근시안의 함정에 주의하라.
> 

본능적으로 에러가 뜬 라인부터 디버깅을 찍긴 하지만 일단 왜 이런 버그가 나왔을지 생각을 해보는 건 좋은 습관인 것 같다.

그리고 이진 분할 탐색을 활용한 팁도 등장을 하는데 이벤트 방식으로 구현을 많이 하다 보니 스택만 쫓아서 디버깅을 하기는 쉽지 않을 것 같다.

## Topic 21 텍스트 처리

텍스트 잘 다뤄서 자동화 프로그램을 만들어본 경험이 있어서 그런지 어느 정도 동감이 되었다.

당시 만든 자동화 프로그램은 protobuf 포맷으로 만들어진 데이터 모델링에 따라 해당 데이터를 처리하는 로직을 자동을 만들어주는 프로그램이었는데, 사용자 입장에서 데이터만 추가하고 해당 프로그램을 돌리면 해당 데이터를 처리하는 로직이 담겨 있는 스크립트 파일을 생성해 주고 사용 중인 프로젝트에 복사 붙여넣기까지 가능하도록 했기 때문에 아주 유용하게 사용했었다.

텍스트를 잘 활용하면 자동화하는데 도움이 되는 것은 사실이다.

## Topic 22 엔지니어링 일지

태형님이 애정 하는 방식이라서 더욱 눈에 들어왔던 챕터였다.

새롭게 알게 된 사실을 정리해서 모아놓은 공간이 있지만 일지는 아직 한 번도 써보진 않았다.

확실히 그 당시 느꼈던 생각, 감정, 인사이트 등을 잘 정리해 놓으면 후에 비슷한 상황에 놓였을 때 많은 도움을 얻을 수 있을 것으로 보인다.
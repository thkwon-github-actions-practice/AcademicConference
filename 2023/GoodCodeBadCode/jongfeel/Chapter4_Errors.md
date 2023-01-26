## 4장 오류

모든 것이 잘못될 수 있고 잘못될 것이기 때문에 
오류 사례를 생각하지 않고 견고하고 신뢰성 높은 코드를 작성할 수 없다.

오류에 대해 생각할 때
소프트웨어가 작동을 계속할 수 있는 오류와
작동을 계속할 합리적인 방법이 없는 오류로 구분하는 것이 유용할 때가 많다.

코드가 오류를 어떻게 전달하고 처리해야 하는지에 관해 의견이 분분하다.

<pre>
논의내용)
4.5절에서 호출하는 쪽에서 예외 복구 시 비검사 예외 vs 명시적 기법에 대한 재밌는 내용이 있다.
이것에 대해서 다들 하나씩 <b>자신이 생각하는 건 어느 쪽 주장을 따르는지 얘기</b>해 보자 (혹은 여기에 속하지 않는 주장도 상관 없음)
</pre>

### 4.1 복구 가능성

특정 오류가 발생할 경우 복구할 수 있는 현실적인 방법이 있는지 생각해야 하는 경우

#### 4.1.1 복구 가능한 오류

오류가 나서 시스템이 작동을 멈추면 훌륭한 사용자 경험은 아니다.
잘못된 사용자 입력에 대해 오류 메시지를 제공하고 올바른 입력을 요청하는 편이 더 낫다.

추가로 두 가지 복구 가능한 오류의 예는 아래와 같다.

- 네트워크 오류: 네트워크를 통해 서비스에 연결할 수 없는 경우라면, 네트워크 연결 확인하도록 요청
- 중요하지 않은 작업 오류: 통계 기록에서 오류가 발생해도 실행을 계속 진행

일반적으로 시스템 외부의 오류에 대해 시스템 전체가 표나지 않게 적절하게 처리해야 한다.

#### 4.1.2 복구할 수 없는 오류

개발자가 코드의 어느 부분에서 뭔가를 망쳐 놓은 것으로 프로그래밍 오류 문제가 발생한다.
오류를 복구할 수 있는 방법이 없다면, 피해를 최소화하고 개발자가 문제를 발견하고 해결할 가능성을 최대화 하는 것이다.

```
의견)
초보 개발자는 알 수 없는 오류나 문제는 컴퓨터 자체의 문제로 치부하고
자신도 확신할 수 없는 코드를 작성해 놓고 기도 메타를 통한 미신으로 믿고 싶은 경우가 많은데
나는 전적으로 문제의 원인과 시작은 개발자의 잘못 99% 라고 보는 입장이고 여기 나온 내용에도 동의하는 편이다.
나머지 1%는 천재지변의 경우의 수를 남겨둔 것이다. 카카오 데이터 센터 화재 같은 경우이다.  
```

#### 4.1.3 호출하는 쪽에서만 오류 복구 가능 여부를 알 때가 많다

간결한 추상화 계층을 만들고자 한다면 코드의 잠재적 호출자에 대한 가정을 하지 않는 것이 좋다.
함수를 작성하거나 수정하는 시점에 오류로부터 복구할 수 있는지 여부를 항상 알 수 있는 것은 아니기 때문이다.

아래 상황 중 하나라도 해당되면, 함수에 제공된 값으로 인해 발생하는 오류는 호출하는 쪽에서 복구하는 것으로 간주해야 한다.

- 함수가 어디서 호출될지, 호출 시 제공되는 값이 어디서 올지 정확한 지식이 없다
- 코드가 미래에 재사용될 가능성이 희박하다. 재사용이 된다면 코드가 어디에서 호출되고 값이 어디에서 오는지 가정이 의미가 없어진다

여기서도 코드가 어떻게 사용하는지 자신을 알수 있어도 다른 사람에게는 분명하지 않을 수 있다.
그러므로 특정 입력이 유효하지 않다는 사실이 코드 계약의 세부 조항에 숨겨져 있다면 다른 개발자는 이를 놓칠 가능성이 크다.

#### 4.1.4 호출하는 쪽에서 복구하고자 하는 오류에 대해 인지하도록 하라

호출하는 쪽에서 유효한 값만 사용할 것이라고 예상하는 것은 합리적이지 않다.
함수 작성자는 이 함수에서 오류가 발생할 수 있다는 가능성을 호출하는 쪽에서 확실하게 인지하도록 해야 한다.
그렇지 않으면 오류를 처리하는 코드를 작성하지 않을 수 있고 그 상태에서 오류가 발생해 버리면 예상과 다른 결과가 초래된다.

### 4.2 견고성 vs 실패

오류 실패, 더 높은 코드 계층이 오류를 처리하게 하거나 프로그램의 작동을 멈추게 한다
vs
오류를 처리하고 계속 진행

오류가 있어도 처리하고 계속 진행하면 견고한 코드라고 볼 수 있지만
오류가 감지되지 않고 이상한 일이 발생하기 시작한다는 의미도 될 수 있다.

#### 4.2.1 신속하게 실패하라

신속하게 실패하기failing fast는 문제의 실제 발생 지점으로부터 가까운 곳에서 오류를 나타내는 것이다.
복구할 수 있는 오류라면 복구할 수 있는 기회를 최대한 제공하고
복구할 수 없는 오류라면 개발자가 문제를 파악하고 해결할 수 있는 기회를 제공한다.

#### 4.2.2 요란하게 실패하라(Failing loudly)

버그가 있다는 사실을 알지 못하면 고칠 수 있는 방법이 없다.

요란한 실패는 오류가 발생하는데도 불구하고 아무도 모르는 상황을 막고자 하는 것이다.
이를 위한 명백한 방법은 예외를 발생시켜 프로그램이 중단되게 하는 것이다.
오류 메시지를 기록하는 방법도 있는데 개발자가 얼마나 부지런하게 로그를 확인하는지 로그에 방해되는 메시지가 얼마나 있는지에 따라 오류 메시지가 무시될 수 있다.

실패할 때 신속하고, 요란하게 오류를 나타내면 개발이나 테스트 도중에 버그가 발견될 가능성이 크다.
그렇지 않더라도 배포 이후에 오류 보고를 보기 시작하고 그 내용으로 부터 버그가 발생한 위치를 알 수 있는 이점이 있다.  

#### 4.2.3 복구 가능성의 범위

소프트웨어는 견고하게 작성하는 것이 좋고, 한 번의 잘못된 요청으로 전체 서버의 동작이 멈추는 것은 바람직하지 않다.
오류를 모른채 시스템이 동작하게 만드는 것도 잘못된 것이기에 코드가 요란하게 실패해야 한다.

가장 요란하게 실패하는 것은 프로그램을 멈추도록 하는 것이지만, 소프트웨어를 견고하지 못하게 만드는 문제가 있다.

해결책으로 오류가 발견되면 오류를 기록하고 모니터링 하는 것이다.
오류 정보를 기록해서 디버그할 수 있게 해주고, 오류 발생률이 높아지면 개발자에게 알림 메시지를 보내게 한다.

모든 유형의 오류를 기록하는 건 주의해야 하는데, 오류 전달 대신 기록만 하면 오류가 숨겨져서 문제가 발생한다.

#### 4.2.4 오류를 숨기지 않음

오류가 적절히 기록되거나 보고되지 않으면 개발팀이 문제를 인식하지 못할 수 있다.

떄로는 실수를 숨기고 실수가 없던 것 처럼 코드를 작성할 수 있는데
오류를 숨기는 것은 복구가능하거나 불가능한 오류 모두 문제를 일으킨다.

- 호출하는 쪽에서 오류를 숨기면 복구할 수 있는 기회를 없애는 것이다. 잘못이 일어난 걸 알지 못하면 소프트웨어가 의도한 대로 동작하지 않을 가능성이 크다.
- 복구할 수 없는 오류를 숨기면 프로그래밍 오류가 감춰진다. 이런 오류들은 개발팀이 알아야 고칠 수 있다. 그러면 버그는 꽤 오랫동안 방치되어 있게 된다.
- 두 경우 모두 예측한 대로 코드가 실행되지 않는 케이스이다. 실제 코드가 제대로 동작을 못하고 잘못된 정보를 출력하거나 데이터를 손상시키다가 작동이 멈출 수 있다.

**기본값 반환**

함수가 오류가 발생하고 값을 반환할 수 없다면 기본값을 쓰는게 간단하고 쉬운 해결책 처럼 보일 때가 있다.
기본값의 문제점은 오류가 발생했다는 사실을 숨긴다는 것인데,
이는 코드를 호출하는 쪽에서 모든 것이 정상인 것처럼 계속 진행하다는 것을 의미한다.

기본값이 유용한 경우가 있을 수 있지만, 대부분의 경우 적합하지 않다.
이런 방식은 신속한 실패와 요란한 실패의 원리를 위반하는 것이다.

```
의견)
OMG!
이 부분에서 많은 생각을 하게 됐는데
이 부분을 이해하기 전 까지는 기본값을 사용하는 걸 즐겨했고 그게 문제를 발생시키지 않으면서 프로그램이 잘 동작하게 만드는 방법이라고 생각했기 때문이다.
기본값을 반환하는 부분은 저자의 생각과 완전 반대였었는데, 향후 프로그래밍을 하게 될 때 기본값을 쓰는 것에 대해 조금 더 생각하게 될 것 같다.
다음에 언급하는 널 객체 패턴과 대치되는 상황이 아닌가? 라는 생각을 미처 하기도 전에 널 객체 패턴이 디자인 패턴에서 양날의 검이라는 얘기를 하는 걸 봐서 저자의 주장에 조금 더 깊이 있는 이해를 할 수 있는 계기가 됐다. 
```

**널 객체 패턴**

널 객체는 실제 반환값처럼 보이지만 멤버 함수는 아무것도 하지 않거나 의미없는 기본값을 반환한다.
마찬가지로 널 객체 패턴역시 오류 처리에 사용하는 것은 바람직하지 않다.

```
의견)
어쩌면 싱글톤 패턴에 이어 두 번째 안티 패턴이 될 가능성이 있어 보인다.
추가로 널 객체 패턴을 책에 많이 언급하고 주장하고 있는 로버트 C 마틴과 한번 대화해 보면 재밌을 것 같다는 생각도 든다.
물론 관점의 차이가 다르다는 건 이해해야 한다.
톰 롱: 예외처리 vs 널 객체
로버트 C 마틴: 널 vs 널 객체 
```

**아무것도 하지 않음**

코드가 오류 신호도 없이 어떤 작업이 수행되었다면
개발자가 가지고 있는 멘탈 모델과 코드가 실제로 수행하는 것 사이의 불일치를 일으킬 가능성이 높다.
이로 인해 예상에 벗어나는 동작과 버그가 발생한다.

오류를 숨기는 것은 바람직하지 않으며 때로 심각한 결과가 발생할 수 있다.
오류가 발생하면 알리는 것이 좋다.

### 4.3 오류 전달 방법

오류가 발생하면 더 높은 계층으로 오류를 알린다.
복구할 수 없는 경우는 높은 계층에서 실행을 중지하고, 오류 기록을 한다.
복구가 잠재적으로 가능한 경우는 호출하는 쪽에 오류를 알려 정상적으로 처리할 수 있도록 한다. 

오류를 알리는 방법은 크게 두 종류이다.

- 명시적 방법: 호출한 쪽에서 오류가 발생할 수 있음을 인지할 수밖에 없도록 한다.
  - 기법의 예: 검사 예외, 널 반환 유형(null safety), 옵셔널 반환 유형, 리절트 반환 유형, 아웃컴 반환 유형(반환값 확인이 필수인 경우), 스위프트 오류
- 암시적 방법: 호출한 쪽에 오류를 알리지만, 호출하는 쪽에서 오류를 신경쓰지 않아도 된다.
  - 기법의 예: 비검사 예외, 매직값 반환(should be avoided), 프로미스 또는 퓨처, 어서션, 체크(구현에 따라 달라짐), 패닉

#### 4.3.1 요약: 예외

오류나 예외적인 상황이 발생한 경우 이를 전달하기 위한 방법이다.
예외는 일반적으로 충분한 기능을 가진 클래스로 구현된다.
자바의 검사 예외checked exception를 제외하고 대부분의 주요 언어는 비검사 예외만 가지고 있다.

```
추가 정리)
자바를 잘 모르는 경우를 위해 잠깐 추가해 보자면
검사 예외는 명시적으로 선언해야 하며, 런타임이 아닌 컴파일 타임에 정의해야 하는 예외이다.
```

#### 4.3.2 명시적 방법: 검사 예외

호출하는 쪽에서 예외 처리를 위한 코드를 작성하거나 해당 예외 발생을 선언해야 한다.
따라서 검사 예외를 사용하는 것은 오류를 전달하기 위한 명시적 방법이다.

**검사 예외를 사용한 오류 전달**

검사 예외를 명시적으로 정의하고 코드를 작성, 예외 처리 코드를 작성하지 않으면 컴파일 되지 않는다.

**검사 예외 처리**

검사 예외를 포착하면 오류를 설명하는 메시지를 표시한다.
검사 예외를 포착하지 않는다면 함수의 시그니처에 예외가 발생할 수 있음을 선언해야 한다.
그러면 예외가 발생했을 때 자신이 처리하는게 아니라 함수를 호출한 코드에게 맡기는 것이 된다.

``` java
void displaySquareRoot() throws NegativeNumberException { }
```

#### 4.3.3 암시적 방법: 비검사 예외

비검사 예외는 코드가 예외를 발생시킬 수 있다는 사실을 전혀 모를 수 있다.
역시 문서화가 필요하게 되고 문서는 안보게 되는 문제가 생기므로, 코드 계약의 세부 조항이 된다.
또 세부 조항은 신뢰할 만한 방법이 아닐 때가 많으므로 결국 오류를 암시적으로 알리는 방법이 된다.

**비검사 예외를 사용한 오류 전달**

Java에서는 RuntimeException 클래스를 확장하는 예외 클래스는 비검사 예외이다.
그러면 예외를 발생한다는 선언할 필요가 없게 되고, 문서에서 언급할 수 있지만 강제 사항은 아니게 된다.

**비검사 예외 처리**

예외를 포착하고 처리하는 코드를 검사 예외와 동일한 방식으로 작성할 수 있다.
비검사 예외이므로 이제 호출하는 쪽에서 예외를 확인하고 처리하지 않아도 된다.
비검사 예외는 컴파일이 정상적으로 되고 이후 런타임에서 예외가 발생하면 콜 스택을 타게 되고 끝까지 예외 처리가 안되면 프로그램은 종료된다.

호출하는 쪽에서는 예외가 발생할 수 있는 사실을 몰라도 되므로, 비검사 예외는 오류를 암시적으로 전달하는 방법이 된다.

#### 4.3.4 명시적 방법: 널값이 가능한 반환 유형

널 안정성을 지원할 때 널값이 가능한 반환 유형을 사용하는 건 오류를 전달하기 위한 명시적 방법이다.
널 안정성을 지원하지 않는 언언느 옵셔널 반환 유형을 사용한다.

**널값을 이용한 오류 전달**

널값을 반환하는 방식의 문제는 오류가 발생한 이유에 대한 정보를 제공하지 않기 때문에
널값이 의미하는 바를 주석이나 문서로 설명해야 한다. 

**널값 처리**

호출하는 쪽에서 널인지 확인하고 사용해야 한다.

#### 4.3.5 명시적 방법: 리절트 반환 유형

널값이나 옵셔널 타입은 오류 정보를 전달할 수 없다는 문제가 있다.
추가로 값을 얻을 수 없는 이유를 알려주면 유용한데, 리절트result 유형을 사용하는 것이 적절할 수 있다.

스위프트swift, 러스트Rust, F#은 리절트 유형을 지원한다.
그 외 프로그래밍 언어를 써도 자신만의 리절트 유형을 만들 수 있다.

이걸 만들어서 사용한다고 해도 사용하는 방법에 얼마나 익숙해지는가에 따라 제대로 쓸 수 있다.
getValue()를 호출하기 전 hasError()를 통해 오류를 확인하지 못하면 리절트 유형은 무용지물이 된다.

하지만 리절트 유형이 익숙하다고 가정하면, 이것만으로도 오류가 발생할 수 있다는 점을 알게 되는 것이므로
오류를 알리는 명시적인 방법이 된다.

**리절트 유형을 이용한 전달**

Result< Double, NegativeNumberError >
함수의 결과는 Double 형으로 함수의 에러는 NegativeNumberError에 추가 정보와 잘못된 값을 가지고 있다.

**리절트 처리**

함수를 호출하는 쪽에서는 Result < V, E > 형으로 반환하는 것을 알고 있다.
이걸 사용하는 방법이 익숙하다면 hasError()를 호출해야 하고
오류가 발생하지 않았다는 걸 확인하면 getValue()를 호출하여 결과 값을 얻을 수 있다.
오류가 발생한 경우라면 getError()를 출해서 세부 정보를 얻을 수 있다.

```
체크)
C#은 없나 했는데 공식적으로 .NET에 포함되어 있지 않고
추가 패키지를 설치하면 사용 가능하다.
```
https://betterprogramming.pub/bring-error-handling-and-eliminate-nullreferenceexceptions-using-a-result-type-in-net-f4447dceb6c4 

#### 4.3.6 명시적 방법: 아웃컴 반환 유형(Outcome return type)

함수가 수행한 동작의 결과를 나타내는 값을 반환하도록 함수를 수정하는 것.
호출하는 쪽에서 강제로 확인해야 한다면 오류를 알리는 명백한 방법이다.

**아웃컴을 이용한 오류 전달**

어떤 함수에 파라미터로 객체를 전달한다.
내부에서 적절한 로직이 수행되면 객체의 메서드 호출을 통해 메시지를 전달한다.
오류는 리턴 타입으로 판단한다.
가능한 결과 상태가 두 개 이상, 불리언으로 표현하는게 의미가 분명하지 않은 경우에 열거형으로 사용하면 유용하다.

``` java
Boolean sendMessage(Channel channnel, String message) {
  if (channel.isOpen()) {
    channel.send(message);
    return true; // 메시지 전송 됨
  }
  return false; // 오류 발생
```

```
의견)
의존성 주입DI 방식과 매우 유사한 느낌인데 함수의 파라미터로 전달한 객체의 타입이 인터페이스라면 완벽할 것 같다.
```

**아웃컴 처리**

호출한 함수의 리턴 값을 보고 적절한 처리 로직을 만든다.

**아웃컴이 무시되지 않도록 보장**

호출하는 쪽에서는 반환값을 무시하거나 함수가 값을 반환한다는 사실조차 인식 못할 수 있다.
아웃컴 반환 유형은 오류를 알리는 명시적 방법으로는 한계가 있다.

결과 반환 값을 무시하고, 실제로 처리가 되지 않은 경우에도 처리 되었다고 알려준다.

일부 언어에서는 호출하는 쪽에서 함수의 반환값을 무시하면 컴파일러가 경고를 생성하도록 할 수 있다.

- Java: CheckReturnValue 애너테이션
- C#: MustUseReturnValue 애너테이션
- C++: [[nodiscard]] 속성

이런 표시를 사용하면 컴파일러 경고를 무시하기는 어려우므로 개발자가 아웃컴을 처리한 정상적인 로직으로 수정할 것이다.

#### 4.3.7 암시적 방법: 프로미스 또는 퓨처

비동기적으로 실행하는 코드의 경우 프로미스promise나 퓨처future를 쓴다.
오류 처리를 강제해야 하는 것은 아니지만 코드 계약의 세부 조항을 모르면 오류 처리 코드를 작성해야 한다는 것을 모를 수 있다.

```
의견)
C#은 Task를 쓰고 async await 키워드로 비동기 처리를 깔끔하게 진행한다.
```

**프로미스를 이용한 전달**

비동기로 처리된다는 표시 async await 키워드 사용
예외와 결과에 대해서 리턴 값은 진행되는데 그 결과가 프로미스로 감싸서 진행됨

```
Promise<Double> getSquareRoot(Double value) async {
  await Timer.wait(Duration.ofSeconds(1));
  if (value < 0.0) {
    throw new NegativeNumberError(value);
  }
  return Math.sqrt(value);
```

**프로미스 처리**

프로미스로 리턴된 함수를 처리하는데 있어서
then() 함수는 프로미스가 이행되면 호출되는 콜백 기능을 설정
catch() 함수는 프로미스가 거부되면 호출되는 콜백을 설정하기 위해 사용한다.

```
의견)
코드 예제는 분명 자바의 형태를 띄는데 Promise 문법 자체는 자바스크립트와 매우 유사... 아니 똑같다.
자바에 이런 문법이 있었던가??
의사코드로 만든 거지만 분명 아름다운 코드임에는 틀림없다.
내 생각에는 '자바에는 async await가 아직 없어요!'를 돌려 까는 것 같은 느낌도 든다.
```

**왜 프로미스는 암묵적인 오류 전달 기법인가**

오류가 발생하고 프로미스가 거부될 수 있음을 알려면 프로미스를 생성하는 함수의 세부 사항을 확인해야 한다.
그렇지 않으면 잠재적인 오류 상태를 쉽게 알 수 없고 then() 함수를 통해서만 콜백을 제공할 것이다.
catch() 함수를 통해 콜백이 제공되지 않으면, 오류는 일부 상위 수준의 오류 처리 코드에 의해 포작되거나 모를 수 있다.

프로미스와 퓨처는 비동기 함수로부터 값을 반환하는 훌륭한 방법이다.
호출하는 쪽에서는 잠재적인 오류 시나리오를 알지 못하기 때문에
프로미스와 퓨처를 사용하는 것은 오류를 알리는 암시적인 방법이 된다.

**프로미스를 명시적으로 만들기**

리절트 유형의 프로미스를 반환한다.
유용한 기술일 수 있지만, 코드가 복잡해지기 때문에 모든 사람이 다 이렇게 사용하려고 하지는 않을 수 있다.

``` java
Promise<Result<Double, NegativeNumberError>> getSquareRoot() async { } 
```

```
의견)
프로미스 안에 result를 감싸는 그런 방법이??
코드 이해도만 높다면 책의 부정적인 말에 비해 오류 메시지 처리도 하고 비동기 처리도 하고 좋은 방법일 듯 하다.
```

#### 4.3.8 암시적 방법: 매직값 반환(returning a magic value)

특별한 의미를 부여하는 값이고 그 의미를 알려면 문서나 코드를 읽어야 한다.
그러므로 암시적 오류 전달 기법에 속한다.

매직값의 오류를 알리는 일반적인 값은 -1을 반환한다. 
매직값은 호출하는 쪽에 의미를 알릴 수 없어서 예상을 벗어나는 결과를 가져올 수 있고 버그로 이어질 수 있다.
오류를 알릴 수 있는 좋은 방법에는 속하지 않는다.

### 4.4 복구할 수 없는 오류의 전달

복구할 가능성이 없는 오류가 발생하면 신속하게 실패하고, 요란하게 실패하는 것이 최상의 방법이다.

- 비검사 예외를 발생
- 프로그램이 패닉panic이 되도록
- 체크나 어서션의 사용

프로그램이 종료되면 뭔가 잘못됐다는 걸 알 수 있고
오류 메시지는 스택 트레이스나 줄 번호를 제공하므로 오류 위치를 명확하게 알 수 있다.

오류를 복구할 방법이 없을 때는 암시적인 방법을 사용하면 합리적이다.
자신을 호출한 쪽에 오류를 전달하는 것 밖에는 할 수 없이 때문이다.

### 4.5 호출하는 쪽에서 복구하기를 원할 수도 있는 오류의 전달

이 주제에 대해 모범 사례를 두고 소프트웨어 엔지니어 사이에서 일치된 의견이 없어 흥미로운 주제다.
비검사 예외와 명시적 오류 전달 기법 중 어느 것을 사용해야 하는지에 대한 논쟁이 있는데, 서로 타당한 주장과 반론이 있다.

무엇보다 기억해야 할 것은 나와 나의 팀이 동의한 철학이 다른 어떤 주장보다도 중요하다.
최악은 팀이 하나의 관행을 따르는 것이 아니라 절반만 하고 있고 다른 절반은 다른 관행을 따르는 것이다.

```
의견)
이것은 정말 중요한 문제로 소프트웨어는 팀으로 개발한다는 것을 잘 알고 있다면
팀에서 합리적으로 합의한 결정을 따르는 것이 책 한권 읽고 책에 나온 내용대로 해야 한다고 설레발 치는 것 보다는 훨씬 맞는 방향이라고 본다.
그리고 책의 설명대로 팀에서 절반만 관행을 따르고 있다는 건 처음부터 팀으로 일하는 조직이 아니라고 보고 싶다.
```

#### 4.5.1 비검사 예외를 사용해야 한다는 주장

**코드 구조 개선**

대부분의 오류 처리가 코드의 상위 계층에서 이루어지기에, 비검사 예외를 발생시키면 코드 구조를 개선할 수 있다는 주장
장점은 오류를 처리하는 로직이 코드 전체에 퍼져 있는게 아니라 몇 개의 계층에만 집중되어 있다는 점이다.

**개발자들이 무엇을 할 것인지에 대해서 실용적이어야 함(BEING PRAGMATIC ABOUT WHAT ENGINEERS WILL DO)**

예외를 포착하고도 무시한다거나, 널이 가능한 유형을 확인도 하지 않고 널이 불가능한 유형으로 변환을 하는 것.
예외를 처리하는 명시적인 코드를 추가하다 보면 더 위의 계층까지 수정해야 하고 그러다 보면 작업의 양이 많아지니까
예외 처리는 하되 오류를 숨기고 넘어가는 코드를 짜게 된다.
이런 번거로운 작업 대신에 편의를 도모하기 위해 잘못된 작업을 하고 싶은 마음이 들 수 있다.
이런 방식은 호출하는 쪽에서는 알지 못하기 때문에 좋은 방식은 아니다.

비검사 예외의 사용에 찬성하기 위해 자주 표현되는 주장 중 하나가 이 문제에 대해 실용적으로 접근해야 한다는 주장이다.

#### 4.5.2 명시적 기법을 사용해야 한다는 주장

**매끄러운 오류 처리**

호출하는 쪽에서 오류가 날 가능성에 대해 강제적으로 인식하도록 하면 오류를 매끄럽게 처리할 가능성이 커진다.

**실수로 오류를 무시할 수 없다**

실제로 오류를 처리해야 하는 경우가 있다.
명시적인 오류 전달 방식을 사용한다 해도, 개발자들은 여전히 예외를 포착해도 무시해 버릴 수 있다.
이렇게 하는 것은 적극적인 노력이 들어가는 위반 사항이다.  코드 리뷰시 명확하게 드러나게 되고 잘못된 코드를 차단할 가능성이 커진다.
명확한 오류 전달 방식을 사용하면 잘못된 일이 기본적으로 혹은 실수로 인해 일어나지 않는다.

**개발자들이 무엇을 할 것인지에 대해서 실용적이어야 함(BEING PRAGMATIC ABOUT WHAT ENGINEERS WILL DO)**

오류를 너무 많이 처리해야 하기 때문에 잘못된 처리를 한다는 주장은 비검사 예외의 사용을 반박하는 것에 적용할 수 있다.
비검사 예외는 문서화, 세부 조항이 필요한데 경험상 문서화 되어 있지 않은 경우가 많다.
그래서 어떤 코드가 예외가 발생할지 확실이 알지 못하고 예외 처리를 두더지 잡기 하듯이 할 수 밖에 없다.

여러가지 비검사 예외가 발생하는 걸 처리하다 보면, 문서화 되어 있지 않은 추가 예외가 발생하는 오류를 접하게 되고
그렇게 하다 보면 구체적인 예외(InvalidEncodingException) 보다는 모든 유형의 예외(Exception)을 처리하는 방향으로 간다.
이는 바람직 하지 않은데, 어쩌면 심각한 프로그래밍 오류가 숨겨질 수 있기 때문이다.
또 눈에 띄지 않게 되기 때문에 소프트웨어가 조용하고 이상한 방식(silent and weird way)으로 실패할 수 있다.

모든 유형의 예외로 처리하는 부분은 코드 검토 중에 발견되고 수정되어야 한다.
코드 검토 프로세스가 이런 위반을 탐지할 만큼 충분히 강력하지 않다면, 어떤 방식을 사용하던 문제가 있게 된다.
진짜 문제는 개발자들이 일을 허술하게 하고 이것을 걸러낼 강력한 과정이 없다는 점이다.

```
의견)
이부분을 보고 들었던 생각은 예외 처리 부분 뿐 아니라 코드 검토 프로세스가 잘 갖춰진다면 예외 처리에서 문제점을 사전에 발견하고 조치할 수 있겠다는 점이다.
```

```
의견)
저자가 구글 엔지니어 출신인데 구글에서도 개발자들이 일을 허술하게 한다는 얘기를 책으로 접하게 되다니 사람 사는데는 다 똑같은듯? 
```

#### 4.5.3 필자의 의견: 명시적 방식을 사용하라

호출하는 쪽에서 복구하기를 원할 수도 있는 오류에 대해
비검사 예외를 사용하지 않는 것이 최상이다.
역시 문제는 문서화의 부재고 그로 인해 오류 처리를 어떻게 해야 하는지 개발자가 알기란 거의 불가능하기 때문에 그렇다.

저자가 그런 경험이 많다 보니 명시적 오류 전달 방식을 개인적으로 선호한다.
역시 경험상 비검사 예외가 나중에 심각한 문제가 더 많았다.
하지만 이보다 더 바람직하지 않은 상황은 팀 내에서 개발자들이 일관되지 않고 서로 다른 접근 방식으로 처리하는 것이다.
따라서 팀원들이 오류 전달에 대한 철학에 동의하고 그것을 따르는 것이 가장 바람직하다.

### 4.6 컴파일러 경고를 무시하지 말라

컴파일러 경고는 코드가 의심스러우면 표시해 주는데, 버그에 대한 조기 경고일 수 있다.
경고에 주의를 기울이면 코드베이스에 병합되기 훨씬 전부터 오류를 발견하고 제거할 수 있다.

대부분의 컴파일러는 경고를 오류로 여기고 코드가 컴파일되지 않도록 설정할 수 있다.
과장되고 엄격해 보일 수 있지만, 개발자들이 경고에 대해 수정하도록 강제하므로 실제로는 매우 유용하다.

### 요약

- There are broadly two types of error:
  - Those that a system can recover from
  - Those that a system cannot recover from
- Often only the caller of a piece of code knows if an error generated by that code can be recovered from or not
- When an error does occur it’s good to fail fast and if the error can’t be recovered from to also fail loudly
- Hiding an error is often not a good idea and it’s better to signal that the error has occurred
- Error signalling techniques can be divided into two categories:
  - Explicit: in the unmistakable part of the code’s contract. Callers are aware that the error can happen
  - Implicit: in the small print of the code’s contract, or potentially not in the written contract at all. Callers are not necessarily aware that the error can happen
- Errors that cannot be recovered from should use implicit error signalling techniques
- For errors that can potentially be recovered from:
  - Engineers don’t all agree on whether explicit or implicit techniques should be used
  - My personal opinion is that explicit techniques should be used
- Compiler warnings can often flag that something is wrong with the code. It’s good to pay attention to them.
> 모든 구체 클래스에서 toString을 재정의하자. 귀찮다면 자동 생성이라도 하자.

## toString을 재정의 해야하는 이유
- toString의 규약 : **간결하면서 사람이 읽기 쉬운 형태의 유익한 정보를 반환**
- `클래스_이름@16진수로_표시한_해시코드` 보다는 `PhoneNumber@010-1111-1111`이 더 가독성이 좋고 유익한 정보가 담겨져 있음
- 디버깅에 용이

## 어떻게 재정의할까?
### 객체가 가진 주요 정보를 '모두' 반환하는 것이 좋다.
객체가 너무 거대하거나 문자열로 나타내기 적합하지 않다면 요약 정보를 담는다. 
- 예시) 맨해튼 거주자 전화번호부(총 1487536개)

### 반환값의 포맷을 문서화 할지 정해야 한다. 
전화번호, 행렬같은 값 클래스는 문서화하는 것을 추천한다. 포맷을 명시하면 객체는 표준적이고, 명확하고, 사람이 읽을 수 있게 된다. 

명시한 포멧에 맞는 문자열과 객체를 상호 전환할 수 있는 정적 펙터리나 생성자를 함께 사용해주면 좋다. 
(BigInteger.toString, BigDecimal.toString 등...)

```java
// 포맷을 명시하는 경우
/**
* 이 전화번호의 문자열 표현을 반환한다.
* 이 문자열은 "XXX-YYY-ZZZZ" 형태의 12글자로 구성된다.
* XXX는 지역코드, YYY는 프리픽스, ZZZZ는 가입자 번호다.
* 각각의 대문자는 10진수 숫자 하나를 나타낸다.
* 
* 전화번호의 각 부분의 값이 너무 작아서 자릿수를 채울 수 없다면,
* 앞에서부터 0으로 채워나간다. 예컨데 가입자 번호가 123이라면
* 전화번호의 마지막 네 문자는 "0123"이 된다.
*/
@Override 
public String toString() {
    return String.format("%03d-%03d-%04d", areaCode, prefix, lineNum);
}
```

포맷 명시의 장점 
- 가독성이 좋음 
- 값 그대로 입출력에 사용하거나 CSV 파일처럼 데이터 객체로 저장 가능

포맷 명시의 단점
- 평생 그 포멧에 얽매이게 된다. 
- 포맷 수정이 어려워진다.

포맷을 문서화하는게 부담스럽다면?
- 인텔리제이에서 제공하는 toString 자동 생성을 사용하자 
    - `mac : option + n 에서 toString() 선택`

#### 주의사항 
- 포맷 명시 여부와 상관없이 **toString이 반환한 값에 포함된 정보를 얻어올 수 있는 API를 제공**해야한다. 
    - 위의 예시인 전화번호의 경우 areaCode, prefix, lineNum 정보를 가져오는 접근자가 없으면 toString의 반환값을 파싱해야하기 때문에 향후 포맷이 바뀌면 문제가 발생한다. 
- 재정의시 순환참조를 주의하자. 

### 추가 팁 
- 정적 유틸리티 클래스는 toString을 제공할 이유가 없음
    - 인스턴스를 생성하지 않기 때문이다.
- 대부분의 열거타입도 이미 좋은 toString을 제공하니 따로 재정의할 필요 없음
- 하위 클래스들이 공유해야할 문자열 표현이 있는 추상 클래스라면 toString을 재정의해줘야 한다.
# Java Intermediate 2

## Generic(제네릭)
### 왜 사용?
  - 코드 재사용성과 타입 안정성 2마리 토끼를 잡기 위해
    - Dog, Cat 둘다 똑같은 클래스인데... 코드 중복이 너무 많아요 -> 계속 중복된 코드를 가진 클래스들이 생성되잖아요
    - Object로 해버리면 Dog, Cat 이외에도 Integer, Double처럼 상관없는 객체가 들어올 수 있잖아요! -> 반환 타입을 다 캐스팅 해줘야 하는것도 귀찮고 오류도 많아요  

### 어떻게 선언하는가 
  ```
  package generic.ex1; 
  
  public class GenericBox<T> { // 클래스 옆에 <T> 선언한다 
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }

  }
  
  ```

### 용어 설명
- <> : 다이아몬드
- T : 타입. 후에 Integer나 String으로 변할 수 있음
- 타입 매개변수 : GenericBox<T>에서 T
- 타입 인자 : GenericBox<Integer> 에서 Integer
- 제네릭 타입 : 제네릭이 들어가 있는 클래스나 인터페이스 (ex: GenericBox) = 제네릭 클래스, 인터페이스 

### 특징
- 생성 시점에 원하는 타입 지정 = 메서드 정의 시점에는 타입을 알 수 없다 
- 기본형은 되지 않음. 객체 타입만 인자로 들어올 수 있다 
- 컴파일 과정에 타입 정보를 반영하는 것임 = 그냥 클래스가 바뀌어서 생성되는구나로 이해
- **사용할 타입을 미래로 미루는 것**이 포인트

### 관례
- 일반적으로 제네릭 타입 매개변수에는 대문자를 사용
  - ex) T = type, K = key 

### 기타
- 한번에 여러 타입 매개변수를 선언할 수 있다(ex: class Data<K, V>)
- 타입 매개변수로 아무것도 지정하지 않으면(`GenericBox integerBox = new GenericBox();`) Object가 들어간다 = row type
  - row type은 쓰지 말아야 한다!!! 
  - 항상 타입을 지정하는 것을 권장한다(`GenericBox<Object> integerBox = new GenericBox<>();`)
  - row type은 제네릭이 없던 과거 코드와의 하위 호환이 필요하기에 사용한다 
  
### 제네릭의 문제점
- T에 어떤 값이 들어올지 예측 불가능하기 때문에, 클래스 내부에서 특정 객체의 메서드는 사용할 수 없다. 즉, Object 메서드만 사용할 수 있음 
- 제네릭에서 타입 매개변수를 사용하면 어떤 타입이든 들어올 수 있다
-> 해결 방법은 타입 매개변수 제한

### 타입 매개변수 제한 
- `<T extends Animal>` : 타입 매개변수에 입력될 수 있는 상한을 지정 
- 타입 매개변수를 Animal과 그 자식만 받을 수 있도록 제한을 두는 것
- 적어도 Animal의 메서드는 쓸 수 있음 (부모, 자식 객체들을 받으니까 적어도 부모 객체의 기능 정도는 쓸 수 있게 해주는 것)
- ! 근데 제한하지 말고 그냥 Animal 받으면 되는거 아니에요?
  - <Animal> 이 되어버리면 개 병원에 고양이도 받을 수 있게 됨. 타입 매개변수가 다형성의 원리가 적용되니까

### 제네릭 메서드 
- != 제네릭 타입(제네릭 클래스)
  - 제네릭 타입은 클래스에 사용하는 것. 객체를 생성하는 시점에 타입이 정해짐 
- 메서드 반환 값 앞에 다이아몬드<>를 사용한다
  - ex: `public static <T> T genericMethod(T obj)`와 같이 사용한다
  - 특정 메서드 단위로 제네릭을 도입할 때 사용함
- 사용방법은 `GenericMethod.<Integer>genericMethod(10);` 처럼 하면 된다
  - 메서드를 호출하는 시점에 타입이 결정된다
  - 다이아몬드를 생략해도 된다 -> 들어오는 인자로 추론 가능 
- 인스턴스, static 메서드 둘 다 적용할 수 있음
  - 하지만 static 제네릭 메서드는 외부 클래스가 만약 제네릭 타입이라면 제네릭 타입의 타입을 사용할 수 없다(왜냐하면 객체가 생성이 되어야지만 타입이 정해지기 때문)
- 타입 매개변수도 제한할 수 있다


### 와일드 카드
- 와일드 카드는 제네릭 타입이나 제네릭 메서드를 선언하는 것이 아니다.
  - **이미 만들어진 제네릭 타입을 활용할 때 사용한다.** -> 제네릭 타입이나 제네릭 메서드가 아니다!!!
  - 와일드 카드는 제네릭을 쉽게 사용할 수 있도록 도와주는 도구
- 그냥 일반적인 메서드, 일반적인 클래스이다
- Box<Dog>, Box<Cat>처럼 이미 타입 인자가 정해진 제네릭 타입을 전달받아 사용한다 
```
public class WildcardEx {

    static <T> void printGenericV1(Box<T> box) { // 제네릭 타입을 꺼내기
        System.out.println("T = " + box.get());
    }

    static void printWildcardV1(Box<?> box) { // 와일드 카드 사용
        System.out.println("? = "+ box.get());
    }

}

```
- 제네릭 메서드는 반환 값을 내가 동적으로 지정할 수 있음 <-> 와일드 카드는 지정할 수 없음 (캐스팅 필수)
- extends를 통해 상한을 지정할 수 있다(? extends Animal : Animal을 포함한 그 자식들 ) <-> super를 통해 하한을 지정할 수 있다 (? super Animal : 최소 Animal부터)

## ArrayList
### 배열
- 인덱스를 통해 입력, 변경, 조회는 O(1)이다
  - 이유 : `배열 한 칸 당의 메모리 * 찾고자 하는 인덱스 + 인덱스 0의 위치` 를 통해 바로 주소를 찾을 수 있음
  - 배열 한 칸 당 차지하는 메모리가 같기 때문에 + 배열이 다 같이 붙어 있음
- 배열에 들어있는 데이터를 찾는 검색은 O(n)이다
  - 이유 : 배열 안에 있는 값을 찾는 것이기 떄문에 하나하나 비교해야 함
- 데이터를 추가할 때에는 성능이 위치에 따라 다르다
  - 첫번째 위치 : O(1+N) = O(N) (첫번째 위치 찾기 + 값을 뒤로 밀기)
  - 중간 위치 : O(1+N/2) = O(N) (중간 위치 찾기 + 절반의 값을 뒤로 밀기)
  - 마지막 위치 : O(1) (마지막 인덱스 찾기)
- 장점 : 인덱스를 활용할 때 O(1)이라는 미친 효율
- 단점 : 크기가 생성시점에 정해야 함(메모리 부족 or 메모리 낭비 발생) -> 길이를 동적으로 변경할 수 없다 -> 데이터를 추가하기 어려움 

### 빅오(O) 표기법
- 중요한 것은, 알고리즘의 정확한 실행 시간을 계산하는 것이 아니라 데이터 양의 증가에 따른 성능의 변화 추세를 이해하는 것(데이터가 많아지면 이 알고리즘은 대략적으로 점점점 이정도로 느려지겠군!)

### 리스트(List)
- 배열의 불편함을 해소
- **순서가 있고 중복을 허용함**
- 배열과의 차이점 : 크기가 정적(배열), 크기가 동적으로 변함(리스트)
- 
#### ArrayList(배열 리스트)
- 실제 내부에서는 배열을 사용한 리스트
- 마지막에 있는 데이터를 삭제할 때는 O(1)로 매우 빠름
- 하지만 중간에 데이터를 추가하거나 삭제할 때에는 O(N)으로 느리다
- 데이터를 중간에 추가하고 삭제하는 변경보다는 순서대로 입력하고 순서대로 출력하는 경우에 효율적이다 
- 메모리가 좀 낭비가 되는 것은 있음 

#### LinkedList
- ArrayList의 단점
  - 사용하지 않는 공간 낭비
  - 배열의 중간, 앞 부터 데이터를 추가할 경우 : 데이터를 이동하고 해줘야 함
- ArrayList의 단점을 보완 : 중간 위치의 데이터 추가 가능, 메모리 낭비 없음
- 단점
  - 검색, 중간이나 마지막에 데이터 추가할 때 O(N)의 시간복잡도가 소요됨
- 장점
  - 하지만 메모리를 낭비하지 않음 
### ArrayList, LinkedList 둘 다 비슷한데 ? -> 추상화 가능 

#### 의존관계 주입
추상화된 인터페이스에 의존해서 클라이언트 코드 변경 없이 런타임에 구현체를 선택할 수 있다 

#### 컴파일 타임, 런타임 의존관계
- 컴파일 타임 : 코드 컴파일 시점(소스코드를 보고 결정하는 시간)
- 런타임 : 프로그램 실행 시점(코드 한줄 한줄 인스턴스를 생성하는 시간)

- 컴파일 의존관계 : 코드를 보고 직관적으로 아는 관계(무슨 클래스가 무슨 클래스를 안다)
- 런타임 의존관계 : 인스턴스 간의 의존관계

- **클라이언트 클래스는 컴파일 타임에 추상적인 것에 의존하고, 런타임에 의존 관계 주입을 통해 구현체를 주입받아 사용함으로써 OCP 원칙을 지킴**
  -> 전략 패턴 : 결정을 나중으로 미룬다 -> 재사용성 (추상화에만 의존하기 때문에 구현체 아무거나 들어올 수 있어서 런타임 시점에 변경 가능, 코드 중복 삭제)

- 배열 리스트는 메모리 상 붙어서 데이터가 저장되기 때문에 CPU 캐시효율이 좋다 접근 속도도 빠르다
- 반면, 연결 리스트는 메모리 상에 떨어져서 생성되기 때문에 CPU 캐시 효율이 좋지 않다

### Collection 인터페이스
- 자바가 제공하는 자료구조의 공통기능을 모아둔 인터페이스
  - ex) add, remove 등 
- Collection : 자료를 모아둔 것
- List : Collection에 순서를 부여한 것

### Java List
- 배열과 비슷하지만 크기가 동적으로 변화하는 컬렉션을 다룰때 사용하는 자료구조
#### Java ArrayList
- 배열을 사용함
- 기본 사이즈 10
- 사이즈가 넘어가면 50% 증가
- **중간에 데이터를 추가하면, loop를 사용하는 것이 아닌 시스템 레벨(OS,CPU)에서 최적화된 메모리 고속 복사 연산을 사용해서 빠르게 수행함** 
#### Java LinkedList
- 이중 연결 리스트 구조
- 마지막 노드에 대한 참조를 제공함 : 역방향으로 조회가능 -> 인덱스 조회 성능 최적화 가능
#### 실제로는
- 평균적으로 데이터를 중간에 삽입할 경우 ArrayList가 훨씬 빠르다(메모리가 붙어있기 때문에 CPU 캐시효율이 좋고 접근 속도가 빠르기 때문 + 고속 복사 기능)
- ArrayList의 사이즈가 늘어나야 할 경우는 배열을 다시 만들어 복사하는 과정이 있지만 이것은 가끔 일어나는 일이고 50%만 증가하기 때문에 그렇게 성능에 영향을 미치지 않는다

#### 따라서
- 앞에서 추가할 경우를 제외하고는 ArrayList가 성능상 유리하다 -> 대규모 데이터일 경우 
- 리스트는 순서가 중요하거나 중복된 요소를 허용해야 하는 경우에 주로 사용된다
---
## Set
- 유일한 요소들의 컬렉션
- 중복을 허용하지 않고, 요소의 유무만 중요한 경우에 사용됨
- 또한, 순서를 보장하지 않

### 해시 알고리즘의 생각원리
- 나오게 된 이유 : Set에서 중복 체크를 할 때 O(N)의 성능이 걸림 
- 처음 : 인덱스와 저장될 데이터의 값이 같다면?
  - 문제 : 찾는 속도가 비약적으로 O(1)이지만, 메모리 공간이 너무 낭비가 돼...
- 해결책 : 나머지 연산 사용 -> 입력값을 배열의 크기로 나머지 연산하여 인덱스로 사용하는 것
  - 문제 : 해시 충돌 -> 배열 안에 또 배열을 만들어서 해결한다
  - 완료 : 해시 충돌이 날 경우는 거의 없기 때문에 add, remove, contains 모두 O(1)의 성능 

### 문자열 해시 코드
- 문자 데이터를 기반으로 숫자 해시 인덱스를 구하려면?
- 해시 코드 : 문자를 charArr로 바꿔서 각 char의 아스키 코드를 더한 것 즉, 데이터를 대표하는 값
- 해시 인덱스 : 해시코드를 가지고 나머지 연산 즉, 해시 코드를 사용해 데이터의 저장 위치를 결정하는 값 
- 해시 함수 : 임의의 길이의 데이터를 입력 받아 고정된 길이의 해시값(해시 코드)를 출력하는 함수 즉, 해시 코드를 만들어내는 함수
  - ex) 객체여도, 숫자여도 모두 정수로 해시코드가 생성된다던지 

### hashCode()와 equals()
- 자바는 모든 객체가 자신만의 해시 코드를 표현할 수 있는 기능을 제공함 -> Object.hashCode()
  - Object.hashCode() : 원래는 객체의 참조값으로 해시 코드로 만듦 -> 따라서 보통은 오버라이딩을 해야 함
    - Objects.hash() 사용해서 오버라이딩 
- **Integer, String과 같은 기본 클래스들은 내부에 이미 값을 기반으로 hashCode를 만들 수 있도록 hashCode() 메서드를 재정의를 해 놓음**
- 직접 만든 객체의 경우 hashCode()와 equals()를 반드시 오버라이딩 해야 한다 -> equals는 해시 충돌이 되어 리스트가 된 인덱스에서 같은 id값이 있는지, 그 id랑 찾으려는 id와 같은지를 확인한다
  - hashCode, equals가 오버라이딩 안되었을 때 : 참조값을 기반으로 해시 코드를 생성하기 때문에 -> 같은 id 객체가 다른 인덱스에 저장되어서 결과적으로 set에 중복 저장 + 검색 안됨
  - hashCode는 했지만 equals가 오버라이딩 안되었을 때 :
 
### java set
- Collection(interface) <- Set(interface) <- HashSet <- LinkedHashSet, TreeSet
- **중복을 허용하지 않는** 유일한 요소의 집합 또한 **순서 보장하지 않음**
  - 수학의 집합 요소를 구현한 것 
- 특정 요소가 집합에 있는지 여부 확인에 최적화
#### HashSet(조회 가장 빠름)
- 해시 자료 구조를 이용해서 요소 저장
- 순서를 보장하지 않음(나머지 연산으로 해시 인덱스를 구하기 때문에)
- 해시를 사용하기 때문에 평균적으로 O(1)
- 유일성이 중요하지만 순서는 중요하지 않을 때
- hashCode, equals()를 모두 사용
#### LinkedHashSet(순서 중요)
- HashSet + 연결 리스트 추가 = 요소들의 **입력 순서 유지**
- 조회 시 요소들이 추가된 순서대로 반환
- 평균 O(1)
- 데이터 유일 + 삽입 순서가 중요할 때
- 연결 링크를 유지해야 하므로 HashSet보다 조금 더 무거움(노드 객체를 저장해야 하기 때문에)
- 양방향 + tail도 있음 
#### TreeSet(데이터 자체 정렬) 
- 내부에 레드 블랙 트리(이진탐색 트리를 개선한)를 사용
  - 트리가 한쪽으로 치우쳐지지 않게 하기 위해서 
- **요소들이 정렬된 순서로 저장(데이터 자체의 값들을 순서)**
- O(log n)
- 용도 : 데이터들을 정렬된 순서로 유지하면서 집합의 특성을 유지할 떄
#### java의 SET 최적화
- 해시 충돌을 막기 위해, HashSet은 데이터 양이 75%를 넘어갈 때 배열의 크기를 2배 늘리고 늘어난 크기로 다시 해시 인덱스를 적용함(rehashing)

-- 
## MAP, Stack, Queue
### Map
- **key-value**
- key는 중복되면 안됨. 값은 중복 가능
- 순서를 유지하지 않음
- **Collection 인터페이스의 상속을 받지 않음**
  - 컬렉션은 하나로 쭉 저장하는 형태임 
- HashMap, LinkedHashMap, TreeMap : HashMap을 가장 많이 사용

- Map의 keySet() 메서드 : key는 중복을 허용하지 않고, 순서를 보장하지 않는다 -> Set에 적합!
- 반대로 values는 collection에 적합하다 : value는 중복이 되어도 되니까(set안됨) -> 하지만 순서를 보장하지 않음(list는 안됨)
  - 값의 모음인 Collection으로 반환 
- entry : key-value의 짝. key-value 하나가 entry임
- 즉, Map은 내부에서 Entry 객체를 만들어서 보관함
- putIfAbsent : key가 없는 경우에만 데이터를 저장하고 싶을 때 사용 
#### HashMap? HashSet? 뭔가 비슷한 느낌
- Map과 Set은 뭔가 비슷한 느낌이 들 것이다 -> 그렇다면 정답! Map은 Set은 거의 같음 
- 사실 Map은 set 옆에 단순히 value가 따라 붙은 것
- **실제 자바 내부에서 HashSet의 구현은 대부분 HashMap의 구현을 가져다가 사용함. 그냥 Map에 value만 비워두면 Set으로 이용가능**
#### HashMap
- 실무에서 가장 많이 사용
- key로 들어가는 객체는 반드시 hashCode와 equals를 오버라이딩 해야 한다

--
## Stack
- LIFO : 가장 마지막에 넣은 것이 가장 먼저 나간다
- stack클래스를 사용하지 말자! 대신에 Deque를 사용하는 것이 더 좋다

## Queue
- FIFO : 가장 먼저 넣은 것이 가장 먼저 나옴
- Collection 인터페이스를 상속받은 Queue 인터페이스가 있음
  - 그리고 Queue 인터페이스를 상속받은 Deque가 있음

## Deque
- Double Ended Queue : 양쪽 끝에 요소를 추가하거나 제거할 수 있다
- **일반적인 큐와 스택의 기능을 모두 포함하고 있음**
- 대표적인 구현체 : ArrayDeque(이게 훨씬 빠름), LinkedList
### Deque와 Stack, Queue
- Deque는 Stack과 Queue로 사용하기 위한 메서드 이름까지 제공함 

--
## 순회
- 자료 구조에 들어있는 데이터를 차례대로 접근해서 처리하는 것
- 하지만, 자료 구조마다 데이터를 접근하는 방법이 모두 다르다
- 자료구조 모두 일관성있게, 똑같은 방법으로 순서대로 접근하는 방법 = iterable, iterator 사용

### Iterable 인터페이스 
- '반복 가능한' 이란 뜻 : 클래스를 순회할 수 있게 만듦 
- 인터페이스
- 내부에 iterator(반복자) 인터페이스를 return 하는 iterator() 메소드만 갖고 있음 : 이걸 가지고 반복하게 만듦
- 반복가능한 클래스에서 반복자가 나온다 

### Iterator 인터페이스
- '반복자' 라는 뜻 
- 인터페이스
- 내부에 hasNext, next 메소드 있음 

- Collections 자료구조들은 Iterable을 상속받고 내부에 Iterator을 반환하는 iterator 메소드를 오버라이딩함 
- Iterator 인터페이스는 내부에 hasNext, next 메소드가 있어서 Iterator의 구현체는 그것을 오버라이딩한다

### 향상된 for문 = for-each
- for-each를 사용하려면 **Iterable을 상속받은 객체만 가능**하다
- 자료구조를 순회하는 것이 목적
- 컴파일 시점에 Iterator의 hasNext와 next로 코드를 바꿔준다


## Comparable, Comparator
- Iterable, Iterator처럼 정렬할 수 있는, 정렬자?
- 데이터를 정렬하는 방법
### 자바의 정렬 알고리즘
- 자바는 데이터가 작을 때는 듀얼 피벗 퀵소트, 데이터가 많으면 팀소트를 사용함 -> O(n log n)

### Comparator
- 비교자
- 두 값을 비교할 때 비교 기준을 직접 제공할 수 있다
- 내부에 compare()이란 메소드가 있다
  - 2개의 인자를 받고, 첫번째 인자가 두번째 인자보다 현재 앞에 서 있는 형태이다 
  - 파라미터 2개 중 무엇이 더 큰지를 내가 정할 수 있음
  - **내림차순이던 오름차순이던 Comparator의 return 값이 -1과 0이면 자리 유지. 1이면 무조건 자리 바꿈** -> 이 부분은 알고리즘이 해주니까 우리는 그저 두 파라미터 간의 차이만 구하면 된다
  - 두 파라미터가 마치 1씩 차이가 난다고 가정하고 -1일지 1일지를 골라보자 
  
- 오름차순 comparator에 reverse()를 붙일수도 있다 : compare 계산을 정반대로 뒤집을 수 있다 
- Arrays.sort에 비교자를 넘겨주면 된다
  - 알고리즘에서 어떤 값이 더 큰지 두 값을 비교할 때 사용

- 내가 만든 객체를 정렬하려면?
  - 어떤 객체가 더 큰지 알려줄 방법이 필요
  - 기본 정렬 방법 : **Comparable 인터페이스를 구현 : 객체에 비교 기능을 추가해 줌**
    - 객체는 Comparable을 구현해야 함 -> compareTo 메소드 오버라이딩 -> 이게 기본 정렬 방법(Arrays.sort)
  - 다른 필드값으로 정렬하고 싶다면 -> Arrays.sort의 인수로 Comparator를 만들어 넘겨준다.
  - Arrays.sort 기본 정렬을 사용하려면 인자로 Comparable을 구현한 객체를 받아야 한다
  - 하지만 만약 Arrays.sort의 인자로 반복자가 들어간다면, 객체는 Comparable을 구현할 필요가 없다
  - 자바의 기본 객체들은 모두 Comparable 구현체이다
  - compareTo : Comparable에 있는 기본 정렬 기준 메소드
  - compare : Comparator에 있는 임의의 정렬 기준 메소드 
 
- 정리
  - 정리하자면, 객체의 정렬이 필요하면, Comparable을 통해 기본 자연 순서(오름차순, 내림차순)를 제공하고 Comparator을 이용해서 자연 순서 외에 다른 정렬 기준을 추가할 수 있다

## Collection Utils
- of메소드 : 불변 컬렉션을 생성함
- 

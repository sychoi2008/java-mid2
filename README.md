# Java Intermediate 2

## Generic(제네릭)
### 왜 사용?
  - 코드 재사용성과 타입 안정성 2마리 토끼를 잡기 위해
    - Dog, Cat 둘다 똑같은 클래스인데... 코드 중복이 너무 많아요 -> 계속 클래스들이 생성되잖아요
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
- Generic Type : 제네릭이 들어가 있는 클래스나 인터페이스 (ex: GenericBox) = 제네릭 클래스, 인터페이스 

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

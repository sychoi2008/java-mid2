# Java Intermediate 2

## Generic(제네릭)
### 왜 사용?
  - 코드 재사용성과 타입 안정성 2마리 토끼를 잡기 위해
### 어떻게 사용?
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
- 생성 시점에 원하는 타입 지정
- 기본형은 되지 않음 객체 타입만 가능
- 자바 컴파일러가 우리가 입력한 타입 정보를 기반으로 이런 코드가 있다고 가정함 -> 컴파일 과정에 타입 정보를 반영하는 것임
- 그냥 클래스가 바뀌어서 생성되는구나로 이해
- **사용할 타입을 미래로 미루는 것**이 포인트

### 관례
- 일반적으로 제네릭 타입에는 대문자를 사용

### 기타
- 한번에 여러 타입 매개변수를 선언할 수 있다(ex: class Data<K, V>)
- 타입 매개변수로 아무것도 지정하지 않으면(`GenericBox integerBox = new GenericBox();`) Object가 들어간다 = row type
  - row type은 쓰지 말아야 한다  
  - 항상 타입을 지정하는 것을 권장한다(`GenericBox<Object> integerBox = new GenericBox<>();`)
  - 제네릭이 없던 과거 코드와의 하위 호환이 필요하기에 
  

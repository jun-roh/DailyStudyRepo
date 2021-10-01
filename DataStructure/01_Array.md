## 01 . Array

#### 1. Primitive 자료형과 Wrapper 클래스
Wrapper 사용이유
- null 가능
- 메소드 사용가능 ex) toString
- Generic 사용시에는 Wrapper 형태로만 사용 가능
Primitive 자료형
- Wrapper 클래스 객체 생성 비용 참조 비용 등 많이 들기때문에 비쌈
- 가벼운 Primitive 자료형 사용

#### 2. List 와 ArrayList 차이점
- List 는 인터페이스, ArrayList 는 클래스
- 인터페이스는 모든 메서드가 추상 메서드인 경우를 의미
- 인터페이스를 상속받는 클래스는 인터페이스에서 정의된 추상 메서드를 모두 구현해야함
- ArrayList 가 아니라 List 로 선언된 변수는 필요에 따라서 리스트 클래스를 쓸 수 있는 구현상의 유연성을 제공
    - List<Integer> list = new ArrayList<Integer>();
    - List<Integer> list = new LinkedList<Integer>();


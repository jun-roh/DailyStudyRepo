## 07. Process 구조

#### 1. Process 구조
![Alt text](./images/process_structure.png "프로세스 구조")

* Text(CODE) : 코드
* data : 변수 / 초기화된 데이터
* stack : 임시 데이터 (함수 호출, 로컬 변수 등)
* heap : 코드에서 동적으로 만들어지는 데이터


* PC(program Counter) + SP(Stack Pointer)
    * PC : 코드를 한줄 한줄 가리키는 주소 레지스터
    * SP : 함수가 실행될 때 최상단 주소 레지스터
    
#### 2. Heap 이란?

* 위에서 말한것과 같이 코드에서 동적으로 만들어지는 데이터
* Java 일 경우에 new 키워드로 생성
* C 언어에서는 malloc 함수를 통해서 동적 할당
* 인위적으로 할당을 하기 때문에 프로세스 실행 중에 메모리 영역이 결정됨

#### 3. Stack overflow

* Data 영역은 BSS 와 DATA 로 구분
* BSS 는 초기화 되지 않은 전역 변수
* DATA 는 초기 값이 있는 전역변수
* Stack Overflow 는 Stack Pointer 가 Stack 의 경계를 넘어설 때 일어남
* 데이터 저장할 때 해당 데이터가 Stack Memory 크기보다 더 많은 Stack Memory 를 사용하면서 발생하는 에러
* 해킹 사용 방법 중에 하나
  * 스택 메모리 사이즈보다 많은 데이터를 저장할 때 다른 stack 메모리 공간에 덮어 씌어지면서 저장
  * 자신이 원하는 함수의 위치로 저장시키면서 공격

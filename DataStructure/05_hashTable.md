## 05. HashTable

#### 1. Hash Table
- Key 와 Value 를 매핑할 수 있는 데이터 구조
- Hash 함수를 통해 배열에 키에 대한 데이터를 저장할 수 있는 주소를 계산
- key를 통해 바로 데이터가 저장되어 있는 주소를 알 수 있으므로 저장 및 탐색 속도가 빨라짐
- Hash 함수가 생성할 수 있는 주소에 대한 공간을 배열로 할당한 후 키에 따른 데이터 저장 및 탐색 지원
  - Slot : Hash Table 에서 한 개의 데이터를 저장할 수 있는 공간

#### 2. Hash Function
- 임의의 데이터를 고정된 길이의 값으로 리턴해주는 함수
  - Hash, Hash Value, Hash Address : Hashing 함수를 통해 리턴된 고정된 길이의 값



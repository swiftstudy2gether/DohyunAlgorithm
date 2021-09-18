# Algorithm



### Swift


### - 대문자 O(빅오) 표기법
- 입력 n에 따라 결정되는 시간 복잡도 함수
- O(1), O(log n), O(N), O(n log n), O(N²), O(2²), O(N!)
- 입력 N의 크기에 따라 기하급수적으로 시간 복잡도가 늘어날 수 있슴
- O(1) < O(log n) < O(N) < O(n log n) < O(N²) < O(2²) < O(N!)
단순하게 입력 n에 따라, 몇번 실행되는지를 계산하면 됩니다. 표현식에 가장 큰 영향을 미치는 n 의 단위로 표기합니다. n이 1이든 100이든, 1000이든, 10000이든 실행
- 무조건 2회(상수회) 실행한다: O(1)
- N 에 따라 N번, N + 10번 또는 3N + 10 번등 실행 한다 : O(N)
- N 에 따라 N²번, N² + 1000번, 또는 100N² - 100 번등 실행한다 : O(N²) 


#### - abs() : Swift에서 제공해주는 함수이며 절대값을 Return 해주는 함수이다
- abs Method는 얼마나 떨어져있는지의 거리를 구할때 주로 사용하면 편리하다.




### 해쉬 테이블(Hash Table) : 키에 데이터를 저장하는 구조

##### - (Hash)해쉬 용어

###### 해쉬 함수(Hash Function) : key에 대해 산술 연산을 이용해 데이터 위치를 찾을 수 있는 함수

###### 해쉬 테이블(Hash Table) : 키 값의 연산에 의해 직접 접근이 가능한 데이터 구조

###### 해쉬(Hash) : 임의 값을 고정 길이로 변하는 것

###### 해쉬 값(Hash Value) 또는 해쉬 주소(Hash Address) : key를 해싱 함수로 연산해서, 해쉬 값을 알아내고 이를 기반으로 해쉬 테이블에서 해당 key에 대한 데이터 위치를 일관성 있게 찾을 수 있음

###### 슬롯(Slot) : 한 개의 데이터를 저장할 수 있는 공간



#### Unicode to Int

먼저 아스키 코드를 Int형으로 바꾸는 방법 입니다.

~~~ swift
var char = "A"

UnicodeScalar(char)?.value // 65 -> UInt32
~~~

이렇게 하면 A의 아스키코드 값인 65는 나오지만 UInt32 Type입니다. Int로 이값을 사용해야한다면 Int로 캐스팅해주세요 :)

~~~ swift
let num = 97

if let num = UnicodeScalar(num) {
  print(num) // a
}
~~~

 UnicodeScalar는 옵셔널을 반환하기 때문에 if let으로 바인딩 해주었습니다.

UnicodeScalar를 사용하지 않고 편하게 아스키코드 값을 뽑는 방법

~~~ swift
let str = "abcDEF"

for index in str.utf16 {
  print(index) // 97 98 99 68 69
}
~~~

 이렇게 하면 간단하게 아스키 코드값을 뽑을수 있다 :)



### 해쉬 테이블 만들기 

~~~ swift
// MARK: - HashTable 구현
var hash_table: [String?] = .init(repeating: nil, count: 3)

// MARK: - Hash Function 구현 (Division)법을 통해 해쉬 테이블 접근
func hash_func(_ key: Int) -> Int {
  return key % 5
}
// MARK: - Hash Table 값 저장
func updateValue(_ value: String, key: String) {
  guard let key = UnicodeScalar(key)?.value else { return }
  let hashAddress = hash_func(Int(key))
  hash_table[hashAddress] = value
}

// MARK: - Hash Table 값 읽기
func getvalue(forkey key: String) -> String? {
  guard let key = UnicodeScalar(key)?.value else { return nil }
  let hashAdress = hash_func(Int(key))
  return hash_table[hashAdress]
}


~~~



---



##### 자료구조 해쉬 테이블 장단점과 주요 용도

##### 장점

- 데이터 저장/읽기 속도가 빠르다.(검색 속도가 빠르다.)
- 해쉬는 키에 대한 데이터가 있는지(중복) 확인이 쉬움

##### 단점

- 일반적으로 저장공간이 좀더 많이 필요하다.
- 여러 키에 해당하는 주소가 동일할 경우 충돌을 해결하기 위한 별도 자료구조가 필요함

##### 주요 용도

- 검색이 많이 필요한 경우
- 저장, 삭제, 읽기가 빈번한 경우
- 캐쉬 구현시(중복 확인이 쉽기 떄문)



~~~ swift
var hash_table:[String] = .init(reapting:nil, count: 10)

func hash_function(_ key: Int) -> Int{
  return key % 8
}

// MARK: - HashTable 값 생성
func updateKeyValue(_ key: String, value: String) {
  guard let key = UnicodeScalar(key)?.value else { return }
  let hashAddress = hash_function(Int(key))
  hash_table[hashAddress] = value
}
// MARK: - HashTable 값 읽기
func getValue(forkey key: String) -> String? {
  guard let key = UnicodeScalar(key)?.value else { return }
  let hashAddress = hash_function(Int(key))
  return hash_table[hashAddress]
}


~~~



###### 충돌(Collision) 해결 알고리즘 (좋은 해쉬 함수 사용하기)

> 해쉬 테이블의 가장 큰 문제는 충돌(Collision)의 경우입니다. 이 문제를 충돌(Collision) 또는 해쉬 충돌(Hash Collision)이라고 부릅니다. 

###### Chaining 기법

- 개방 해슁 또는 Open Hashing 기법중 하나 : 해쉬 테이블 저장공간 외의 공간을 활용하는 기법
- 충돌이 일어나면, 링크드 리스트라는 자료구조를 사용해서, 링크드 리스트로 데이터를 추가로 뒤에 연결시켜서 저장하는 기법

~~~ swift
public struct HashTable<key:Hashable,Value> {
    private typealias Element = (key: key, value: Value)
    private typealias Bucket = [Element]
    private var buckets: [Bucket]

    private(set) public var count = 0 //count를 통해 테이블에 Item 갯수를 계속 파악
    private var isEmpty: Bool { return count == 0 }

    public init(capacity: Int) { //main 배열의 이름은 buckets이고 이건 고정 사이즈라 사이즈 초기화 함수 제공해야함
        assert(capacity > 0)
        buckets = Array<Bucket>(repeating: ([]), count: capacity)
    }

    //Hash Function
    private func index(forkey key: key) -> Int { // 주어진 키를 활용하여 요소를 집어넣을 위치인 배열의 인덱스를 계산
        return abs(key.hashValue % buckets.count)
    }

    public func value(forkey key: key) -> Value? {
        let index = self.index(forkey: key)
        for element in buckets[index] {
            if element.key == key { // 키에 맞는값 찾으면 리턴
                return element.value
            }
        }
        return nil // 해쉬테이블 안에 key 값이 없을떄
    }

    public mutating func updateValue(_ value: Value, forKey key: key) -> Value? {
        let index = self.index(forkey: key)

        for (i,element) in buckets[index].enumerated() {
            if element.key == key {
                let oldValue = element.value
                buckets[index][i].value = value
                return oldValue
        }
     }
        buckets[index].append((key:key,value:value))
        count == 1
        return nil
    }


    public subscript(key: key) -> Value? {
        get {
            return value(forkey: key)
        } set {
            if let value = newValue {
                updateValue(value, forKey: key)
            } else {
								
            }
        }
    }
}
~~~



###### Linear Probing 기법

- 폐쇄 해슁 또는 Close Hashing 기법 중 하나: 해쉬 테이블 저장공간 안에서 충돌 문제를 해결하는 기법
- 충돌이 일어나면, 해당 hash address의 다음 address부터 맨 처음 나오는 빈공간에 저장하는 기법
- 저장공간 활용도를 높이기 위한 기법







---

##### Swift - HashAble

> Swift의 Dictionary 자료형은 Key - Value 쌍으로 이루어져 있다.

~~~ swift
var dictionary: [String: String] = [:]

// MARK: - Dictinary add Key- value pair
dictionary["firstName"] = "Steve" 
// MARK: - Outputs "Steve"
dictionary["firstName"]
~~~

이 Dictionary는 저장을 위해 Key - Value 쌍을 Hash  Table로 전달할 것이다.



##### Hash Tables

- Hash Table은 배열에 불과하다. 처음에 배열은 비어있고, 특정 키값을 Hash Table에 넣으면 Hash Function을 이용해 계산된 값을 배열의 index에 저장한다.

![alt text](https://media.vlpt.us/images/hansangjin96/post/f8870769-7c7c-413f-962b-c6ef21d46b23/image.png)

> hashTable["firstName"] = "Steve"

- hashTable은 "firstName" 키를 받아서 hashValue 프로퍼티에 물어본다. 따라서 키는 Hashable 프로토콜을 반드시 상속한다.



#### Hash Functions

"firstName".hashValue 값을 사용하면 big Integer인 -8378883973431208045을 return 한다.

![alt text](https://media.vlpt.us/images/hansangjin96/post/268862be-4b00-4547-a006-c959f5c9625b/image.png)

아래와 같이 간단한 Hash 함수를 작성할 수 있다.




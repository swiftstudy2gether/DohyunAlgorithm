### [BOJ] 10828 스택 문제

- Stack은 (LIFO) 즉 Push와 pop을 이용한 자료구조이다



### 	문제 풀이

- 풀이(1) : Push 입력시 정수 x를 스택 배열에 append 합니다 ^^
- 풀이(2) : pop 스택에서 가장위의 정수를 빼고 그 수를 출력한다. 즉 pop으로 인한 정수값을 출력합니다 만약 스택의 배열이 비워있다면 -1을 출력 합니다.
- 풀이(3) : size 스택에 있는 원소의 개수를 출력하면 됩니다 ^^
- 풀이(4) : empty 스택이 비어있는지의 여부를 확인 합니다 만약 비어있으면 1을 출력하고 그렇지 않으면 0을 출력하면 됩니다
- 풀이(5) : top 스택의 가장 위에 정수를 출력합니다. 이부분에서 popLas t를 사용하면 안됩니다 popLast같은 경우 그 마지막 원소를 pop즉 제거까지 시키기 때문에 endIndex로 접근하시는게 좋은 방법 같습니다 ^^

```swift
struct Stack {
    var stack: [Int] = []
    
    mutating func push(x: Int) {
        stack.append(x)
    }
    
    mutating func pop() {
        if stack.isEmpty {
            print("-1")
        } else {
            print("\(stack.popLast()!)")
        }
    }
    
    mutating func size() {
        print(stack.count)
    }
    
    mutating func empty() {
        if stack.isEmpty {
            print("1")
        } else {
            print("0")
        }
    }
    
    mutating func top() {
        if stack.isEmpty {
            print("-1")
        } else {
            print("\(stack[stack.endIndex - 1])")
        }
    }
}


var N = Int(readLine()!)!
var jennyStack = Stack()

for i in 0...N - 1 {
    var call = readLine()!.split(separator: " ").map{$0}
    if call[0] == "push" {
        jennyStack.push(x: Int(call[1])!)
    } else if call[0] == "pop" {
        jennyStack.pop()
    } else if call[0] == "size" {
        jennyStack.size()
    } else if call[0] == "empty" {
        jennyStack.empty()
    } else if call[0] == "top" {
        jennyStack.top()
    }
}
```


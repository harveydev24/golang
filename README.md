# Go

## 기본 구조

```go
package main

func main() {
  
}
```



## 변수와 상수

```go
//변수
var name string = "harvey"
name := "harvey"

//상수
const name string = "harvey"
```



## 함수

### 기본 형태

```go
func myFunc(name string) int {
  return len(name)
}
```

- 다음과 같이 여러 개의 반환값을 가질 수 있음

  ```go
  func lenAndUpper(name string) (int, string) {
    length := len(name) 
    upper := strings.ToUpper(name)
    return length, upper
  }
  ```

- 다음과 같이 여러 개의 인자를 받을 수 있음

  ```go
  func add(n ...int) int {
    return n[0]
  }
  ```

  

### Naked Return

- 반환 값을 미리 지정할 수 있음

```go
func lenAndUpper(name string) (length int, upper string) {
  length = len(name) // := 아님에 주의. length는 이미 위에서 생성
  upper = strings.ToUpper(name)
  return
}
```



### Defer

- 함수가 끝난 직후 실행되는 라인

```go
func myFunc(name string) int {
  defer fmt.Println("Done!") // Println과 같이 대문자로 시작하는 함수는 다른 패키지에서export되어 import한 함수
  return len(name)
}
```



## 반복문

- go에는 오로지 for만 존재

  ```go
  func superAdd(numbers ...int) int {
    for i:=0; i<len(numbers); i++{
      fmt.Println(numbers[i])
    }
    return 1
  }
  
  func main() {
    superAdd(1,2,3,4,5,6)
  }
  
  /*
  1
  2
  3
  4
  5
  6
  */
  ```

  

- range는 기본적으로 인덱스를 줌

  ```go
  func superAdd(numbers ...int) int {
    for number := range numbers {
      fmt.Println(number)
    }
    return 1
  }
  
  func main() {
    superAdd(1,2,3,4,5,6)
  }
  
  /*
  0
  1
  2
  3
  4
  5
  */
  ```

- range는 다음과 같이 파이썬의 enumerate와 비슷하게 쓸 수도 있음

  ```go
  func superAdd(numbers ...int) int {
    for idx, number := range numbers {
      fmt.Println(number)
    }
    return 1
  }
  
  func main() {
    superAdd(1,2,3,4,5,6)
  }
  
  /*
  0 1
  1 2
  2 3
  3 4
  4 5
  5 6
  */
  ```

- sum 같은 기본 내장함수가 없어서 구현해서 써야함

  ```go
  func superAdd(numbers ...int) int {
    total := 0
    // _는 값을 무시함
    for _, number := range numbers {
    	total += number
    }
    return total
  }
  
  func main() {
    result := superAdd(1,2,3,4,5,6)
    fmt.Println(result)
  }
  
  // 21
  ```



## 조건문

### if문

```go
func canIDrink(age int) bool {
  if age < 18 {
    return false
  }
  return true
}

func main() {
  fmt.Println(canIDrink(16))
}

// false
```



- Variable expression

  - if문 만을 위한 변수를 생성할 수 있음

    ```go
    func canIDrink(age int) bool {
      if koreanAge := age + 2; koreanAge < 18 {
        return false
      }
      return true
    }
    ```

    

### swith문

- 복잡한 else-if의 반복을 피할 수 있음
- if문과 동일하게 variable expression 사용 가능

```go
func canIDrink(age int) bool {
  swith {
    case age < 18:
    	return false
    case age == 18:
    	return true
    case age > 50:
    	return false	
  }
  return false
}
```



## 메모리

- 포인터

  ```go
  func main() {
    a := 2
    b := a // 값을 복사
    a = 10
    fmt.Println(a,b)
  }
  
  // 10 2
  
  func main() {
    a := 2
    b := a // 값을 복사
    fmt.Println(&a,&b)
  }
  
  // 0xc0000b2008 0xc0000b2010
  
  func main() {
    a := 2
    b := &a
    fmt.Println(&a,b)
  }
  
  // 0xc0000b2008 0xc0000b2008
  
  func main() {
    a := 2
    b := &a
    fmt.Println(a,*b)
  }
  
  // 2 2
  
  func main() {
    a := 2
    b := &a
    *b = 20
    fmt.Println(a,*b)
  }
  
  // 20 20
  ```

  

## 배열

- array

  - 길이를 지정해주어야 함

    ```go
    func main() {
      names := [5]string{"a","b","c"}
      names[3] = "d"
      names[4] = "e"
      fmt.Println(names)
    }
    
    // [a b c d e]
    ```

- slice

  - 길이를 지정해 줄 필요 없음

  - 자유롭게 요소 추가 삭제 가능

    ```go
    func main() {
      names := []string{"a","b","c","d","e"}
      names = append(names, "f")
      fmt.Println(names)
    }
    
    // [a b c d e f]
    ```



# Map

- 파이썬의 딕셔너리와 유사

  ```go
  func main() {
    // 키와 밸류 존재
    // map[key]value
    nico := map[string]string{
      "name": "harvey",
      "age": "12" // 숫자가 올 수 없음
    }
    fmt.Println(nico)
  }
  
  // map[age:12 name:harvey]
  ```



- 반복문에 사용 가능

  ```go
  func main() {
    nico := map[string]string{
      "name": "harvey",
      "age": "12"
    }
    for _, value := range nico {
      fmt.Println(value)
    }
  }
  
  /*
  harvey
  12
  */
  ```

  

## Struct

- go에는 클래스나 오브젝트가 없음

- 대신 객체와 유사한 struct

- struct에는 constructor가 없음(파이썬 init같은거)

  ```go
  type person struct {
    name string
    age int
    favFood []string
    
  }
  
  func main() {
    favFood := []string{"kimchi","pizza"}
    harvey := person{
      "harvey",
      18,
      favFood
    }
    fmt.Println(harvey.name)
  }
  
  // harvey
  ```

- 좀 더 명확하게 다음과 같이 사용

  ```go
  func main() {
    favFood := []string{"kimchi","pizza"}
    harvey := person{
      name: "harvey",
      age: 18,
      favFood: favFood
    }
    fmt.Println(harvey.name)
  }
  ```

  


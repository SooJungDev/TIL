## 쉽고 빠른 Go 시작하기 듣고 정리

- 코드는 무조건 go/src 에 위치해있어야함
- 로컬에 Go PATH가 존재해야함
- main 은 컴파일을 위해서 필요한것

### Packages and Imports
- export 하려면 대문자로 해줘야함 
```go
package something

import "fmt"

func sayBye() {
	fmt.Println("Bye")
}

func SayHello() {
	fmt.Println("Hello")
}

```


### Variables and Constants
- const 한번 할당하면 바꿀수 없다
- 축약해서 사용할 경우 go 가 알아서 타입을 찾아서 넣어준다
```go
	const name string = "nico"
	fmt.Println(name)

	//var name2 string = "nico"
	name2 := "nico"
	name2 = "lynn"
	fmt.Println(name2)
```

### function
- go 는 function 에 여러 리턴 값을 반환 할 수 있다
- 무시하고 싶으면 _ 사용하면된다.
```go
func lenAndUpper(name string) (int, string) {
    return len(name), strings.ToUpper(name)
}

func main() {
    tatalLenght, upperName := lenAndUpper("soojung")
    fmt.Println(tatalLenght, upperName)

}

```

- 여러개의 인자를 받을 경우 아래와 같이
```go
func repeatMe(words ...string) {
	fmt.Println(words)
}

func main() {
	repeatMe("gkgk", "soo", "dd", "aaa")

}
```

- naked return
- return할 variable 을 굳이 명시하지 않아도됨
- go 에서 자동으로 리턴해줌
```go
func lenAndUpper(name string) (length int, uppercase string) {
	length = len(name)
	uppercase = strings.ToUpper(name)
	return
}

func main() {
	tatalLenght, upperName := lenAndUpper("soojung")
	fmt.Println(tatalLenght, upperName)
}
```

- defer
- function 이 끝났을때 실행시켜줌
```go
func lenAndUpper(name string) (length int, uppercase string) {
	defer fmt.Println("I'm done")
	length = len(name)
	uppercase = strings.ToUpper(name)
	return
}

func main() {
	tatalLenght, upperName := lenAndUpper("soojung")
	fmt.Println(tatalLenght, upperName)
}
```

### for, range, ...args
```go
func superAdd(numbers ...int) int {
	total := 0
	for _, number := range numbers {
		total += number
	}
	return total
}

func main() {
	result := superAdd(1, 2, 3, 4, 5, 6)
	fmt.Println(result)
}
```

###  If with a Twist 
- if 문을 위한 variable 을 생성하고 쓸수 있다는점이 매력적임
```go
func canIDrink(age int) bool {
	if koreanAge := age + 2; koreanAge < 18 {
		return false
	}
	return true

}

func main() {
	fmt.Println(canIDrink(17))
}
```

### switch
```go
func canIDrink(age int) bool {
	switch koreanAge := age + 2; koreanAge {
	case 10:
		return false
	case 18:
		return true
	}
	return false
}

func main() {
	fmt.Println(canIDrink(17))
}
```

### Pointers!
- & 주소를 뜻
- * 살펴보는것
- *와 주소를 함께 사용하면 값을 변경 할 수 있다.
```go
func main() {
	a := 2
	b := &a
	*b = 20
	fmt.Println(a)
}
```

###  Arrays and Slices
- 대부분 Slices 를 사용함 길이 제약 X
- append 는 기존값을 수정하는게 아니고 새로운 slices를 리턴해줌
```go
func main() {
	names := []string{"nico", "lynn", "dal"}
	names = append(names, "flynn")
	fmt.Println(names)
}
```

### map
- key, value 값으로 이루어져잇음
```go
func main() {
	soojung := map[string]string{"name": "soojung", "age": "12"}
	for key, value := range soojung {
		fmt.Println(key, value)
	}
}
```

### Structs
```go
type person struct {
	name    string
	age     int
	favFood []string
}

func main() {
	favFood := []string{"kimchi", "ramen"}
	soojung := person{name: "soojung", age: 31, favFood: favFood}
	fmt.Println(soojung)
}
```


## 참고
- [쉽고 빠른 Go 시작하기](https://nomadcoders.co/go-for-beginners)
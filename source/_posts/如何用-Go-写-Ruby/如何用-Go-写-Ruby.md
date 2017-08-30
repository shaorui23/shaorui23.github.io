---
layout: post
title: 如何用 Go 写 Ruby
date: 2016-07-12 10:45:04
tags:
---

<span itemprop="articleBody">

Go 学习笔记，持续更新。 原文请参考: [http://kimh.github.io/blog/en/go/how-can-i-do-x-in-ruby-with-go-part-1/](http://kimh.github.io/blog/en/go/how-can-i-do-x-in-ruby-with-go-part-1/)

### [](#Create-Array "Create Array")Create Array

Ruby:  

```
numbers = [1,2,3]
fruits = ["apple", "banana", "grape"]
```

Go:

```
numbers := []int{1,2,3}
fruits := []string{"apple", "banana", "grape"}
```


### [](#Append-an-element-to-array "Append an element to array")Append an element to array

Ruby:  

```
numbers = [1,2,3]
numbers << 4
```


Go:  

```
numbers := []int{1,2,3}
numbers.append(numbers, 4)
```


### [](#Concatenate-arryas "Concatenate arryas")Concatenate arryas

Ruby:  

```
numbers1 = [1,2,3]
numbers2 = [4,5,6]
numbers1 = numbers1 + numbers2
```


Go:  

```
numbers1 := []int{1,2,3}
numbers2 := []int{4,5,6}
numbers1 = append(numbers1, numbers2...)
```

Go:  

```
numbers1 := []int{1,2,3}
numbers1 = append(numbers1, 4, 5, 6)
```


### [](#Create-multi-dimension-array "Create multi dimension array")Create multi dimension array

Ruby:  

```
multi_array = [[1,2,3],[4,5,6]]
```

Go:  

```
multi_array := [][]int{{1, 2, 3}, {4,5,6}}
```


### [](#Create-empty-array "Create empty array")Create empty array

Ruby:  

```
array = []
```


Go:  


```
array := []int
```


### [](#Iterate-an-array "Iterate an array")Iterate an array

Ruby:  

```
numbers = [1,2,3]
numbets.each {|num| puts num }
```

Go:  

```
numbers := []int{1,2,3}

for _, num := range numbers {
  fmt.Println(num)
}
```

### [](#Looping-N-times "Looping N times")Looping N times

Ruby:  

```
5.times {|num| puts num}
```

Go:  

```
for num := 0; num < 5; num ++ {
  fmt.Println(num)
}
```

### [](#Clone-array "Clone array")Clone array

Ruby:  

```
new_array = old_array.clone
```


Go:  

```
new_array := make([]int, len(old_array))

copy(new_array, old_array)
```

### [](#Accessing-elements-of-array-by-range "Accessing elements of array by range")Accessing elements of array by range

Ruby:  

```
numbers = [1,2,3,4]
numbers[0..3]
```


Go:  

```
go_array := []int{1,2,3,4}
go_array[0:4]
```


### [](#Compare-array "Compare array")Compare array

Ruby:  

```
if array1 == array2
  puts "Same array"
end
```


Go:  

```
go_array := []int{1, 2, 3, 4, 5, 6, 7}
bb_array := []int{3, 2, 3, 4, 5, 6, 7}

same := true

for i, ele := range go_array {
  if bb_array[i] != ele {
    same = false
    break
  }
}

if same == true {
  fmt.Println("Same slice")
}
```

### [](#Check-if-array-include-an-element "Check if array include an element")Check if array include an element

Ruby:  

```
fruits = ["apple", "banana", "grape"]

if fruits.include?("apple")
  puts "include!"
end
```

Go:  

```
include := false
fruits := []string{"apple", "banana", "grape"}

for _, ele := range fruits {
  if ele == "apple" {
    include = true
    break
  }
}

if include == true {
  fmt.Println("Included")
}
```

### [](#Define-a-method-with-variable-length-argument "Define a method with variable length argument")Define a method with variable length argument

Ruby:  

```
def foo(*args)
  args.each {|arg| puts arg}
end
```

Go:  

```
func foo(arguments ...int) {
	for _, arg := range arguments {
		fmt.Println(arg)
	}
}

foo(1,2,3,4,5)
```

### [](#Nil-checking "Nil checking")Nil checking

Ruby:  

```
if val.nil?
  puts "val is nil"
end
```

Go:  

```
var str string

if str =="" {
  fmt.Println("str is empty")
}

var i int

if i == 0 {
  fmt.Println("i is zero")
}
```
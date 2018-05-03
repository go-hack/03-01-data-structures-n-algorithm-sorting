
# 03-01-data-structures-n-algorithm-sorting

In this chapter we will be covering sorting. While most modern programming languages already have efficient algorithms implemented for sorting it is important to get a handle on what is its and why it is important.

Sorting things in programming is one of the most basic thing and your computer spend a surprising amount of time sorting things. As you will discover all algorithms is not created equal. There is many ways to skin a cat and having some knowledge about the data your sorting can provide you with some benefits when creating an algorithm, we will however cover onyl generic once at the moment.


## Big-O notation and complexity

In computer science we use what is called the [Big-O](https://en.wikipedia.org/wiki/Big_O_notation) notation to describe how hard a problem is to solve and how fast the complexity grows dependent on the input. We wont cover it here really, but the basics is quite easy to grasp an gives you a good idea of how fast a algorithm will be.

The big-O notation is described as a function O with an input n, e.g. `O(n)` meaning that for n input element the complexity grows linear.

So some examples might be nice here.

```go 
 a := 1
 b := 2
 c := a + b 
// O(1), regardless of the input size,
//       the problem does not grow.
 ```

 ```go 
 data := []int{1,2,3,4}
 sum := 0
 for _, val := range data {
     sum += val
 }
// O(n), if we are doing the sum of all values of the array, 
//       the time it takes to do this grows linearly with the number
//       of inputs
 ```

```go 
data := []int{3,1,2,4}
for i := 0; i < len(data); i++ {
    for j := i; j < len(data); j++ {
        if data[j] < data[i]{
            tmp := data[j]
            data[i] = data[j]
            data[j] = tmp
        }
    }
}
// O(n^2), so the mo this is one of the more naiive solution to
//         sorting an array, its called bubble sort. We have one for 
//         loop inside another and the number of times the most inner
//         code is run is about n*n times where n is the number of
//         elements in the input data and the input gose towards
//         infinity.
 ```


## Sorting
There are many [algorithms](https://en.wikipedia.org/wiki/Category:Sorting_algorithms) out there and we are going to look closer at a few of them. More precise a subset which can be described as in-place-sorting-algorithms. This means that we do not allocate any extra memory in order to sort them. We only compare elements and switch places on the.

### Bubble sort
The most naiiv way of sorting element and a good place to start. Simply put, we traverse through all the elements and find the smallest one, put it in first place. Then we traverse through all but the first element, find the smallest one, and put it in second place. We continue on, until we done it for all subsets.

```go
func bubbleSort(data []int) []int{
    for i := 0; i < len(data); i++ {
        for j := i; j < len(data); j++ {
            if data[j] < data[i]{
                tmp := data[j]
                data[i] = data[j]
                data[j] = tmp
            }
        }
    }
    return data
}
```

## Lets code
I have prepared a lib to get a feeling for how the different algorithms work.

```go
package main

import (
    "time"
    "github.com/go-hack/vsort"
)

func main() {
    r := vsort.Init()
    r.TickTime = time.Millisecond * 1

    r.Run(bubble) // and your function for the algorithm you want to test.
                  // You can add multiple algorithms here to compare them
}

func bubble(ops *vsort.Ops) {
    for i := 0; i < ops.Len(); i++ {
        for j := 1; j < ops.Len()-i; j++ {
            if ops.GreaterThen(j-1, j) {
                ops.Swap(j-1, j)
            }
        }
    }
}

```

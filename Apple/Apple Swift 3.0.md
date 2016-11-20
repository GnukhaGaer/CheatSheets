# Swift 3 Language References

## Completion Handlers

**Example 1**

```swift
func hardProcessingWithString(input: String, completion: (result: String) -> Void) {
	...
	completion("we finished!")
}
```
```swift
hardProcessingWithString("commands") {
	(result: String) in
	print("got back: \(result)")
}
```

**Example 2**

```swift
func getSortedNr(a: Int, b: Int, completion_getSorted: @escaping (_ min: Int, _ max: Int) -> Void){
    
    compare(nr1: a, nr2: b){ (small, big) -> Void in
        print("comparing the numbers")
        completion_getSorted(small, big)
    }
}
```
```swift
func compare(nr1: Int,nr2: Int, completion_compare: (_ smallValue: Int, _ bigValue: Int) -> Void){
    if nr1 < nr2{
        completion_compare(nr1, nr2)
    }
    else{
        completion_compare(nr2, nr1)
    }
}
```
```swift
getSortedNr(a: 6, b: 2) {(min, max) -> Void in
    print("\(min) \(max)")
}
```

**Example 3**

```swift
typealias CompletionHandler = (success:Bool) -> Void

func downloadFileFromURL(url: NSURL,completionHandler: CompletionHandler) {

    // download code.

    let flag = true // true if download succeed,false otherwise

    completionHandler(success: flag)
}

// How to use it.

downloadFileFromURL(NSURL(string: "url_str")!, { (success) -> Void in

    // When download completes,control flow goes here.
    if success {
        // download success
    } else {
        // download fail
    }
})
```










## GCD Grand Central Dispatch

```swift
DispatchQueue.global(qos: .background).async {
    // do some task

    DispatchQueue.main.async {
        update some UI
    }
}
```

## 


func backgroundThread(background: (() -> Void)? = nil, completion: (() -> Void)? = nil){
    DispatchQueue.global(qos: .background).async {
        if(background != nil){ background!(); }

        DispatchQueue.main.async {
            if(completion != nil){ completion!(); }
        }
    }
}

To run a process in the background then run a completion in the foreground:

backgroundThread(background: {
    // Your function here to run in the background
},
completion: {
    // A function to run in the foreground when the background thread is complete
})


## Closures and functions

### Variadic functions

Variadic functions are functions that have a variable number of arguments 
(indicated by ... after the argument's type) that can be accessed into their body as an array.

```swift
func jediBladeColor (colors: String...) -> () {
  for color in colors {
    println("\(color)")
  }
}
jediBladeColor("red","green")
```

### Closures with known types:

When the type of the closure's arguments are known, you can do as follows:

```swift
func applyMutliplication(value: Int, multFunction: Int -> Int) -> Int {
  return multFunction(value)
}

applyMutliplication(2, {value in
  value * 3
})
```

### Closures shorthand argument names:

Closure arguments can be references by position ($0, $1, ...) rather than by name

```swift
applyMutliplication(2, {$0 * 3})
```

Furthermore, when a closure is the last argument of a function, parenthesis can be omitted as such:

```swift
applyMutliplication(2) {$0 * 3}
```

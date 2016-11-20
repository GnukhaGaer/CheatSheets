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

**Better Completion Handlers in Swift**

An enumeration defines a common type for a group of related values and 
enables you to work with those values in a type-safe way within your code.

```swift
class ContentStore {

	typealias CompletionHandlerType = (Result) -> Void
  
	enum Result {
		case Success(AnyObject?)
		case Failure(Error)
	}
  
	enum Error: ErrorType {
		case AuthenticationFailure
	}
  
	func fetch(completionHandler: CompletionHandlerType) {
		let random = Int(arc4random_uniform(7))
		if (random > 4) {
			completionHandler(Result.Success(1))
		} else {
			completionHandler(Result.Failure(Error.AuthenticationFailure))
		}
	}
}
```

The completion handler is defined within a typealias. The completion handler returns the enumeration ‘Result‘. There are two enumeration values ‘Success’ or ‘Failure’. The ‘Success’ value takes an ‘AnyObject’ value and the success an enumeration ‘Error’ which is of type ‘ErrorType’ The ‘fetch’ function defines a random number and uses this value to either return a ‘Success’ or ‘Failure’, ‘Success’ if the random number if greater than 4.

```swift
ContentStore().fetch { (result) -> Void in
	switch (result) {
	case .Success(let number):
		if let unwrappedNumber = number {
			print("\(unwrappedNumber)")
		}
		break;
	case .Failure(let error):
		print("\(error)")
		break;
	}
}
```
The use of an enumeration within the completion handler allows us to use a switch statement to respond to ‘Success’ or ‘Failure’. Once you are within the case either ‘Success’ or ‘Failure’ you can read the passed value.







## GCD Grand Central Dispatch

Most common CGD pattern used to get one of the Background threds:
```swift
DispatchQueue.global(qos: .default).async {
    // Background thread
    DispatchQueue.main.async {
        // UI updates
    }
}
```

Queue attributes(**QOS**):
- `.userInitiated`
- `.default`
- `.utility`
- `.background`

Some hight priority background work:
```swift
DispatchQueue(qos: .userInitiated).async {
    // do some task

    DispatchQueue.main.async {
		// update some UI
    }
}
```

## GCD with Completion Handler

```swift
func loadImage(_ urlString: String, handler:@escaping (_ image:UIImage?)-> Void)
    {
        let imageURL: URL = URL(string: urlString)!

        URLSession.shared.dataTask(with: imageURL) { (data, _, _) in
            if let data = data{
                handler(UIImage(data: data))
            }
        }.resume()
    }
```
Call func like this:
```swift
loadImage("SomeURL") { (image) -> Void in
	if let image = image{
		DispatchQueue.main.async {
			self.imageView.image = image
		}
	}
}
```

**A function for the background work:**
```swift
func backgroundThread(background: (() -> Void)? = nil, completion: (() -> Void)? = nil){
    DispatchQueue.global(qos: .background).async {
        if(background != nil){ background!(); }

        DispatchQueue.main.async {
            if(completion != nil){ completion!(); }
        }
    }
}
```

To run a process in the background then run a completion in the foreground:

```swift
backgroundThread(background: {
    // Your function here to run in the background
},
completion: {
    // A function to run in the foreground when the background thread is complete
})
```


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

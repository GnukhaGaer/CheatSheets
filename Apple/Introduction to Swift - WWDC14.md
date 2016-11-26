# Introduction to Swift

##### Table of contents
[Variables and constants](#variables-and-constants)  
[Array and Dictionary](#array and dictionary)
[Loops](#loops)  
[Optionals](#optionals)  


## Variables and constants
### Inferred type
```swift
var languageName = "Swift" // variable languageName inferred as String
let languageName = "Swift" // constant
```
### Not inferred type
```swift
var languageName: String = "Swift" // variable
let languageName: String = "Swift" // constant
```

## Array and Dictionary

### Creating an empty array and dictionary
```swift
var names = [String]()
var numberOfLegs = [String: Int]()
```
### Creating an array and a dictionary containing items
```swift
var names = ["Anna", "Alex", "Brian", "Jack"]
var numberOfLegs = ["ant": 6, "snake": 0, "cheetah": 4]
```

## Loops
### While
```swift
while !done {
    doWork()
}
```
### For
```swift
for var doctor = 1; doctor <= 13; ++doctor {
    exterminate(doctor)
}
```
### For-In: Strings
```swift
for character in "My string" {
  print(character)
}
```
### For-In: Ranges
```swift
for number in 0...3 { // 1...3 will execute 4 times. But 0..3 will execute only 3 times (0, 1 and 2)
  print(character)
}
```
### For-In: Dictionaries
```swift
for (animalName, legCount) in numberOfLegs {
    println("\(animalName)s have \(legCount) legs")
}
```

## Optionals
### Unwrapping an Optional
```swift
if let legCount = possibleLegCount {
    println("An aardvark has \(legCount) legs")
}
```  




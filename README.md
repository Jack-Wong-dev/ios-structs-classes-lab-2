# Structs and Classes Lab - 2


## Question 1

Using the Room struct below, write code that demonstrates that it is a value type.

```swift
struct Room {
     let maxOccupancy: Int
     let length: Double
     let width: Double
}

//Solution Q1

var room1 = Room(maxOccupancy: 10, length: 10, width: 10)
var room2 = room1

//0x000000011f39faa0  - Memory address of room1  (On my computer at the time of execution)
print(Unmanaged<AnyObject>.fromOpaque(&room1).toOpaque())  //prints memory address where instance room1 is stored

//0x000000011f39fab8  - Memory address of room2 (On my computer at the time of execution)
print(Unmanaged<AnyObject>.fromOpaque(&room2).toOpaque()) //prints memory address where instance room2 is stored

//Checking if both instances share the same memory address
print(Unmanaged<AnyObject>.fromOpaque(&room1).toOpaque() == Unmanaged<AnyObject>.fromOpaque(&room2).toOpaque())

```

## Question 2

Using the Bike class below, write code that demonstrates that it is a reference type.

```swift
class Bike {
    var wheelNumber = 2
    var hasBell = false
}
```

## Question 3

a. Given the Animal class below, create a Bird subclass with a new `canFly` property.

```swift
class Animal {
    var name: String = ""
    var printDescription() {
        print("I am an animal named \(name)")
    }
}
```

b. Override the printDescription method to have the instance of the Bird object print out its name and whether it can fly


c. Create a Robin subclass of Bird.  Its name should always be "Robin".

```Swift
//Q3 Part A, B, C
class Animal {
    var name: String = ""
    func printDescription() {
    print("I am an animal named \(name)")
    }
    init(name:String) {
        self.name = name
    }
}

class Bird: Animal{
    var canFly: Bool
    override func printDescription() {
        print("I am an bird named \(name) and I \(canFly ? "can" : "can't") fly" )
    }
    init(name: String, canFly: Bool) {
        self.canFly = canFly
        super.init(name: name)
    }
}

var budgie = Bird.init(name: "Articuno", canFly: true)
budgie.printDescription()

class Robin: Bird{

    override init(name: String, canFly: Bool) {
        super.init(name: name, canFly: canFly)
        self.name = "Robin"
    }
}

let birb = Robin(name: "meh", canFly: true)

print(birb.name)
birb.name = "Bird Brain"

```

## Question 4

class Bike {
  let wheelNumber = 2
  let wheelWidth = 1.3
  var hasBell = true
  func ringBell() {
    if hasBell {
      print("Ring!")
    }
  }
}


## Question 5

a. Given the Point object below, complete the `distance method` so that it returns the distance between a given point.

The equation for the distance formula can be found [here](https://www.mathsisfun.com/algebra/distance-2-points.html).

`sqrt` is a method in Swift that gives the square root.  Make sure to have `import Foundation` or `import UIKit` to use this method.

```swift
struct Point {
    let x: Double
    let y: Double
    func distance(to point: Point) -> Double {

    }
}

let pointOne = Point(x: 0, y: 0)
let pointTwo = Point(x: 10, y: 10)

print(pointOne.distance(to: pointTwo)) //Prints 14.142135623730951
```


b. Given the above Point object, and Circle object below, add a `contains` method that returns whether or not a given point is on the circle

```swift
struct Circle {
    let radius: Double
    let center: Point
}

let pointOne = Point(x: 0, y: 0)
let circleOne = Circle(radius: 5, center: pointOne)
let pointTwo = Point(x: 5, y: 0)
let pointThree = Point(x: 4, y: 0)
let pointFour = Point(x: sqrt(12.5), y: sqrt(12.5))
circleOne.contains(pointTwo) //true
circleOne.contains(pointThree) // false
circleOne.contains(pointFour) //true
```

c. Add another method to `Circle` that returns a random point on the circle

Hint: Given the radius of a circle and the x value of a point on the circle, the y value of the point is defined by:

```7
√(r^2) - (x^2)
```

```swift
circleOne.contains(circleOne.getRandomPoint()) //Should always be true
```

```swift

struct Circle {
    let radius: Double
    let center: Point
    init(radius: Double, center: Point) {
        self.radius = radius
        self.center = center
    }
    func contains(_ point: Point) -> Bool{
        if center.distance(to: point) <=  radius {
            return true
        }else{
            return false
        }
    }
}


let pointOne = Point(x: 0, y: 0)
let circleOne = Circle(radius: 5, center: pointOne)
let pointTwo = Point(x: 5, y: 0)
let pointThree = Point(x: 4, y: 0)
let pointFour = Point(x: sqrt(12.5), y: sqrt(12.5))
let pointFive = Point(x: 10, y: 10)
circleOne.contains(pointTwo) //true
circleOne.contains(pointThree) // true
circleOne.contains(pointFour) //true
circleOne.contains(pointFive) //false

```


## Question 6

a. Create a struct called HangmanModel with 3 properties `targetWord: String`, `numberOfIncorrectGuesses: Int` and `guessedLetters: [Character]`.

b. Add a method called `playerWon` that returns whether all of the characters in `targetWord` are in `guessedLetters`

var model = HangmanModel()
model.targetWord = "hello"
model.guessedLetters = ["h","e","o","l"]
model.playerWon //true

```swift

struct HangmanModel{
    var targetWord: String = ""
    var numberOfImncorrectGuesses: Int = 0
    var guessedLetters: [Character] = []
    func playerWon() -> Bool {
        for c in targetWord where !(guessedLetters.contains(c)){
            return false
        }
        return true
    }
}
//b. Add a method called playerWon that returns whether all of the characters in targetWord are in guessedLetters

var model = HangmanModel()
model.targetWord = "hello"
model.guessedLetters = ["h","e","o","l"]
print(model.playerWon()) //true
```

c. Add a method called `printDisplayVersionOfWord` that prints the `targetWord` replacing characters that are not in `guessedLetters` with "_"

var model = HangmanModel()
model.targetWord = "hello"
model.guessedLetters = ["h","l"]
model.printDisplayVersionOfWord
//prints h_ll_

d. Add a method called `guess(_:)` that takes in a character as input, and updates `guessedLetters` and `numberOfIncorrectGuesses` as appropriate.

var model = HangmanModel()
model.targetWord = "hello"
model.guess("h")
model.guess("a")
model.guessedLetters // ["h", "a"]
model.numberOfIncorrectGuesses // 1

e. Have `guess(_:)` also print out the current display version of the word, the number of incorrect guesses and if the player has won.

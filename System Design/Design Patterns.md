- **Creational:** Helps solve issues related to creating and managing new objects in JS.
- **Structural:** Designed to solve problems related to handling the schema/structure of JavaScript objects.
- **Behavioral:** Helps tackle problems related to passing the control and responsibility between various objects in JavaScript.
# Singleton pattern
(using object literal)
```Javascript
const Config = {
  start: () => console.log('App has started'),
  update: () => console.log('App has updated'),
}

// We freeze the object to prevent new properties being added and existing properties being modified or removed
Object.freeze(Config)

Config.start() // "App has started"
Config.update() // "App has updated"

Config.name = "Robert" // We try to add a new key
console.log(Config) // And verify it doesn't work: { start: [Function: start], update: [Function: update] }
```
consists of an object that can't be copied or modified.
# Factory Method
creating objects that can be modified after creation.
```javascript
class Alien {
    constructor (name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}

const alien1 = new Alien("Ali", "I'm Ali the alien!")
console.log(alien1.name) // output: "Ali"
```
# Abstract Factory 
an abstract factory the client interacts with. That **abstract factory** calls the corresponding **concrete factory** given the corresponding logic. And that concrete factory is the one that returns the end object.

it just adds an abstraction layer over the factory method pattern, so that we can create many different types of objects, but still interact with a single factory function or class.

```javascript
// And and abstract factory that works as a single point of interaction for our clients
// Given the type parameter it receives, it will call the corresponding concrete factory
const vehicleFactory = {
    createVehicle: function (type) {
        switch (type) {
            case "car":
                return new Car()
            case "truck":
                return new Truck()
            case "motorcycle":
                return new Motorcycle()
            default:
                return null
        }
    }
}
```
# Builder 
Builds the object in steps.
The cool thing about this pattern is that we separate the creation of properties and methods into different entities.
Create an object using properties of a factory class, then use Builder to customize it.

Related to Object Composition in JavaScript.
```javascript
// We declare our objects
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

// These functions take an object as parameter and add a method to them
const addFlyingAbility = obj => {
    obj.fly = () => console.log(`Now ${obj.name} can fly!`)
}

// Finally we call the builder functions passing the objects as parameters
addFlyingAbility(bug1)
bug1.fly() // output: "Now Buggy McFly can fly!"
```
# Prototype
Prototypal Inheritance - create an object using another object as blueprint, inheriting its properties and methods.
Same as using classes but more flexibility as sharing happens without having to be of same class.
```javascript
// We declare our prototype object with two methods
const enemy = {
    attack: () => console.log("Pim Pam Pum!"),
    flyAway: () => console.log("Flyyyy like an eagle!")
}

// We declare another object that will inherit from our prototype
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

// With setPrototypeOf we set the prototype of our object
Object.setPrototypeOf(bug1, enemy)

// With getPrototypeOf we read the prototype and confirm the previous has worked
console.log(Object.getPrototypeOf(bug1)) // { attack: [Function: attack], flyAway: [Function: flyAway] }

console.log(bug1.phrase) // Your debugger doesn't work with me!
console.log(bug1.attack()) // Pim Pam Pum!
console.log(bug1.flyAway()) // Flyyyy like an eagle!
```
# Adapter (start of Structural design)
Helps 2 objects with incompatible (expecting different I/O types) interfaces to interact with each other 
```javascript
// Our array of cities
const citiesHabitantsInMillions = [
    { city: "London", habitants: 8.9 },
    { city: "Paris", habitants: 2.1 },
] 

// The new city we want to add
const BuenosAires = {
    city: "Buenos Aires",
    habitants: 3100000
}

// Our adapter function takes our city and converts the habitants property to the same format all the other cities have
const toMillionsAdapter = city => { city.habitants = parseFloat((city.habitants/1000000).toFixed(1)) }

toMillionsAdapter(BuenosAires)

// We add the new city to the array
citiesHabitantsInMillions.push(BuenosAires)

// And this function returns the largest habitants number
const MostHabitantsInMillions = () => {
    return Math.max(...citiesHabitantsInMillions.map(city => city.habitants))
}

console.log(MostHabitantsInMillions()) // 8.9
```
# Decorator 
The **Decorator** pattern lets you attach new behaviors to objects by placing them inside wrapper objects that contain the behaviors.
```javascript
import { useState } from 'react'
import Context from './Context'

const ContextProvider: React.FC = ({children}) => {

    const [darkModeOn, setDarkModeOn] = useState(true)
    const [englishLanguage, setEnglishLanguage] = useState(true)

    return (
        <Context.Provider value={{
            darkModeOn,
            setDarkModeOn,
            englishLanguage,
            setEnglishLanguage
        }} >
            {children}
        </Context.Provider>
    )
}

export default ContextProvider
```
Then we wrap the whole application around it.
And later on, using the `useContext` hook I can access the state defined in the Context from any of the components in my app.
# Facade
The **Facade** pattern provides a simplified interface to a library, a framework, or any other complex set of classes.
example: `map`, `sort`, `reduce` and `filter` which work as for loops usually.
# Proxy
The **Proxy** pattern provides a substitute or placeholder for another object. The idea is to control access to the original object, performing some kind of action before or after the request gets to the actual original object.

A function `authenticateToken` receives the token as parameter, and once it's done it calls the `next()` function. This function is a middleware, and we can use it in any endpoint of our API in the following way. We just place the middleware after the endpoint address and before declaration of the endpoint function.

In this way, if no token or a wrong token is provided, the middleware will return the corresponding error response. If a valid token is provided, the middleware will call the `next()` function and the endpoint function will get executed next.
# Chain of Responsibility (start of Behavioral)
The **Chain of Responsibility** passes requests along a chain of handlers. Each handler decides either to process the request or to pass it to the next handler in the chain.
same example as middleware
# Iterator
Used to traverse elements of a collection.
any of the JavaScript built in functions we have at our disposal to iterate over data structures (`for`, `forEach`, `for...of`, `for...in`, `map`, `reduce`, `filter`, and so on) are examples of the iterator pattern.
# Observer
The **observer** pattern lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing. Basically, it's like having an event listener on a given object, and when that object performs the action we're listening for, we do something.

If we declare any variables within the dependency array, the function will execute only when those variables change.

```javascript
  useEffect(() => { console.log('var1 has changed') }, [var1])
```


- [Swift](#swift)
	- [Сlosures and functions](#closures-and-functions)
	- [How Do I Declare a Closure in Swift?](#how-do-i-declare-a-closure-in-swift)
	- [Generics](#generics)
	- [Что такое протокол-ориентированное программирование? Как оно связано со Swift? Чем протоколы Swift отличаются от протоколов Objective-C?](#protocols)
	- [Difference between Array VS NSArray VS |AnyObject|](#array-nsarray-anyobject)
	- [Objective-C id is Swift Any or AnyObject](#id-any-anyobject)
	- [ValueType vs. ReferenceType](#valuetype-vs-referencetype)
	- [What is copy on write mechanism](#copy-on-write)
	- [RxSwift](#rxswift)
	- [SwiftUI](#swiftui)
	- [Combine](#combine)

<a name="swift"></a>
# Swift
* Multi-paradigm: protocol-oriented, object-oriented, functional, imperative, block structured
* Designed by	Chris Lattner and Apple Inc.
* First appeared:	June 2, 2014
* Stable release:	5.6.1 / April 8, 2022
* Typing discipline:	Static, strong, inferred
* OS: Darwin, Linux, FreeBSD
* Influenced by C#, CLU, D, Haskell, Objective-C, Python, Ruby, Rust

Swift is a multi-paradigm, compiled programming language created by Apple Inc. for iOS, OS X, watchOS and tvOS development. Swift is designed to work with Apple's Cocoa and Cocoa Touch frameworks and the large body of existing Objective-C code written for Apple products. Swift is in-tended to be more resilient to erroneous code ("safer") than Objective-C and also more concise. It is built with the LLVM compiler framework included in Xcode 6 and later and uses the Objective-C runtime, allowing C, Objective-C, C++ and Swift code to run within a single program.
Swift supports the core concepts that made Objective-C flexible, notably dynamic dispatch, wide-spread late binding, extensible programming, and similar features. These features also have well known performance and safety trade-offs, which Swift was designed to address. For safety, Swift introduced a system that helps address common programming errors like null pointers, as well as introducing syntactic sugar to avoid the pyramid of doom that can result. For performance issues, Apple has invested considerable effort in aggressive optimization that can flatten out method calls and accessors to eliminate this overhead. More fundamentally, Swift has added the concept of protocol extensibility, an extensibility system that can be applied to types, structs and classes, Apple promotes this as a real change in programming paradigms they refer to as "protocol-oriented programming".

<a name="closures-and-functions"></a>
## Сlosures and functions
```swift
()->()
```
Closures in Swift are similar to blocks in C and Objective-C.
Closures are first-class objects, so that they can be nested and passed around (as do blocks in Objective-C). In Swift, functions are just a special case of closures.

_Defining a function:_

You define a function with the `func` keyword. Functions can take and return none, one or multiple parameters (tuples).
Return values follow the `->` sign.
```swift
func jediGreet(name: String, ability: String) -> (farewell: String, mayTheForceBeWithYou: String) {
	return ("Good bye, \(name).", " May the \(ability) be with you.")
}
```
_Calling a function:_
```swift
let retValue = jediGreet("old friend", "Force")
println(retValue)
println(retValue.farewell)
println(retValue.mayTheForceBeWithYou)
```
_Function types_

Every function has its own function type, made up of the parameter types and the return type of the function itself.
For example the following function:
```swift
func sum(x: Int, y: Int) -> (result: Int) { return x + y }
```
has a function type of:
```swift
(Int, Int) -> (Int)
```
Function types can thus be used as parameters types or as return types for nesting functions.

_Passing and returning functions_

The following function is returning another function as its result which can be later assigned to a variable and called.
```swift
func jediTrainer() -> ((String, Int) -> String) {
	func train(name: String, times: Int) -> (String) {
		return "\(name) has been trained in the Force \(times) times"
  	}
  	return train
}
let train = jediTrainer()
train("Obi Wan", 3)
```
_Variadic functions_

Variadic functions are functions that have a variable number of arguments (indicated by `...` after the argument's type) that can be accessed into their body as an array.
```swift
func jediBladeColor(colors: String...) -> () {
	for color in colors {
		println("\(color)")
  	}
}
jediBladeColor("red","green")
```

__Closures__
```swift
{()->() in}
```
_Defining a closure:_

Closures are typically enclosed in curly braces `{ }` and are defined by a function type `() -> ()`, where `->` separates the arguments and the return type, followed by the `in` keyword which separates the closure header from its body.
```swift
{ (params) -> returnType in
  statements
}
```
An example could be the map function applied to an Array:
```swift
let padawans = ["Knox", "Avitla", "Mennaus"]
padawans.map({(padawan: String) -> String in
	"\(padawan) has been trained!"
})
```

_Closures with known types:_
When the type of the closure's arguments are known, you can do as follows:
```swift
func applyMutliplication(value: Int, multFunction: Int -> Int) -> Int {
	return multFunction(value)
}

applyMutliplication(2, {value in
	value * 3
})
```

_Closures shorthand argument names:_

Closure arguments can be references by position `($0, $1, ...)` rather than by name
```swift
applyMutliplication(2, {$0 * 3})
```
Furthermore, when a closure is the last argument of a function, parenthesis can be omitted as such:
```swift
applyMutliplication(2) {$0 * 3}
```

<a name="how-do-i-declare-a-closure-in-swift"></a>
## How Do I Declare a Closure in Swift?
_As a variable:_
```swift
var closureName: (parameterTypes) -> (returnType)
```
_As an optional variable:_
```swift
var closureName: ((parameterTypes) -> (returnType))?
```
_As a type alias:_
```swift
typealias closureType = (parameterTypes) -> (returnType)
```
_As a constant:_
```swift
let closureName: closureType = { ... }
```
_As an argument to a function call:_
```swift
func({(parameterTypes) -> (returnType) in statements})
```
_As a function parameter:_
```swift
array.sort({ (item1: Int, item2: Int) -> Bool in return item1 < item2 })
```
_As a function parameter with implied types:_
```swift
array.sort({ (item1, item2) -> Bool in return item1 < item2 })
```
_As a function parameter with implied return type:_
```swift
array.sort({ (item1, item2) in return item1 < item2 })
```
_As the last function parameter:_
```swift
array.sort { (item1, item2) in return item1 < item2 }
```
_As the last parameter, using shorthand argument names:_
```swift
array.sort { return $0 < $1 }
```
_As the last parameter, with an implied return value:_
```swift
array.sort { $0 < $1 }
```
_As the last parameter, as a reference to an existing function:_
```swift
array.sort(<)
```
_As a function parameter with explicit capture semantics:_
```swift
array.sort({ [unowned self] (item1: Int, item2: Int) -> Bool in
	return item1 < item2
})
```
_As a function parameter with explicit capture semantics and inferred parameters / return type:_
```swift
array.sort({ [unowned self] in return item1 < item2 })
```

<a name="generics"></a>
## Generics

Generic code enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define. You can write code that avoids duplication and expresses its intent in a clear, abstracted manner.
Generics are one of the most powerful features of Swift, and much of the Swift standard library is built with generic code. In fact, you’ve been using generics throughout the Language Guide, even if you didn’t realize it. For example, Swift’s Array and Dictionary types are both generic collections. You can create an array that holds Int values, or an array that holds String values, or indeed an array for any other type that can be created in Swift. Similarly, you can create a dictionary to store values of any specified type, and there are no limitations on what that type can be.
```swift
func swapTwoValues<T>(inout a: T, inout _ b: T) {
	let temporaryA = a
	a = b
	b = temporaryA
}
```

<a name="protocols"></a>
## Что такое протокол-ориентированное программирование? Как оно связано со Swift? Чем протоколы Swift отличаются от протоколов Objective-C?

Protocol-Oriented Programming is a new programming paradigm ushered in by Swift 2.0. In the Protocol-Oriented approach, we start designing our system by defining protocols. We rely on new concepts: protocol extensions, protocol inheritance, and protocol compositions. The paradigm also changes how we view semantics. In Swift, value types are preferred over classes. However, object-oriented concepts don’t work well with structs and enums: a struct cannot inherit from another struct, neither can an enum inherit from another enum. So inheritancefa - one of the fundamental object-oriented concepts - cannot be applied to value types. On the other hand, value types can inherit from protocols, even multiple protocols. Thus, with POP, value types have become first class citizens in Swift.

__Protocol Extensions__

You can extend a protocol and provide default implementation for methods, computed properties, subscripts and convenience initializers.

__Protocol Inheritance__

A protocol can inherit from other protocols and then add further requirements on top of the requirements it inherits.

__Protocol Composition__

Swift types can adopt multiple protocols.

<a name="array-nsarray-anyobject"></a>
## Difference between Array VS NSArray VS [AnyObject]

`Array` is a struct, therefore it is a value type in Swift.

`NSArray` is an immutable Objective C class, therefore it is a reference type in Swift and it is bridged to `Array<AnyObject>`. `NSMutableArray` is the mutable subclass of `NSArray`.

`[AnyObject]` is the same as `Array<AnyObject>`

<a name="id-any"></a>
## Objective-C id is Swift Any or AnyObject

`Any` can represent an instance of any type at all, including function types and optional types.

`AnyObject` can represent an instance of any class type.

> As part of its interoperability with Objective-C, Swift offers convenient and efficient ways of working with Cocoa frameworks. Swift automatically converts some Objective-C types to Swift types, and some Swift types to Objective-C types. Types that can be converted between Objective-C and Swift are referred to as bridged types. Anywhere you can use a bridged Objective-C reference type, you can use the Swift value type instead. This lets you take advantage of the functionality available on the reference type’s implementation in a way that is natural in Swift code. For this reason, you should almost never need to use a bridged reference type directly in your own code. In fact, when Swift code imports Objective-C APIs, the importer replaces Objective-C reference types with their corresponding value types. Likewise, when Objective-C code imports Swift APIs, the importer also replaces Swift value types with their corresponding Objective-C reference types.

<a name="valuetype-vs-referencetype"></a>
## ValueType vs. ReferenceType

> __Reference type__: a type that once initialized, when assigned to a variable or constant, or when passed to a function, returns a reference to the same existing instance.

A typical example of a reference type is an object. Once instantiated, when we either assign it or pass it as a value, we are actually assigning or passing around the reference to the original instance (i.e. its location in memory). Reference types assignment is said to have shallow copy semantics.

When to use:

* Subclasses of NSObject must be class types
* Comparing instance identity with === makes sense
* You want to create shared, mutable state

> __Value type__: a type that creates a new instance (copy) when assigned to a variable or constant, or when passed to a function.

A typical example of a value type is a primitive type. Common primitive types, that are also values types, are: `Int`, `Double`, `String`, `Array`, `Dictionary`, `Set`. Once instantiated, when we either assign it or pass it as a value, we are actually getting a copy of the original instance. The most common value types in Swift are structs, enums and tuples can be value types. Value types assignment is said to have deep copy semantics.

When to use:

* Comparing instance data with == makes sense (Equatable protocol)
* You want copies to have independent state
* The data will be used in code across multiple threads (avoid explicit synchronization)

<a name="copy-on-write"></a>
## What is copy on write mechanism

A fully stack allocated value type will not need reference counting, but a value type with inner references will unfortunately inherit this ability.
```swift
final class ExampleClass {
  let exampleString = "Ex value"
}
struct ExampleStruct {
  let ref1 = ExampleClass()
  let ref2 = ExampleClass()
  let ref3 = ExampleClass()
  let ref4 = ExampleClass()
}
```
This way of having inner reference types is an expensive operation that requires many calls to `malloc/free` and a significant reference counting overhead each time you copy. `COW` enables value types to be referenced when they are copied just like reference types. The real copy only happens when you have an already existing strong reference and you are trying to modify that copy.
```swift
let array = [1,2,3] //address A
var notACopy = array //still address A
notACopy += [4,5,6] //now address B
```
__Manually implementing Copy On Write__

Swift standard library has already implemented `COW` for Dictionary, Array etc. The easiest way to implement copy-on-write is to compose existing copy-on-write data structures, such as Array. Swift arrays are values, but the content of the array is not copied around every time the array is passed as an argument because it features copy-on-write traits.There are two obvious disadvantages of using Array for `COW` semantics. The first problem is that Array exposes methods like `append` and `count` that don’t make any sense in the context of a value wrapper. These methods can make the use of the reference wrapper awkward. It is possible to work around this problem by creating a wrapper struct that will hide the unused APIs and the optimizer will remove this overhead, but this wrapper will not solve the second problem. The Second problem is that Array has code for ensuring program safety and interaction with Objective-C. Swift checks if indexed accesses fall within the array bounds and when storing a value if the array storage needs to be extended. These runtime checks can slow things down. An alternative to using Array is to implement a dedicated copy-on-write data structure to replace Array as the value wrapper.

Let’s consider a struct Car in which we want to use Copy on Write:
```swift
struct Car {
  let model = "M3"
}
```
We can create a `Ref` class which will wrap our `Car` value type.
```swift
final class Ref<T> {
  var val : T
  init(_ v : T) {val = v}
}
struct Box<T> {
    var ref : Ref<T>
    init(_ x : T) { ref = Ref(x) }
    var value: T {
        get { return ref.val }
        set {
          if (!isKnownUniquelyReferenced(&ref)) {
            ref = Ref(newValue)
            return
          }
          ref.val = newValue
        }
    }
}
```
The `Box` struct has a reference to `Ref` class and will return the value of our `Car` struct . When you try to set the value, it checks if there are any existing strong references and creates a new `Ref` if needed thereby limiting the copies to be made only while writing to it. `isKnownUniquelyReferenced` returns a boolean indicating whether the given object is known to have a single strong reference.

<a name="rxswift"></a>
## RxSwift

The first thing you need to understand is that everything in RxSwift is an observable sequence or something that operates on or subscribes to events emitted by an observable sequence. Arrays, Strings or Dictionaries will be converted to observable sequences in RxSwift. You can create an observable sequence of any Object that conforms to the `Sequence` Protocol from the Swift Standard Library. Observable sequences can emit zero or more events over their lifetimes.
In RxSwift an `Event` is just an Enumeration Type with 3 possible states:
1. `.next(value: T)` — When a value or collection of values is added to an observable sequence it will send the next event to its subscribers as seen above. The associated value will contain the actual value from the sequence.
2. `.error(error: Error)` — If an Error is encountered, a sequence will emit an error event. This will also terminate the sequence.
3. `.completed` — If a sequence ends normally it sends a completed event to its subscribers
If you want to cancel a subscription you can do that by calling dispose on it. You can also add the subscription to a `DisposeBag` which will cancel the subscription for you automatically on `deinit` of the `DisposeBag` Instance.

__Subjects__

A `Subject` is a special form of an Observable Sequence, you can subscribe and dynamically add elements to it. There are currently 4 different kinds of Subjects in RxSwift
1. `PublishSubject`: If you subscribe to it you will get all the events that will happen after you subscribed.
2. `BehaviourSubject`: A behavior subject will give any subscriber the most recent element and everything that is emitted by that sequence after the subscription happened.
3. `ReplaySubject`: If you want to replay more than the most recent element to new subscribers on the initial subscription you need to use a `ReplaySubject`. With a `ReplaySubject`, you can define how many recent items you want to emit to new subscribers.
4. `Variable`: A `Variable` is just a `BehaviourSubject` wrapper that feels more natural to a none reactive programmers. It can be used like a normal `Variable`

__Schedulers__

Operators will work on the same thread as where the subscription is created. In RxSwift you use schedulers to force operators do their work on a specific queue. You can also force that the subscription should happen on a specifc Queue. You use `subscribeOn` and `observeOn` for those tasks. If you are familiar with the concept of operation-queues or dispatch-queues this should be nothing special for you. A scheduler can be serial or concurrent similar to `GCD` or `OperationQueue`. There are 5 Types of Schedulers in RxSwift:
1. `MainScheduler` — “Abstracts work that needs to be performed on MainThread. In case schedule methods are called from the main thread, it will perform the action immediately without scheduling.This scheduler is usually used to perform UI work.”
2. `CurrentThreadScheduler` — “Schedules units of work on the current thread. This is the default scheduler for operators that generate elements.”
3. `SerialDispatchQueueScheduler` — “Abstracts the work that needs to be performed on a specific dispatch_queue_t. It will make sure that even if a concurrent dispatch queue is passed, it's transformed into a serial one.Serial schedulers enable certain optimizations for observeOn.The main scheduler is an instance of SerialDispatchQueueScheduler"
4. `ConcurrentDispatchQueueScheduler` — “Abstracts the work that needs to be performed on a specific dispatch_queue_t. You can also pass a serial dispatch queue, it shouldn't cause any problems. This scheduler is suitable when some work needs to be performed in the background.”
5. `OperationQueueScheduler` — “Abstracts the work that needs to be performed on a specific NSOperationQueue. This scheduler is suitable for cases when there is some bigger chunk of work that needs to be performed in the background and you want to fine tune concurrent processing using maxConcurrentOperationCount.”

__Difference between `.drive()` and `.bind(to:)`__

Driver ensures that observe occurs only on main thread. Thumb rule is are you trying to drive a UI component use `drive` else use `bindTo`. Generally if you want your subscribe to occur only on main thread and don't want your subscription to error out (like driving UI components) use `driver` else stick with `bindTo` or `subscribe`

__What's the difference between `bind` and `subscribe`?__

By using `bind(onNext)` you can express that stream should never emit `error` and you interested only in item events. So you should use `subscribe(onNext:...)` when you interested in `error` / `complete` / `disposed` events and `bind(onNext...)` otherwise

<a name="swiftui"></a>
## SwiftUI

SwiftUI предполагает, что описание структуры вашего View целиком находится в коде. Причем, Apple предлагает нам декларативный стиль написания этого кода. То есть, примерно так:

`Это View. Она состоит из двух текстовых полей и одного рисунка. Текстовые поля расположены друг за другом горизонтально. Картинка находится под ними и ее края обрезаны в форме круга.`
```swift
struct ContentView: View {
    var text1 = "some text"
    var text2 = "some more text"
    var body: some View {
        VStack{
            Text(text1)
                .padding()
                .frame(width: 100, height: 50)
            Text(text2)
                .background(Color.gray)
                .border(Color.green)
        }
    }
}
```
Обратите внимание, `View` — это структура с некоторыми параметрами. Что бы структура стала `View` — нам нужно задать вычисляемый параметр `body`, который возвращает `some View`. Содержание замыкания` body: some View { … }` — это и есть описание того, что будет отражено на экране.
Три типа элементов, из которых строится тело View:
- Другие View, т.е. Каждая `View` содержит в себе одну или несколько других `View`. Те, в свою очередь могут так же содержать как предопределенные системные `View` вроде `Text()`, так и кастомные, сложные, написанные самим разработчиком.
- Модификаторы - благодаря им, мы коротко и внятно сообщаем SwiftUI, какой мы хотим видеть данную `View`
- Контейнеры - `HStack`, `VStack`, `ZStack`, `Group`, `Section` и прочие. Фактически, контейнеры — это те же `View`, но у них есть особенность. Вы передаете в них некий контент, который нужно отобразить. Вся фишка контейнера в том, что он должны как-то сгруппировать и отобразить элементы этого контента. В этом смысле, контейнеры похожи на модификаторы, с той лишь разницей, что модификаторы предназначены изменять одну уже готовую `View`, а контейнеры выстраивают эти View (элементы контента, или блоки декларативного синтаксиса) в определенном порядке, например вертикально или горизонтально.

__state параметры__

Прежде всего, это переменные состояния — хранимые параметры нашей структуры, изменение которых должно быть отражено на экране. Их оборачивают в специальные обертки `@State` — для примитивных типов, и `@ObservedObject` — для классов. Класс должен удовлетворять протоколу `ObservableObject` — это значит, что данный класс должен уметь оповещать подписчиков (`View`, которые используют данное значение с оберткой `@ObservedObject`) об изменении своих свойств. Для этого достаточно обернуть требуемые свойства в `@Published`.
```swift
struct ContentView: View {
    @State var tapCount = 0
    var body: some View {
        VStack {
            Button(action: {self.tapCount += 1},
                   label: {Text("Tap count \(tapCount)")})
        }
    }
}
```
`@Binding` - это еще один Property Wrapper, с помощью которой мы объявляем параметры структуры, которые будут не просто меняться, а и возвращаться в родительскую `View`.

`@EnvironmentObject` — это как `Binding`, только сразу для всех `View` в иерархии, без необходимости их передавать в явном виде.

`ForEach` — это набор subView, сгенерированных для каждого элемента коллекции исходя из переданного контента.

Роль navigation controller берет на себя специальный `NavigationView`. Достаточно обернуть ваш код в `NavigationView{...}`. А само действие перехода можно добавить в специальную кнопку `NavigationLink`, которая пушит условный экран `DetailView`.
```swift
var body: some View {
     NavigationView {
     Text("World Time").font(.system(size: 30))
          NavigationLink(destination: DetailView() {
               Text("Go Detail")
          }
     }
}
```

<a name="combine"></a>
## Combine

На WWDC 2019 был представлен фреймворк Combine от Apple. Он позволяет моделировать все виды асинхронных событий и операций типа “значения, изменяющиеся во времени”. Не смотря на то, что данное понятие, часто используется в мире реактивного программирования как концепция и способ организации логики, поначалу бывает сложно сразу во всем разобраться.

__Publisher__

Declares that a type can transmit a sequence of values over time.

__Subject__

A publisher that exposes a method for outside callers to publish elements.

__Subscriber__

A Subscriber instance receives a stream of elements from a Publisher, along with life cycle events describing changes to their relationship. A given subscriber’s Input and Failure associated types must match the Output and Failure of its corresponding publisher.

__Scheduler__
Defines when and how to execute a closure.

`Publishers` могут быть бесконечно активны либо завершены по итогу какого-либо события, а также `Publishers` могут быть опционально не выполнены, если произошла какая-то ошибка. Чтобы внедрить `Combine`, Apple доработали некоторые из своих основных библиотек, чтобы они так же могли поддерживаться `Combine`. Например, вот так может использоваться тип `URLSession` для создания `Publisher`, выполняющего сетевой запрос по заданному URL-адресу:
```swift
let url = URL(string: "https://api.github.com/repos/johnsundell/publish")!
let publisher = URLSession.shared.dataTaskPublisher(for: url)
```
После того, как мы создали `Publisher`, мы можем привязать к нему подписки, например, с помощью `sink` API, который позволяет передавать замыкание, срабатывающее каждый раз, когда было получено новое значение, а так же другое замыкание, которое отрабатывает когда `publisher` завершает свою работу:
```swift
let cancellable = publisher.sink(
    receiveCompletion: { completion in
        switch completion {
        case .failure(let error):
            print(error)
        case .finished:
            print("Success")
        }
    },
    receiveValue: { value in
        print(value)
    }
)
```
Обратите внимание на то, как описанный выше метод `sink` возвращает значение, которое мы храним как `cancellable`. При присоединении нового подписчика, `Publisher` всегда возвращает объект, соответствующий протоколу `Cancellable`, действующему как токен для новой подписки. Затем, нам нужно сохранить этот токен до тех пор, пока мы хотим оставить нашу подписку активной, иначе как только она будет освобождена (deallocated), наша подписка будет автоматически отменена (кстати, подписку можно отменить вручную при помощи вызова метода `cancel()` у токена). Операторы используются для построения реактивных цепочек или пайплайнов (конвейеров), по которым могут проходить наши данные, где каждый оператор применяет некоторую форму преобразования к данным, которые были ему отправлены.
`cancellable` используется для отслеживания подписки (subscription) на `Publisher` и должен сохраняться до тех пор, пока мы хотим, чтобы эта подписка (`subscription`) оставалась активной.

Пример с операторами:
```swift
let sub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
    .map( { ($0.object as! NSTextField).stringValue } )
    .filter( { $0.unicodeScalars.allSatisfy({CharacterSet.alphanumerics.contains($0)}) } )
    .debounce(for: .milliseconds(500), scheduler: RunLoop.main)
    .receive(on: RunLoop.main)
    .assign(to:\MyViewModel.filterString, on: myViewModel)
```

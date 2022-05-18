# Go for C# developers

Learning GO is not only about a new language, but also about a different philosophy from what we are used to.

This page contains some useful links and answers to questions I had a long the way. Hopefully, it can help you too.

> NB! This is and always will be a work in progress!

> I'll keep adding more materials, details, and answers a long the way, but as anything in life, the learning process is endless, therefore, the material in here can become outdated.

## Materials

### Official docs

- Effective Go - [https://go.dev/doc/effective_go](https://go.dev/doc/effective_go)
  - A document that gives tips for writing clear, idiomatic Go code. A must-read for any new Go programmer. It augments the tour and the language specification, both of which should be read first.
- Go Code Review Comments [https://github.com/golang/go/wiki/CodeReviewComment](https://github.com/golang/go/wiki/CodeReviewComment)
  - This page collects common comments made during reviews of Go code, so that a single detailed explanation can be referred to by shorthands. This is a laundry list of common mistakes, not a comprehensive style guide. You can view this as a supplement to Effective Go.
- Go Docs - [https://go.dev/doc/](https://go.dev/doc/)
  - Root for many useful documentations
- FAQ - [https://go.dev/doc/faq](https://go.dev/doc/faq)
  - Answers to common questions about Go.


### Courses

- The way to go - [https://www.educative.io/courses/the-way-to-go](https://www.educative.io/courses/the-way-to-go)
  - It's a course from educative.io, it goes from the basics concepts of the language to more advanced ones. It also brings very interesting insights into the differences between the approaches of Java/C# to Go.
  - It has many challenges in exercises you can do directly in the browser. I found it excellent because I didn't need to have any thing installed and I managed to do it in anything that has a browser.

### Videos

- Go in 100 seconds - [https://www.youtube.com/watch?v=446E-r0rXHI&t=38s](https://www.youtube.com/watch?v=446E-r0rXHI&t=38s)
  - Short introduction about Go
- Simplicity is complicated - [https://www.youtube.com/watch?v=rFejpH_tAHM](https://www.youtube.com/watch?v=rFejpH_tAHM)
  - Rob Pike talks about how Go is often described as a simple language. It is not, it just seems that way. Rob explains how Go's simplicity hides a great deal of complexity, and that both the simplicity and complexity are part of the design.
- Concurrency is not parallelism - [https://www.youtube.com/watch?v=qmg1CF3gZQ0&t=1582s](https://www.youtube.com/watch?v=qmg1CF3gZQ0&t=1582s)
  - Rob Pike talks about concurrency and how Go implements it
- Understanding channels - [https://www.youtube.com/watch?v=KBZlN0izeiY&t=1011s](https://www.youtube.com/watch?v=KBZlN0izeiY&t=1011s)
  - Channels provide a simple mechanism for goroutines to communicate, and a powerful construct to build sophisticated concurrency patterns. We will delve into the inner workings of channels and channel operations, including how they're supported by the runtime scheduler and memory
- Concurrency in Go - [https://www.youtube.com/watch?v=\_uQgGS_VIXM&list=PLsc-VaxfZl4do3Etp_xQ0aQBoC-x5BIgJ](https://www.youtube.com/watch?v=_uQgGS_VIXM&list=PLsc-VaxfZl4do3Etp_xQ0aQBoC-x5BIgJ)
  - Playlist with few short videos about different components of concurrency in Go

## Code

### Playground
If you want to try out something directly in your browser you can use [The Go Playground](https://go.dev/play/).

### Experiments

The folder experiments will contain pieces of code I'm playing around, and also examples related to the questions below.

## FAQ for C# developers

### 1 - For what should I use Go?

Go has several uses, since command line applications, web services (yes web apis too :) ), and container applications like Docker and Kubernetes.

### 2 - Which text-editor/IDE should I use?

If you are coming from Visual Studio/Rider background, you will feel more familiar with [Visual Studio Code](https://code.visualstudio.com/) and [GoLand](https://www.jetbrains.com/go/), however there are others IDEs/text editors out there with nice tooling, like [Emacs](https://www.gnu.org/software/emacs/).

- [Visual Studio Code](https://code.visualstudio.com/) has everything you need to develop your application, and it's free.
  It's one of the best text editors, the debugger experience is decent, and there are really good plugins to help out with producitivity. [The extension](https://code.visualstudio.com/docs/languages/go) developed by Google itself is a must have.

- [GoLand](https://www.jetbrains.com/go/) it's a more robust IDE built by JetBrains. If you familiar with Rider or IntelliJ IDEA you will fell at home.
  It's not free, but its price is affordable.

### 3 - Does Go compile to some sort of Intermediate Language?

Nope. There is no IL, the compiler generates an executable that can be executed [directly by the computer](https://getstream.io/blog/how-a-go-program-compiles-down-to-machine-code/).

### 4 - Is it possible to write `async` code which won't block the main thread while it's `await`ing for something?

Huge yes! In Go we don't deal with threads directly. Instead, we use something called goroutines. In some sense they are similar to a `Task` which is not a necessary a thread but represents some work to be done, which will be eventually scheduled, executed in a thread, and resumed.

I'm not naive to try to explain it in a short answer, so check [this video](https://www.youtube.com/watch?v=qmg1CF3gZQ0&t=1582s) from Rob Pike about it.
[This video about concurrency patterns](https://www.youtube.com/watch?v=f6kdp27TYZs) explains how concurrency works and some "patterns" like multiplexing, generator, fan-in, etc.

### 5 - Do I need to use goroutines and channels to make non blocking IO calls?

No, it's not needed. Go offers non-blocking io calls via an blocking interface. It means that, despite you are calling a "sync" method, under the hood it will use goroutines to interact with async APIs from the OS. [The answers for this question on Stackoverflow](https://stackoverflow.com/questions/36112445/golang-blocking-and-non-blocking) give more details about how it's accomplished.

### 6 - What about interfaces?

An interface type in Go are similar to an interface in C#, with the following differences: 

- an interface type can only have method signatures, not properties 
- it doesn't(\o/) support [default implementations](https://devblogs.microsoft.com/dotnet/default-implementations-in-interfaces/) 
- the [support to generics in Go](https://go.dev/blog/intro-generics) is very recent, and it doesn't have the flexibility that C# has.

Another difference is how to implement an interface. A type `T` that implements a interface `I` doesn't need to know it. As long as it has all the methods defined in the interface, you will be able to pass it. e.g.
````go
package main

import "fmt"

type multiplier interface {
	multiply(val int) int
}

type multiplyByFour struct {
}

func (m *multiplyByFour) multiply(val int) int {
	return val * 4
}

func main() {
	var m multiplier
	m = &multiplyByFour{}
	fmt.Println(m.multiply(2))
}
````

Note the type `multiplyByFour` doesnt have a ` : multiplier` or `implements multiplier`. It just have all methods defined in the interface

### Next Questions:

- What about Nullable objects?
- Is there anything similar to LINQ?
- What about dependency injection containers?
- Which framework should I use for writing web APIs?
- What is the preferred t way to write logs?
- How should I organize my project?
- What IDE should I use

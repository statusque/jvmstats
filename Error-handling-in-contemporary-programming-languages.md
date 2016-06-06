We consider error handling in programming languages an unsolved problem. While some of the languages below have better mechanisms than others, no language provides the final answer.

**Please add an issue if you find mistakes, have language suggestions or, even better, a good idea for better ways to do error handling.**

## How do errors get communicated?

Signals, exceptions, Windows structured exceptions, return codes, multiple return values and tuples, output parameters, and globals/singletons (!) are some examples in actual use. Most languages allow at least three. The more mechanisms, the more inconsistent the APIs appear to be.

Some languages support asynchronous delivery of errors.

## Where do errors come from?

Your own code, libraries and frameworks and the OS or virtual machine. A language that strongly encourages, or even enforces, idiomatic error handling is obviously more likely to result in consistent error handling across libraries, making your application's error handling code consistent as well.

## Are there multiple kinds of errors? Does it matter?

Many language designers makes the distinction between recoverable and unrecoverable errors, but not all. Some languages even provide different idioms for these categories of errors. Whether or not the distinction is important is up for debate. In many cases, the system context and architecture decides whether or not a certain error is deemed recoverable. The fact is, most programmers do not spend a lot of time differentiating the two.

## Why do errors propagate?

Error propagation through exceptions is a non-local goto with an unknown destination. This means that reasoning about system behavior gets increasingly, well, impossible. Why do we allow this mess?

Some programmers feel errors should be dealt with immediately, while others feel errors should be dealt with as late as possible, where most of the context exist. For instance, the lowest layer of a three-tier application may experience network outage while trying to communicate with a database - what do you do? In some cases you are obliged to retry and maybe even try alternative routes. In other cases, you consider the error fatal, so you log the error, report the error to the devops system and then terminate. In many cases, you rethrow (maybe wrapping it first) to a higher layer where it is either logged, or ends up in a dialog box or web page. Letting the user deal with it is an unfortunate, yet popular choice.

## What can you do instead of logging and rethrowing to the UI?

Even the best programmers are bad at both anticipating error situations, and finding good ways to handle the errors they *do* anticipate. That's because this stuff is really really hard. Here are some guidelines we think are worthwhile:

### Prevent errors

* Do everything you can to have the compiler and build tools eliminate errors in the first place. If you care about your users, this includes using safe languages (Kotlin prevents NPE's, for instance), design-by-contract api's, employing every static analysis tool available, implement high coverage test suits and do code reviews.

* Use queues whenever possible, not RPC. Preferably persistent queues with fail-over protocols. A huge class of errors just disappeared. Microservices may be the marketing term of this decade, but small independent processes with queues between them is the *only* known way to write robust distributed systems.

### Handle errors like a pro

* Establish idioms and patterns for specific error situations and reuse them. For instance, do you really have to give up on network outages and database connection errors so easily? Use libraries and drivers that attempt auto-reconnects and fails gracefully when not succeeding. Build your own logic if need be and put it in a library.

* Get into the habit of eliminating classes of errors, not specific bug reports. If you get a bug report saying "number format exception", you're likely to do that a few places in your code base. Find a general way to handle them, and fix all the use sites. Make sure you use the improved idea in the future. This is why you need to write (and document) utility libraries.

* Resume. That is, continue on the statement following the throwing statement. It's sad that so few languages support this powerful idiom. I encourage you to try this idea in Perl 6. For other languages, you'll have to resort to tricks.

## Java

Java is infamous for offering both checked and unchecked exceptions. The original idea, as explained by Gosling et.al: unchecked exceptions was meant to express "mostly unrecoverable" errors (such as an NPEs), while checked exceptions was meant to express recoverable errors, such as bad user input and nonexisting files. In practice, the difference is too blury to be useful. If your application depends on a system configuration file and cannot access it at the expected location, is it really recoverable? Probably a permission setting, and you have to give up. 

In reality, people end up making more or less arbitrary choices between checked and unchecked exceptions.

There's a certain class of interfaces where checked exceptions are very good, namely when you *really* need to ensure the caller handles the errors (at least make them put some effort into thinking about it!) Many people have noted that moving from Java to C# makes them feel uneasy, because they lack this tool.

Lambdas doesn't play nicely with checked exceptions, and various workarounds are routinely implemented (such as *sneaky exceptions*)

## Kotlin

Kotlin eliminates many error situations through the type system. For instance, NPE's are (mostly) eliminated.

The language supports unchecked exceptions only and provides a rationale here: <https://kotlinlang.org/docs/reference/exceptions.html>

## Swift

When Swift was first released, it was famously silent on the issue of error handling. Because Swift needs to play well with the Objective-C runtime, this was a hard problem. The solution they came up with was ingenious, given the constraints.

Swift has this idea called protocols. It is similar to a Java interface, but also allows the declaration of properties. Protocols are also intersection-able types, so you can require a parameter to adhere to the intersection of two protocols. Very useful.

ErrorType is such a protocol, and instances thereof can be thrown. You can propagate the errors, handle them in do-catch blocks, convert the error to optional values or force-convert the error to a runtime error using the try! expression.

Related to error handling you'll also find the defer statement, allowing cleanup code to run when a scope is left (for instance, due to error propagation.)

While not perfect, we consider Swift the current gold standard for error handling.

## CSharp

C# has unchecked exceptions, and that's mostly what people use. However, it's not entirely uncommon to see errors reported as output parameters (something which Java doesn't support, unless you use wrapper types.)

It's also common to see return value checking in interop code (which is a fairly common need on Windows.)

## C++

C++ is long in the tooth, and provide many ways to do error handling. Libraries are famously inconsistent with respect to others, making application code "interesting". Checking return codes is still common, if not for other reasons than to interface with C libraries. 

Exception safety is a hard but important problem (that is, avoid leaking resources in the face of an exception.)

In C++, you use the RAII pattern instead of a finally block (see <http://www.stroustrup.com/bs_faq2.html#finally>)

## Haskell

While OOP coders tend to use exceptions to deal with errors, Haskell coders tend to prefer monads. Examples of error handling monads are Maybe and Either. The point is that monads preserve the most important characteristic of a purely functional language: composability.

That said, Haskell support exceptions and it even works with lazy evaluation, and is intelligently tied to the pattern matching system. It's an impressive system that many OOP languages doing async idioms can learn from.

## Python, Ruby, Javascript, etc

Python supports a try/except/else/finally construct. That said, it is not uncommon to see Python scripts not dealing much with exceptions at all. The "log exceptions we can't deal with" approach of Java is certainly frowned upon. We're not sure if "exit with a runtime error if we can't deal" is much better.

Other dynamic languages have different details, but the philosophy is more or less the same.

## Go

Panic/recover somewhat resembles exceptions, but some people consider this an anti-pattern. The idiomatic way to do error handling, as stated by the language authors, is really multiple return values. Some find it elegant, others consider it a sightly improved C-era abomination.

We'll let you decide for your own after reading <https://blog.golang.org/errors-are-values>

## Perl 6

Perl 6 introduces typed exceptions, along with a resume mechanism. Check it out.

## PHP

It's best if we don't go there.


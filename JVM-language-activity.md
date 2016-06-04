A monthly search on Github gives a good indication of which JVM languages are alive and kicking. The search methodology isn't perfect, but it's a decent indication.

# JVM language activity, May

Search: *language:xyz pushed:>2016-05-01*, result >= 10

    Language      Released  Sponsor      Active repos
    =================================================
    Java          1995      Oracle            185,034
    Scala         2004      EPFL                6,508
    Clojure       2007      Clojure team        2,696
    Groovy        2003      Groovy team         1,865
    Kotlin        2016      Jetbrains            1075
    Xtend         2011      Eclipse Foundation     73
    Frege         2011      Frege team             26
    Ceylon        2011      Red Hat                13

# Update frequency of this list

A script automatically checks Github every month. The list is updated if

* a language changes position, or
* a language moves above or below the >= 10 active-repos-per-month limit, or
* a language increases or decreases active-repos-per-month by at least 25%.

# Java

Java is incredibly popular and I expect no change here. Java 9 brings several future-proofing features.

# Scala

By far the second most popular language on the JVM. However, Kotlin is growing so fast that I think this _may_ change in a few years. That said, Odersky and team appears to have a plan for modernizing the language (or provide an alternative), which may help boost popularity even further.

# Kotlin

Jetbrains secured a solid reputation with their IntelliJ platform, and Kotlin benefits greatly from this in various ways. The solid growth comes partly from their Android support (Android Studio supports Kotlin out of the box), partly from the fact that Kotlin is very easy to learn.

# Clojure

Rick Hickey struck a chord with the marriage of Lisp and the huge JDK library. Clojure has seen steady growth since the inception and the current functional programming interest isn't hurting.

# Xtend

Xtend is steadily growing after establishing itself as _the_ language for creating DSL's on the JVM platform (using Xtext usually)

# Groovy

The popular build system *Gradle* is making sure Groovy isn't going away any time soon. A lot of people use Groovy to make tiny DSL's as well.

# Ceylon

Ceylon is actively developed, but has so far not seen widespread adoption. I expect Android support to increase popularity if they succeed with an IDE integration on par with Kotlin.

# Frege

A language I expect will enjoy continued growth, given that it's based on ideas from Haskell (which is growing like crazy these days!)

# Other languages

Add an issue if you want to see your favorite language on the list.
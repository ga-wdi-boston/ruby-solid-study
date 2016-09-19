# SOLID Principles of Object-Oriented Design

Read this study and answer the questions at the end. The "Additional Resources"
are optional, but highly recommended.

## Design Principles

> Controlling complexity is the essence of computer programming. — [Brian
> Kernighan](http://genius.cat-v.org/brian-kernighan/)

A developer's job is managing complexity. More complex systems fail more often,
are more expensive to maintain, and are difficult to understand. Less complex
systems are systems well-suited to change.

Complexity is a function of dependencies. A complex system is tightly coupled,
with changes rippling throughout the entire system. A simple system has
clearly-identifiable dependencies that are not affected by change.

Have you ever dreaded changing some code for fear it might break something you
won't know how to fix? Part of the blame might lie in you, in your
understanding, but the antidote for that is more practice and exposure to design
principles. The majority of the blame probably lies in a complex system that you
can't fully understand. The antidote is the same.

Less complex systems are well-designed, and well-designed systems are easier to
learn. This makes these systems resilient to change, since it is easier to trace
the effects of a change and potentially stop the propagation of change with a
well-placed abstraction. Well-designed systems help us manage our expectations,
and managing our expectations is a crucial part of approaching code with
confidence.

Some characteristics of poorly defined systems:

> -   Rigid (difficult to change)
> -   Fragile (tendency to break when changed)
> -   Immobile (limited reuse of components)
> -   Viscous (easier to do it "wrong" than "right")
>
> – Adapted from ["Design Principles and Design Patterns" by Robert C.
> Martin](http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf)

Bob Martin is much smarter than me: he has identified patterns in poorly-defined
software **and** recommended some principles that can guide us on our way to
better design. These principles are abbreviated SOLID.

Design principles are goals. The strategies we employ in writing code that can
be described by these principles are called "refactorings". Often, we can
recognize bad patterns in the code we write by "code smells". So, design
principles provide us a framework for structuring our work. The cycle for
improving our code will look like this:

1.  Identify an issue with your code, usually by the presence of a code smell.
1.  Extract methods and classes to better define responsibilities.
1.  Inject our new code into our old code so behavior doesn't change.
1.  Repeat as necessary.

This is inherently an iterative process. This process is made easier by the
presence of automated tests, but at the very least we should manually exercise
the part of our codebase that is changing to ensure our system still works.

## SOLID

[SOLID](https://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29) is an
acronym that stands for:

-   [Single Responsibility Principle (SRP)](http://en.wikipedia.org/wiki/Single_responsibility_principle)
-   [Open/Closed Principle (OCP)](http://en.wikipedia.org/wiki/Open/closed_principle)
-   [Liskov Substitution Principle (LSP)](http://en.wikipedia.org/wiki/Liskov_substitution_principle)
-   [Interface Segregation Principle (ISP)](http://en.wikipedia.org/wiki/Interface_segregation_principle)
-   [Dependency Inversion Principle (DIP)](http://en.wikipedia.org/wiki/Dependency_inversion_principle)

These principles are presented and defined separately, but in practice they work
together. For example, we might initially extract a class to improve adherence
to SRP, and in the process we practice DIP so our code doesn't break.

ISP states that "many client-specific interfaces are better than one
general-purpose interface". That means it is better to have many small, focused
objects in our system than to have one general purpose object.

Fortunately, since we use dynamically-typed languages like Ruby and JavaScript,
we get ISP for free through duck-typing. We don't have explicit object
interfaces in Ruby, for example. Instead, we have objects that do or do not
respond to certain messages. Instead of an explicit interface, we have an
implicit protocol. In statically-typed languages, ISP allows us to more easily
compose objects together to get the behavior we want. All we have to do to
achieve ISP is focus on SRP (and DIP).

LSP is another principle we don't have to think much about. LSP says that
"subclasses should be substitutable for their base classes". For example, things
we call "Vehicles" should behave like "Sedans", though the latter may be more
specific. This is similar to the mantra we've repeated in the past about
relationships between different concepts: "all squares are rectangles but not
all rectangles are squares".

```ruby
class Rectangle
  attr_reader :number_of_sides, :length, :width

  def initialize(length, width)
    @number_of_sides = 4
    @length, @width = length, width
  end
end

class Square < Rectangle
  def initialize(side)
    super
    @length = side
    @width = side
  end
end

# Suppose we just want a four-sided object. Any will do.
def is_four_sided?(obj)
  obj.number_of_sides == 4
end

is_four_sided?(Rectangle.new(2,3))
is_four_sided?(Square.new(5))

# Does this code adhere to LSP when the client wants a four-sided object?
# What if the client wants a four-sided object with equal sides?
```

OCP can be thought of as the primary goal of good design. OCP says that "objects
should be open to extension, but closed to modification". If our objects conform
to OCP, they're more easily composable. What this means is that we should be
able to take an object and extend it with new behavior. If we can't create new
object to obtain the behavior we want **without** opening the extended
class/module, we're not adhering to OCP. Ruby makes it really easy for us to
extend objects through inheritance and mixins. Again, we can achieve OCP by
focusing on SRP and DIP.

SRP says that "objects should have one and only one reason to change". If an
object has more than one job, it increases the likelihood that any particular
feature request will cause it change. Objects with the most responsibilities
change the most often in our system. Since our goal is managing change and
reducing complexity, this might be the most important tool we have when
refactoring. If you describe an object's responsibilities in English and you
have to use "and" or "or", it does not have a single responsibility. We may even
notice "and" or "or" in our specs or method names. This is a huge code smell!
Focus on grouping related behavior together and extracting it into another
object.

Finally, DIP states to "depend upon abstractions, not concretions". For example,
a general purpose configuration object in our system should not care about how
specific business models are implemented. Instead, those specific business
models are allowed to depend on the general configuration object to configure
their behavior. In practice, achieving DIP is as simple as injecting an object
with a general purpose back into a specific object after extracting a class.

## Heuristics

Heuristics is ten-dollar word for "rules of thumb". Use these rules as guiding
principles as you code. Most initial code you write will not meet these
criteria, but any refactorings you do should tend toward compliance with these
rules.

### [Sandi Metz' Rules](https://robots.thoughtbot.com/sandi-metz-rules-for-developers)

> 1.  Your class can be no longer than 100 lines of code.
> 1.  Your methods can be no longer than five lines of code.
> 1.  You can pass no more than four parameters and you can’t just make it one
>     big hash.
> 1.  When a call comes into your Rails controller, you can only instantiate one
>     object to do whatever it is that needs to be done. And your view can only
>     know about one instance variable.
>
> You can break these rules if you can talk your pair into agreeing with you.

The Bike Shed podcast has a great [episode](http://bikeshed.fm/1) detailing the
experience of applying these rules on a real project.

### Questions

Ask yourself the following questions when you get ready to refactor your code.
If the answer is "no" to any of these questions, start by addressing that issue
and see where your refactorings lead you.

-   Is it DRY?
-   Does it have one responsibility?
-   Does everything change at the same rate?
-   Does it depend on things that change less often than it does?

## Going Slow to Go Fast

You're probably thinking, "Why would I do this? It's too slow and it will take
forever." Yes it is slow, especially at first. Design is difficult. Testing is
hard. Both good design and testing feed into each other, and result in apps that
are easier to maintain over time. Initially, not thinking about design or
testing will let you move faster, but as time goes on they become crucial to
being able to maintain development speed and user happiness. You shouldn't worry
about design if you expect your project to fail, but:

> If you're an apparent success, if you've gotten enough features out the door
> that people want and you don't design, you can guarantee you will fail later
> when people ask you change it. – Sandi Metz

## Additional Resources

-   [Practical Object-Oriented Design in Ruby](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley-ebook/dp/B0096BYG7C/)
-   [SOLID Design Principles - Practicing Ruby, Issue 1.23](https://practicingruby.com/articles/solid-design-principles)
-   [Confreaks TV | SOLID Object-Oriented Design - GORUCO 2009](http://confreaks.tv/videos/goruco2009-solid-object-oriented-design)
-   [Confreaks TV | SOLID Ruby - Ruby Conference 2009](http://confreaks.tv/videos/rubyconf2009-solid-ruby)
-   [The SOLID Principles - Tuts+ Code Tutorials](http://code.tutsplus.com/series/the-solid-principles--cms-634)

## Response

Use your favorite search engine and the provided readings to research and answer
the following questions.

In your answers, be sure to cite any relevant sources you consulted in your
search. We ask you to write answers in your own words in order to see how you
process what you've read. Please do not answer with direct quotes from source
material. Instead, digest what you've read and repeat it in your own voice.

## Explain Why Good Software Design is Important

In your own words, explain why good design is important.

```md
NOTE: For all answers, I used the provided readings, wikipedia entries for SOLID
object-oriented programming, Duck-typing, and related links within those
articles .

Good design is important because it allows us to write code that doesn't just
meet the current specs of our project, but also anticipates future needs. If we
adhere to basic design principles like SOLID and DRY, we can avoid creating
problems that would cost lots of time and money to fix. After experiencing
certain issues over and over again, computer science theorists have condensed
their solutions for avoiding these problems--which they learned would be an
inevitability for most programmers, regardless of their level of experience--
into a few concise rules. Also, the collaborative nature of programming makes
adherance to these rules even more important, as we are not just writing code
for our current and future selves, but for the other programmers we are working
with.
```

## Identify Good Design

List some criteria for well-designed code. Contrast this list with indicators
of poor design.

```md
Good design is meets most or all of the following criteria:
  -All objects have a single responsiblity that can be described in an
  English sentence without using "and" or "or"
  -When a class is defined, one should have the ability to elaborate on the
  behavior of that class, but the original class should remain unchanged in
  doing so
  -One should be able to substitute an instance of a class for that same class
  and expect the instance to inherit the behavior of its class without breaking
  code; in other words, we should be able to substitute the specific for the
  general (such as "sedan" for "car")
  -If it is necessary to make a change in one place, we should not have to
  refactor a lot of additional code to reflect that one change
  -One should depend on abstraction, rather than concretion, as a guiding
  principle when we write our code; for example, if we are writing code for an
  e-commerce site that sells bicyles, we should try to write code that we could
  apply more generally to any similarly structured e-commerce sites
  -We should define things in such a way that each class or method has only
  a single responsibility

Poor design shows one or more of the following criteria; it is:
  -Rigid (difficult to change)
  -Fragile (tendency to break when changed)
  -Immobile (limited reuse of components)
  -Viscous (easier to do it "wrong" than "right")

  (Sorry for not rephrasing the second part of this answer; I could not think
  of an alternative way to phrase the very concise ideas of rigid, fragile,
  immobile, and viscous.)

```

## Design Heuristics

Are heuristics the same thing as rules? What are some design heuristics you can
use to improve the design of your code?

```md
While similar, heuristics and rules are not identical. Heuristics can best be
described as guiding principles one should generally follow since they apply
in most cases, but that can be broken with good reason (kind of like 'best
practices'). Rules can be thought of as more rigid; they are more than
guidelines, and we should strictly adhere to them when programming, because they
define things like syntax within a particular language. Just like in English,
if we don't follow the rules of Ruby syntax, we will have trouble understanding
one another.

A good place to start when refactoring is by applying Sandi Metz's design
heuristics, defined as folllows:
  1. Your class can be no longer than 100 lines of code.
  2. Your methods can be no longer than five lines of code.
  3. You can pass no more than four parameters and you can’t just make
  it one big hash.
  4. When a call comes into your Rails controller, you can only instantiate
  one object to do whatever it is that needs to be done. And your view can only
  know about one instance variable. (I don't know exactly what this one means
  yet, but I am hoping to find out once we dive into Rails!)

```

## Refactorings

Describe some common, named refactorings you can use to approach a good design.

```md
You can start refactoring by asking yourself, is my code...?
  -SOLID, adhering to the following principles:
    -SRP (Single Responsibility Principle)
    - ICP (Open/Closed Principle)
    - LSP (Liskov Substitution Principle)
    - ISP (Interface Segregation Principle)
    - DIP (Dependency Inversion Principle)
  -DRY (Don't Repeat Yourself)
```

# Cucumber In Practice

[Behavior-Driven Development]: https://en.wikipedia.org/wiki/Behavior-driven_development
[Cucumber]: https://en.wikipedia.org/wiki/Cucumber_(software)
[Cucumber Documentation]: https://cucumber.io/docs/cucumber
[Gherkin]: https://cucumber.io/docs/gherkin/reference
[loom-lab]: https://github.com/kolotyluk/loom-lab
[Road Rash]: https://www.merriam-webster.com/dictionary/road%20rash
[ScalaTest Given/When/Then]: https://alvinalexander.com/scala/adding-given-when-then-behavior-scalatest-bdd-tests
[Two Hard Things]: https://martinfowler.com/bliki/TwoHardThings.html

<div style="padding: 16pt;font-size: 16pt;font-family: 'Times New Roman', serif;">
While, in theory, I find <em>Behaviour-Driven Development</em> very compelling,
in practice,<br />I find that <em>Cucumber</em> is a <em>high-<strong>friction</strong></em> solution,
leading to <a href="https://www.merriam-webster.com/dictionary/road%20rash">Road Rash</a>.
</div>

- [Cucumber In Practice](#cucumber-in-practice)
  * [ScalaTest](#scalatest)
  * [Friction](#friction)
- [Practices](#practices)
  * [Feature Files](#feature-files)
    + [Two Hard Things](#two-hard-things)
    + [Acceptance Testing](#acceptance-testing)
    + [Definition of Ready & Done](#definition-of-ready---done)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

From [Behavior-Driven Development]
> In software engineering, behavior-driven development (BDD) is an agile software development process
> that encourages collaboration among developers, quality assurance testers, and customer representatives
> in a software project. It encourages teams to use conversation and concrete examples to formalize a
> shared understanding of how the application should behave. It emerged from test-driven development (TDD).
> Behavior-driven development combines the general techniques and principles of TDD with ideas from
> domain-driven design and object-oriented analysis and design to provide software development and
> management teams with shared tools and a shared process to collaborate on software development.

a key benefit of this methodology is that it "encourages collaboration among developers, quality assurance
testers, and customer representatives in a software project." 

[Cucumber] is a software tool that implements this methodology, where I first used it in [loom-lab].

See also my thoughts on [DDD Philosophy](DDD%20Philosophy.md)

## ScalaTest

In the past I have used [ScalaTest Given/When/Then] to define BDD Tests, with very little *friction*.

    package com.acme.pizza

    import org.scalatest.FunSpec
    import org.scalatest.BeforeAndAfter
    import org.scalatest.GivenWhenThen

    class PizzaSpec extends FunSpec with GivenWhenThen {

      var pizza: Pizza = _

      describe("A Pizza") {

        it ("should allow the addition of toppings") {
          Given("a new pizza")
          pizza = new Pizza

          When("a topping is added")
          pizza.addTopping(Topping("green olives"))

          Then("the topping count should be incremented")
          expectResult(1) {
            pizza.getToppings.size
          }

          And("the topping should be what was added")
          val t = pizza.getToppings(0)
          assert(t === new Topping("green olives"))
        }
      }
    }

While this only took me a few minutes to set up my first time, by contrast using Cucumber with
JUnit 5 (Jupiter) took many hours to set up and get working, resulting in a lot of anxiety and
frustration. Why is this?

1. ScalaTest is a partial implementation of BDD Methodology
   1. ScalaTest constrains the visibly to the tester, who may also be the developer
   2. Setup only requires a single test file
      1. with the necessary `import` statements
      2. along with the necessary dependencies in the build files
2. Cucumber is a more complete implementation of BDD Methodology
   1. It attempts to involve more roles into the feature design, validation, and verification
      1. involving developers, testers, product managers, etc. into the situation,
      2. requiring more files, requiring more configuration
   2. We need to
      1. Create `.feature` files using [Gherkin] that define the various *scenarios* for our features
      2. Create `step` packages in our tests (convention over configuration)
      3. Create `step` classes for our *features* that define the behavioural tests
      4. Use the same language, the same strings, in our *test* files as in our *feature* files

## Friction

None of what I have described is *friction* so far, it is just necessary discipline to support the
methodology. The friction is that, for the most part, the documentation on how to use Cucumber is abysmal.
My more cynical thoughts are that this is deliberate so that people will pay for more Cucumber support.
However, not only is the official [Cucumber Documentation] terrible, but generally everything I
Googled is terrible as well, so my impression is that this is a cultural thing, that the culture of
Cucumber does not place much value on documentation.

# Practices

While these are intended to be *best practices*, I do not have enough experience with Cucumber yet
to consider myself an expert, but I will bring other expertise into this discussion.

## Feature Files

*Who writes these?* The product manager, the solution architect, the developer, the tester, etc.?

In a sense, these are like requirements, and there are different types of requirements, defined
by different roles.

Pragmatically, in the case of the `.feature` files found in the test resources' directory,
these need to be coordinated with the actual test code, so it's more efficient if these same
person works on both sets of files, usually the test author, typically the developer.

### Two Hard Things

From [Two Hard Things]
> There are only two hard things in Computer Science: cache invalidation and naming things.
> — Phil Karlton

A primary danger in Behaviour Driven Development is that most people are not very good at
naming things (because it's hard); consequently, `.feature` files will suffer from badly named
`features`, `scenarios`, steps (`given`, `when`, `then`, `and`), etc. They are also likely to
suffer from incomplete coverage, where there are insufficient *scenarios* and *steps* defined.

### Acceptance Testing

One way to attempt to mitigate the naming problem is via Acceptance Testing. That is, when defining
a story or issue, it should be defined

    # Acceptance Testing
    1. . . .
    1. Review `.feature` files

### Definition of Ready & Done

Even better than explicit Acceptance Testing is implicitly defining ***Feature Review*** in the
organization's *Definition of Ready* and *Definition of Done*.

Before transitioning a *ticket* to *Ready* or *Done*, there should be *Review Ready* and *Review Done*
states where someone from the QA Team does feature review, including making sure naming standards,
completeness, traceability, verifiability, etc. are validated. Designs are validated, and implementations
are verified. It is important that the reviewer not be the same person who wrote the `.feature` file.
This could be undertaken in the Code Review process, such as in a Pull-Request or Merge-Request.

Of course, in most cultures, especially start-ups, this is simply too much process to consider,
but I included it here as a best practice.
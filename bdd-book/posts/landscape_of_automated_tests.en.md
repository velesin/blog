# The 3 kinds of tests you must understand to do effective TDD

## The tests are a mess!

There is a plethora of different kinds of automated tests. If we check out the [Wikipedia entry on Software Testing](http://en.wikipedia.org/wiki/Software_testing) we can find many specialized types of tests: sanity, smoke, usability, security, conformance and so on.

Similarly, the so called "test pyramid" model, popularized by [Mike Cohn](http://www.mountaingoatsoftware.com/company/about-mike-cohn) in his [Succeeding with Agile](http://www.mountaingoatsoftware.com/books/succeeding-with-agile-software-development-using-scrum) book, also has several variants, with a different number and differently named layers:

[!!!!! IMAGES HERE: !!!!!]

[http://martinfowler.com/bliki/TestPyramid.html]

[http://watirmelon.com/2011/06/10/yet-another-software-testing-pyramid/]

[http://www.thoughtworks.com/insights/blog/xdd-unplugged]


To make things even more fuzzy, there are many different kinds of tests operating at the same level. E.g. functional, acceptance, end-to-end, system and service tests are all high-level tests that treat an app as a black box, go through all layers of the app and are not isolated. The difference lies in the nuances: Does the test go through HTML interface or only through the REST API? Does the test look at the app from the end-user perspective or from more functional perspective? Does the test exercise full, longer workflows or single, more atomic user actions? The differences are often minimal, unclear and the terms are commonly mistaken, swapped and misused.

And that's not the end of the story! Some terms can have several meanings, e.g. for some people integration tests mean end-to-end tests (integration of all the parts of the system) while for other they mean narrowly focused, almost unit tests-level integration with only a single resource or module.

## Don't let it overwhelm you

Do we have to fluently navigate all this jungle to be able to effectively Test Drive a web app?

Fortunately, not.

There are 3 primary _scopes_ of testing:

- separately testing all low-level components of an app
- testing the app as a single whole, from the "outside" point of view
- testing points of connection between the app and external resources

There are several valid nomenclatures, but to set a common baseline for further discussion let's call these 3 scopes, respectively:

- unit tests
- end-to-end tests
- integration tests

Although an orientation in all the fine nuances of testing nomenclature _is_ useful, understanding the characteristics of the above 3 test types and how they complement one another will take you 80% of the way towards the solid TDD process.

However, before diving into details, let's briefly answer one question:

## Why we write tests?

There are 3 primary reasons. We want our app to:

- be as bug-free as possible
- be easy to modify
- meet user expectations

Let's see what roles the aforementioned 3 types of tests take in fulfilling these goals and what characteristics they have as a result:

## Unit tests

Unit tests satisfy the first two needs: eliminating bugs and making the app easy to modify.

What test characteristics result from these two goals?

To make us confident that an app is bug free, the tests must have **as high coverage as possible**. We need to check all the primary and alternative execution paths, edge cases, error conditions, invalid user inputs and so on.

To be able to modify the app easily we need three things:

The changes that we make must be encapsulated, free from any side effects rippling through the entire system.

The tests must be **easy to debug** and give a **precise feedback**, so that in case we make a mistake, we know immediately which part of the code has been broken and why.

And our suite must be **fast**. If it's too slow, we won't run it often, loosing the ability to work in small steps, what will make the modification more difficult.

To satisfy all these needs, our tests must be **isolated**.

Without an isolation, it is difficult to achieve a high test coverage. When looking at an app only from the outside, the number and complexity of tests grows exponentially with the number of interacting components. Assume that we have 3 components, each with 3 edge cases to check. When we test them separately, we get 3 + 3 + 3 = 9 tests. But when we test them all together, we must verify all possible combinations - so we suddenly get an explosion of 3 * 3 * 3 = 27 possible cases to cover. In any non trivial app this quickly becomes unwieldy.

The app design that makes it possible to test all of its parts separately, usually also enables changing them separately. Thus, striving for test isolation is the most straightforward heuristics for designing a side effects-free, easy to modify system.

Another thing that the isolated tests give us is a precise feedback. When all the small building blocks of the system are tested independently of each other, the tests will immediately pinpoint the exact location of a bug.

And the last but not least, isolation is the primary tool for keeping tests fast. Eliminating the dependencies - especially slow ones like data stores or file system - can speed up our suite even a few orders of magnitude.

A final implication of the test isolation is that we must take a **white box** approach (assume the knowledge of the low-level components' architecture and interfaces) and we must test at the **code level**.

## End-to-end tests

End-to-end tests satisfy the last of our 3 needs: ensuring that the app meets user expectations.

Assuring that all the separate components of the app are correct isn't enough. It doesn't automatically guarantee that their sum will also be correct.

First of all, the components may be wired together in a wrong way, so the app may not work at all.

Second, even if the app seems to work, it may not fulfill all the expectations of the user. The separate steps of the process may be correct, but the whole, longer workflow may have flaws at a higher, conceptual level.

To catch problems of this kind, we need tests that are **not isolated** - that treat an app as a whole.

What we mean by a "whole" is a bit fuzzy topic. A modern web app typically consists of 2 parts: a RESTful API backend and a "Single Page App" Javascript client. Plus, it usually connects to several web services (our own or 3rd party), data stores and message queues (local or backend-as-a-service) etc.

We must set some boundaries, which elements of this stack we treat as an integral part of our app, and which ones as independent, external systems. These boundaries may have some impact on the design of our tests (e.g. they may determine if we test through HTTP calls or through the browser automation). However, no matter where we set them, the core characteristics of our end-to-end tests will be similar:

First of all, we must treat our app as a **black box**. We must test at the **end-user interface level**, without knowledge of the app internals. And we shouldn't take any shortcuts: we shouldn't fake any parts of the code, bypass any layers, insert fixtures directly into a data store etc.

Such an approach has several further implications:

Because we always hit all the app layers in each test (including the "heavy" resources like data stores etc.), end-to-end tests are **slow**.

They are also much more **complex** - as discussed in the unit tests section, exercising all the edge cases of all interacting components requires exponentially more tests.

Another implication of hitting all layers at once is that such tests give an **imprecise feedback** and are thus **hard to debug** (they point only which high-level feature doesn't work as expected, but don't tell us which part of the code is defective).

Therefore, we should use **only as few end-to-end tests as necessary**. Usually, we exercise only primary success paths and a few most important alternative and error paths.

## Integration tests

We've already covered all our testing needs: unit tests guarantee low-level correctness and ease of modification and end-to-end tests guarantee fulfilling end-user expectations. Why do we need another kind of tests, then?

Integration tests play a supporting role. They are a specialized type of tests, focused on connection points with resources that are too "heavy" for the unit tests but have too many edge cases to be fully covered by end-to-end tests.

What are their characteristics?

While the unit and end-to-end tests are direct opposites, the integration tests lie somewhere in between, intermixing attributes from both sides:

They are almost as **isolated** as unit tests (they should exercise only a single integration point at a time), so they are similarly **easy to debug** and give as **precise feedback**.

A high level of isolation forces also a **white box** approach and writing integration tests at the **code level** - again the same as in the case of unit tests.

On the other hand, integration tests exercise "heavy" dependencies, so although they don't hit as many layers as end-to-end tests, they are still almost as **slow**. Therefore, similarly as for end-to-end tests, there should be **only as few integration tests as necessary**.

The most interesting question about integration tests is: How many are really necessary?

The naive answer could be: as many as there are "heavy" dependencies. However, it's not that simple. Some dependencies may require checking several states (e.g. a non-empty result set, empty result set, query error and time out for a web service call). On the other hand, some of these cases may be already covered by end-to-end tests. In the case of a 3rd party dependency, it depends also on your knowledge of its API and your level of trust - it may be enough for you to check only if it is "wired" correctly, or you may want to have a more thorough safety net in place. Striking an ideal balance is a broad topic, out of scope of this post.

## A simplified test pyramid

When we generalize the tests into the 3 types discussed above, the test pyramid model becomes simple:

[!!!!! IMAGE HERE !!!!!]

The pyramid consists of only two levels: a lot of unit tests at the bottom and a small number of end-to-end tests at the top.

These 2 kinds of tests complement each other. The unit tests verify all the low-level details, the end-to-end tests provide a high-level view. It is not a stylistic choice, and no kind of tests is "better". Both layers serve a different purpose and must be used together, in the correct proportions - otherwise your test suite wouldn't be fully trusted.

Integration tests are intentionally placed outside the pyramid, and marked with a dotted line. Depicting them as a middle tier of the pyramid, in-between the unit and end-to-end tests, would be in my opinion misleading.

First of all, it would suggest that there are always more integration than end-to-end tests. This is obviously not true. The number of integration tests depends on the number and complexity of the app dependencies, and there may be (and very often are) less integration than end-to-end tests. For a simple app we may even need no integration tests at all.

Second, putting them in-between unit and end-to-end tests, would suggest that they also complement these tests somehow. But the integration tests don't serve such a function - they play only a supporting role and don't participate in the close synergy between unit and end-to-end tests.

Such 2 tier test pyramid may seem over-simplified (especially from a QA specialist point of view), but if you understand it in depth, it is a sufficient model to guide you in effective TDD process.

---

What do you think about such a test pyramid? Do you have any other insights about the 3 types of tests described in the post? Please share below in the comments!

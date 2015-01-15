# The 1 law your tests are telling you to obey

TDD is not only about the correctness of the code. Equally, or even more important is how it drives a better code design. There is a tight feedback loop, a specific kind of symmetry between the production code and the tests: well designed code makes writing tests easier, therefore the tests push you in the direction of a better design.

This synergy is especially visible in case of the Law of Demeter principle.

The Law of Demeter can be jokingly summarized as "Don't talk to strangers". What this mean is that a method M can only call other methods that either:

- belong to the same object as M or to the direct dependencies of this object
- belong to the object that was created inside M or was passed to M as a parameter

In other words, we shouldn't "reach through" your dependencies and send messages directly to their "internals", what is often summarized in a "use only one dot" heuristics.

So how does it work in practice?

Let's assume we want to remind a customer about his hotel reservation in advance. We can imagine sending him a mail with a subject constructed by following method:

```js
reminderMailSubject: function(booking) {
    var city = booking.getHotel().getLocation().getCityName();
    return 'See you in ' + city + ' tomorrow!';
}
```

It doesn't look so bad at the first glance. The code seems concise and clean. It may easily skip our attention, the easier that we're used to jQuery and underscore/lodash method chaining.

However, it breaks the Law of Demeter. The method doesn't limit itself to its direct dependency (`booking`) but it reaches through it to the `hotel` object, and then even further through the `hotel` to the `location` object.

Such a code is very fragile. A change to _any_ of the objects in the `booking->hotel->location` chain will break it.

But let's see what happens when we try to write a test for such a method.

Of course we want our test to be properly isolated, so we stub all dependencies. This results in the following test:

```js
describe('reminder mail subject', function() {
    it('welcomes a guest to the city of the hotel', function() {
        var booking = {
            getHotel: function() { return {
                getLocation: function() { return {
                    getCityName: function() { return 'SomeCity'; }
                }; }
            }; }
        };
                
        expect(reminderMailSubject(booking)).to.contain('SomeCity');
    });
});
```

Ugh. It doesn't look so concise any more. The stubs' setup is really complex and ugly and this time it's rather not possible to miss that.

But let's start from the opposite direction now. If we wrote a test first, how would we want it too look like?

Probably similar to that:

```js
describe('reminder mail subject', function() {
    it('welcomes a guest to the city of the hotel', function() {
        var booking = { getDestinationCityName: function() { return 'SomeCity'; } };

        expect(reminderMailSubject(booking)).to.contain('SomeCity');
    });
}
```

Now it's neat. A single stub instead of a nested hierarchy. How do we need to write the `reminderMailSubject` method to make this test work?

We have to limit ourselves to a single level of nesting, too:

```js
reminderMailSubject: function(booking) {
    var city = booking.getDestinationCityName();
    return 'See you in ' + city + ' tomorrow!';
}
```

Hey! What a surprise! We have accidentaly satisfied the Law of Demeter! Now not only the test is easy to write, but the original method is nicer and less brittle.

This example demonstrates two things:

First, it is a nice display of the "listen to the tests" principle in action. By fixing the complicated test instead of struggling with it, we usually also improve the design of our code - what is a win-win situation.

Second, it shows how tightly the Law of Demeter is connected to the clean tests. If we struggle with a deep nesting of stubs or mocks in our test it is a clear indicator that we missed a violation of the Law of Demeter somewhere in our code. We need to find and fix it, as it'll not only make our life easier when testing but also make our code much more robust.

---

How about you? Do you remember any cases when listening to your tests helped you improve your code? Please share in the comments below!

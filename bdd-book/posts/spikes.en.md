The main TDD rule says "always write the test first".  However, let's think if there are situations where such a way is difficult or even almost impossible.

For instance, such a trouble may occur when:
- We don't have any idea how to resolve the current issue. We don't know what the code structure or output/input data should look like. In that case we don't know what to test, and what the test could look like.
- We don't know the project, framework or library that we have to use. We need to check out how it works before we decide what the test/code could look like.
- We want to try out several solutions to choose the best one.
- We want to verify if our concept is valid and will work as we expect.
- Or we just want to check out what that part of an application could look like.

In the cases mentioned above, applying the "test first" rule may be problematic (more or less - depends on the case). We need an option to skip that rule and write "code first". And we may use such an option - but only under one, very important condition.

In that situation, we may use "spikes". Spikes are no more than experiments - code that may not have tests. When we write spikes, we may write "code first". We may try out our concept, choose a solution, recognize what a test should look like etc. When writing a spike - we may feel free and code without rules. But after the experiment is finished, we must apply an important rule.

The rule is that after we finish prototyping, we're obliged to delete _all_ of the spike code and rewrite our solution using the proper TDD flow. Of course, we remember the previous solution. We're not supposed to forget the way we wrote the code before - but these thoughts shouldn't affect our TDD flow.

To be certain that a spike won't impact our proper TDD flow, it's sufficient to remember TDD rules:
- write a test before writing any code,
- write the code always in the simplest way,
- never cross the bridge until you come to it - never write any additional code not covered by tests

It is important to know that we may write a spike using TDD and "test first" method too. There aren't any contraindications against that. Sometimes it will be easier to create a spike using TDD. However, we must remember that the tests created for a spike are experimental too. So we may be more permissive when we write those tests, but the tests created in such a way must be deleted and rewritten from scratch, like the rest of the spike code.

---

And how about you - have you ever used spikes? What do you think about this method?
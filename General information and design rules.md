# General information and design rules

Here you can read general information about test case design.

## Gherkin Syntax

Writing tests based on [Gherkin](https://cucumber.io/docs/guides/overview/) forces you to think about what you are testing and describe the behavior instead of actions.

This way of writing tests isn't specific to software development, but applies to any sort of testing and experimentation. First you setup your prerequisites, so you have everything ready for the test. Next you perform the test, and finally you verify the result matches your expectation. If it matches your expectation, then your test passes. If you follow that pattern, then you are well on your way to writing proper tests.

Pattern in Gherkin syntax:

```gherkin
Given a situation
When something is performed
Then the result matches X
```

As you can see, you use 'Given' to define your prerequisites. You use 'When' to specify what you test. You use 'Then' to verify your test.

Finally, you can use 'And' in case you want to specify multiple steps of the same type:

```gherkin
Given a situation
And another prerequisite
When something is performed
Then the result matches X
```

## Test case design

Now that you know how to write in the Gherkin syntax, the next step is to define a couple of rules you need to keep in mind when writing tests. If you follow these tips, you will very likely write good quality tests.

### Test only a single thing per scenario

This means minimizing the amount of steps in the 'When' part of the test. Doing this will result in a better test result. If you test multiple things at the same time and, when verifying, the first condition fails, the test fails. This means you don't know if the second condition passes or not. If you separated this into 2 tests, then the test run will verify them separately. Another advantage is that the test scenarios stay a bit simpler.

### Every test must contain at least 1 'When' step and at least 1 'Then' step

Extending upon the previous rule to only test a single thing, you also need to have at least 1 'When' and 1 'Then' step. If you have no 'When' step, then you don't test anything, basically making the test useless. Technically you could do all the setup in the 'Given', but that defeats the purpose of the separation in the Gherkin syntax.

The same goes for the 'Then' step. If you don't verify anything, you don't know if whatever you did in the 'When' step did the correct thing. The only thing you know is that there was no error, and that doesn't mean everything was good. Maybe nothing happened at all, or the wrong thing happened — you won't know until you verify.

### Tests must be idempotent

This means that when running a test multiple times, it must give the same result. For example, let's say you have a restriction in your system that doesn't allow accounts to have a duplicate name. Your test creates an account with name 'HSO'. If you run this twice, the second time it fails with a duplicate error. To make this idempotent, you need to add a prerequisite that states that an account with name 'HSO' doesn't exist in the system.

### Each test must be completely independent from other tests

Make sure that the result of 1 test doesn't affect the result of another test. So avoid setups like 1 scenario creating an account and other scenarios using this account. This means you will sometimes get quite a few prerequisites. To counter that, you can create additional steps for defaulting.

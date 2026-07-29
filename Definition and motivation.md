# Definition and motivation

This page gives insights on the definition for automated testing and motivation to implement automated testing for any customer.

## Definition of automated testing

Automated testing is a software testing technique that enables you to define test cases that can be executed by the system instead of tests that are executed by a person who carefully needs to execute the steps required.

## Motivation for implementing automated testing

There are various reasons for implementing automated testing, we listed a few below:

- Focus: When you automate simple tests, you can focus on more complex scenarios.
- Design: Defining test cases helps in designing your solution.
- Quality: Proof of test results based on predefined test cases.
- Quality: When changing your solution, you can verify if your change impacts previous features.
- Time: The system can test your test cases overnight.
- Time: Testing all cases can be a time-consuming task when done manually.
- ... and many more...

## When is this applicable

Automated testing can be implemented for each customer, it does not matter if it's a large or small implementation.

## Common misunderstandings

There are a lot of common misunderstandings regarding automated testing or testing in general. We have explained some of these misunderstandings to assist you in making the right decisions.

### Misunderstanding: Very time consuming

If you follow proper testing procedures, then you will need to write a test script anyway. Writing a manual test script is a little simpler than automated, as you aren't bound to a specific syntax. However, after writing a few automated test scripts it may even be faster than writing manual scripts. This is due to that syntax, as it will allow a lot more templating.

Then, after you write the script, you need to manually test it. This is way slower than running the script automatically. So probably you already saved time by now. If not, then after doing a couple of retests you will have saved time for sure.

### Misunderstanding: No need for any manual testing

Almost always you won't have 100% automated test coverage, as it's very hard to get this high coverage. So most likely you will need to test some functionalities manually. Even if you would have 100% coverage, you still need to manually test the system in some way. Automated tests focus on testing functionality, but won't test perception. A process might function, but may not be very user friendly.

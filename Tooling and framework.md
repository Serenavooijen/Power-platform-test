# Tooling and framework

At HSO we have a [partnership](https://www.hso.com/nl/nieuws/hso-en-leapwork-versnellen-samen-digitale-transformatie-met-d365-door-ai-ondersteunde-testautomatisering) with the test automation platform provided by [Leapwork](https://www.leapwork.com/), which could also be applied with the Power Platform. This platform provides capabilities to automate tests in many applications. Because of that, it also has a more generic approach than the test tooling framework that is explained below.

### Framework used

When it comes to testing Power Platform specific components, we see that this usually has more focus on the properties on a form and related business logic instead of the UI. It is based on Behaviour Driven Testing. The framework below has specific capabilities to support this, therefore it is still currently the best practice.

Currently we are investigating to what extent the Leapwork platform can be used.

## SpecFlow based framework

For automated testing we use the following frameworks:

- [Microsoft EasyRepro](https://github.com/Microsoft/EasyRepro) for performing tests on Model Driven Apps
  - Microsoft EasyRepro uses [Selenium](https://www.selenium.dev/) to simulate the browser.
- [SpecFlow](https://specflow.org/) for defining structured test cases.
- [CRM.SpecFlow Toolkit](https://github.com/DynamicHands/Crm.Specflow) to link SpecFlow to Microsoft's EasyRepro and to add additional features required.

The frameworks are packaged in a Visual Studio Solution and can be run from either Visual Studio Test Suite or Azure DevOps Pipelines.

## Why we use this framework

The main reasons why we use this framework are:

- Using EasyRepro is [Microsoft's best practice regarding automated UI testing](https://learn.microsoft.com/en-us/power-apps/developer/model-driven-apps/testing-tools-client) for Power Platform and Dynamics 365 CE. This open source library provides you with building blocks to run UI tests.
- SpecFlow is a Behavior Driven Testing (BDT) framework. It allows you to write scripts in normal written language (i.e. English, Dutch or German). The scripts are written using a [Gherkin](https://cucumber.io/docs/gherkin/) syntax. SpecFlow then transforms the written text into C# code. You can find more info on writing tests [here](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/AutomatedTesting/Design/).
- Microsoft EasyRepro requires you to write your scripts in C#. This is a skill that not everyone has, and those who have the skill usually aren't software testers. The CRM.SpecFlow toolkit connects SpecFlow with EasyRepro, allowing you to write scripts in normal language which will be transformed into UI tests on Dynamics 365 CE.

With this it allows you to easily perform tests via the UI of Dynamics 365 CE.

## Behavior Driven Testing (BDT)

The goal of Behavior Driven Testing is a business readable and domain specific language that allows you to describe a system's behavior without detailing how that behavior is implemented.

This goal basically says it all. You create business readable scripts, which means your test scripts can be read (and written) by non-technical people. This greatly increases the potential of your scripts. It's easier to verify correctness, and when somebody asks: "What did you test?" you can simply send them all your scripts and they will understand.

The next major advantage is that you describe behavior instead of actions. This makes it more readable and more robust.

### Example

For example, you want to test a login procedure on a website. With actions, you will have the following script:

```
User navigates to the login page
User enters username and password
User clicks login
```

With BDT you write it like the following:

```
When the user logs in
```

The behavior driven way is way simpler to read. It's also more robust. For example, if MFA is introduced, or instead of username/password you can login with Google, then you would have to rewrite the first script while the BDT way remains the same.

For implementing Dynamics 365 CE and Power Platform apps, it may save you a lot of work rewriting scripts in case Microsoft decides to change something in the user interface. Next to that, it also allows you to perform the same test against multiple targets, like the CE UI and the CE API, without requiring 2 scripts.

## Why we do not use screen recording software

Screen recording software for UI testing seems to be a very good way to create your scripts. It's very easy to do with a very minimal learning curve. It means you can start off fast and with little effort of learning. However, over time you will run into problems:

- **Freedom:** You can record whatever you want by just clicking around without actually having to think about what you are testing. This means you will get tests which don't really test much, or test multiple things at the same time. With BDT you are forced into a certain syntax requiring you to think about what you test, and as it's text you can easily identify scripts which don't follow the BDT pattern.
- **Maintainability:** Recordings are action based and not behavior based. Let's say you have 20 tests covering the account entity and a field is moved to a different tab — then you have to re-record 20 tests. With BDT, your scripts remain the same as they don't specify where the field is located. You may only need to change a building block and then all tests work again.
- **Documentation:** Recordings are what they are — recordings, not written text. BDT scripts are business readable texts. They can, next to being test scripts, also function as a description of how functionality is supposed to work, saving you a lot of time writing documentation.
- **Traceability:** It's hard to keep track of what is changed, when, and by whom in a recording. As BDT scripts are text, this requirement is automatically solved by source control.
- **Branching:** In bigger projects you run into situations where you have some parallel development. You are changing a feature and thus need to re-record the test. You will still need the other version until that feature goes to production. This is hard to do with recordings.

## Testing other applications

Most projects have to deal with integrations to other systems, and that needs to be tested. This can be done with SpecFlow. It will require somebody to program this, similar to what the Crm.SpecFlow toolkit does to connect to Dynamics 365 CE. You can test almost anything that has a browser UI and/or API.

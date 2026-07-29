# CRM SpecFlow tips

This page contains guidelines, tips & tricks regarding usage of the framework. It's recommended to read the [General guidelines first](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/AutomatedTesting/Design/General.html).

## Use Chrome as your testing browser

It's highly recommended to use Chrome as your browser for automated testing for CE. This is by far the fastest and best supported browser. Most people who contribute to EasyRepro have tested the building blocks using Chrome.

## You can run the same scenario via both the API and browser

One of the strong points of this framework is that you can run the same scenario via both the CE API and the browser. This is very powerful as you can verify your functionality will still work properly without the JavaScript running in the browser. Doing this is really important, as a browser is not always used — for example when somebody does a data migration or integrations via Azure.

### Testing server-side business logic

If you need to test the server-side business logic, it often makes more sense to only test the API. Examples would be to test the result of a plug-in, Power Automate or classic workflow.

Performing the action via the API would not result in different behaviour than via the UI.

### Testing client-side business logic

If you need to test the client-side business logic, you can only test this via the browser. Examples would be to test the result of a script or business rule.

Performing the action via the API would not work.

## Globalization is automatically taken care of

One of the hardest things when working with dates & times is globalization. Users can have multiple timezones which can cause issues. The timezone of your test user is automatically incorporated in relation to the timezone of the computer running the test, so that it always works out.

## Logical vs display names

In the scenarios you can use both schema and display names to specify which column you refer to. The recommendation is to use display names whenever possible. This means you will have a little more maintenance on your scenarios, as they stop working when a display name changes, but that's worth it compared to the improvement in readability it gives. It also improves the speed of writing tests, as the chance of knowing the display name of a column by heart is way higher than knowing the logical names. Logical names should be used when you have multiple columns with the same display name. This should only happen in exceptional cases, as it's best practice to not have any duplicate display names.

## Add the cleanup tag to automatically delete data created by the test

Always add the cleanup tag, as it will try to delete any data created by the test. This will keep the environment clean. Keep in mind that sometimes, due to business logic, data can't be deleted. If that happens, you can see that in the logging. Tests won't fail on this as it's not part of the test scenario.

# Component Library

## Introduction to Component Libraries

Canvas Apps offer great versatility in terms of functionality and design, but you will catch yourself making the same elements repeatedly. A header, a modal, a menu, a button with perhaps an HTML element with box-shadow underneath to make a more modern/material design look. This takes precious time. Now, there is an even better way.

Component libraries are best seen as a repository, a one-stop-shop for all the previously made elements which you can pick and choose from. The library is available to every app maker within your environment. This means that you can easily ensure that apps will look and function in similar ways. Branding will become easier. Making an app that looks familiar will become easier. And pushing updates of the components to dependent apps will also become easier. Earlier, components were app specific. This meant that with every change to the component you had to individually update related components in other apps. With the use of a component library, app makers receive a notification of a new component version available and if they would like to upgrade. Much easier.

In short, Component libraries are containers of component definitions that make it easy to:

- Discover and search components.
- Publish updates.
- Notify app makers of available component updates.

Component libraries are the recommended way to reuse components across apps. When using a component library, an app maintains dependencies on the components it uses. The app maker will be alerted when updates to dependent components become available. Hence, all new reusable components should be created within the component libraries instead. An earlier Power Apps feature that allowed importing components from one canvas app to another is retired.

## Definition of insanity for Custom Components for Canvas Apps

If not governed in a controlled way, adding new components can end up resulting in multiple instances of the same components in the library. Eventually, users may end up using different components from the same library which are actually the same component. It defeats the purpose. Adequately understanding and governing component libraries can help companies achieve uniform component utilization, saving time in remaking something, and thereby having uniform branding and styling.

## Why Use A Power Apps Component Library

Components can be stored inside a component library or inside an app. Most times you will want to choose a component library for these 3 reasons:

1. **Available To Use In Other Apps** — Any component in a component library can be used in other apps, whereas an independent component stored inside an app cannot. The main benefit of a component library is to save time by re-using components in as many places as possible.
2. **Ease Of Updating Across Many Apps** — When changes are made to a component inside a component library, the developer can open any app where it is being used and a prompt will appear asking whether it should be updated.
3. **Share With Others** — By saving components in a component library, they will be easier for others to find. Component libraries have their own menu separate from apps. This makes them more discoverable.

## Governance, Sharing, Accessibility for Component Library

Sharing a component library works the same way you share a canvas app.

### Things to keep in mind

1. When you share a component library, you allow others to reuse the component library.
2. Once shared, others can edit the component library and import components from this shared component library for creating and editing apps.
3. If shared as a co-owner, a user can use, edit, and share a component library, but not delete it or change the owner.
4. You can't add existing component libraries to a solution. However, you can create new component libraries for solutions using the add component library flow.
5. You can't access controls in the component from outside of the component.
6. You can't refer to anything outside of the component from inside the component. The exception is data sources shared between an app and its components.

## Creating, importing, versioning and known limitations

For instructions on how to create, import, and version component libraries, and for known limitations, please refer to Microsoft Docs on Component Libraries. Check the following guidelines from Microsoft regarding views: https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/component-library

## Component Library Best Practices

Header, footer and themes are an important and reusable part of canvas apps. It is advisable to standardize these aspects of an app when you start building an app. Before creating a new library, assess if there is an already existing library that can be used. Use these standard component libraries for any new apps when applicable. Ensure that the administration/ownership of the app is limited to a small group of people to avoid impacting other apps that use the library.

When standardizing these libraries, the following aspects related to a company need to be kept in mind:

- Define your theme — Background Color
- Header: Color, Font, Default Text
- Footer: Color, Font, Default Text
- Company Logo

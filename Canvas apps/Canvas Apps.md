
# Canvas Apps

Canvas apps are a type of app you can create within Power Apps, part of the Microsoft Power Platform. Unlike Model-driven apps that are based on data models and relational structures, Canvas apps offer a more flexible and visually driven development approach. In Canvas apps, you design the user interface by dragging and dropping controls (like buttons, galleries, forms, and images) onto a blank canvas, hence the name "Canvas."

Canvas apps allow you to create highly customized applications that connect to a variety of data sources, including Microsoft Dataverse, SharePoint, SQL Server, Excel, and many third-party APIs. These apps can be used to automate tasks, gather data, or create complex business processes without requiring advanced programming knowledge.

See also: [Microsoft Canvas App guidelines](https://pahandsonlab.blob.core.windows.net/documents/PowerApps%20canvas%20app%20coding%20standards%20and%20guidelines.pdf)

## Key Features of Canvas Apps

- **Fully Customizable Interface:** The canvas provides complete control over the design and layout. You can arrange elements freely, similar to designing a PowerPoint slide.
- **Wide Range of Data Connectors:** Canvas apps support a broad set of connectors to various data sources such as SharePoint, SQL Server, Excel, Microsoft 365, and even third-party services.
- **Formula-Based Logic:** You can create complex business logic and interactions using Power Fx, a low-code formula language built for Power Apps.
- **Responsive Design:** With responsive layouts, you can ensure that your app works well on any device, whether a desktop, tablet, or mobile phone. (Note: this is only available out of the box using the Microsoft Templates. Building this yourself takes more effort.)
- **Cross-Platform Deployment:** Once developed, Canvas apps can be deployed to multiple platforms, including mobile apps (iOS/Android) and web browsers.

## Why Use Canvas Apps?

Canvas apps provide an intuitive, no-code or low-code way to create applications, making them accessible to a broad range of users, including business analysts, IT professionals, and citizen developers. Below are some key reasons why you might choose to use Canvas apps:

- **Flexibility in Design:** You can design apps exactly the way you want, with full control over the layout, color schemes, fonts, and the overall user experience.
- **Integration with Microsoft Ecosystem:** Canvas apps are well-integrated with other Microsoft services, such as Power Automate (for automation), Power BI (for reporting), and Microsoft Dataverse (for data storage).
- **Speed of Development:** With pre-built templates, a large library of controls, and an intuitive drag-and-drop interface, you can create apps faster compared to traditional software development.
- **Cost-Effective:** Canvas apps can save time and resources, especially for organizations looking to digitize processes without investing in custom-built software.
- **Accessibility for Non-Developers:** Canvas apps are built with the goal of enabling users who may not have traditional programming skills to build and maintain apps.

## Use Cases for Canvas Apps

Canvas apps are ideal for various scenarios, especially where business process automation, data capture, and customized user interfaces are essential. Some common use cases include:

1. **Data Collection and Reporting** — Build apps to collect data from users or devices and store it in databases like SharePoint, SQL Server, or Dataverse. Example: a field service app that collects inspection data and uploads it in real-time.
2. **Task and Workflow Automation** — Integrate with Power Automate to trigger workflows based on user input. Example: an approval app where users can submit requests for approval, and the system automatically triggers approval workflows.
3. **Business Process Management** — Customize workflows and forms that reflect business processes unique to your organization. Example: an inventory management app where employees can update stock levels, generate reports, and track items across locations.
4. **Customer Engagement and Interaction** — Develop customer-facing apps to capture feedback, track customer interactions, or facilitate order management. Example: a customer service app that allows agents to log tickets, track customer issues, and escalate them as needed.
5. **Internal Tools and Dashboards** — Create internal apps that serve as dashboards or tools for team collaboration and decision-making. Example: a sales dashboard app that aggregates data from multiple sources and displays key metrics in real-time.

## Building Blocks of Canvas Apps

Canvas apps are composed of several building blocks that allow developers to customize both the functionality and appearance of their applications:

- **Screens:** The primary containers for your app's user interface. Each screen can represent a different part of your app, such as a home page, data entry form, or report.
- **Controls:** Visual elements (buttons, labels, text boxes, images, etc.) that users interact with. Controls can be added to the canvas and customized using properties and formulas.
- **Data Sources:** External data sources that Canvas apps connect to, such as SharePoint lists, SQL databases, or REST APIs. These data sources serve as the backend for your app.
- **Formulas:** Logic written in Power Fx, the low-code language of Power Apps. Formulas allow you to define actions, handle events (like button clicks), perform data manipulation, and more.
- **Variables and Collections:** Variables store data for use within the app, while collections are more complex data structures that can store tables or lists of records temporarily.
- **Themes and Customization:** You can apply pre-built themes or create custom styles to maintain consistent branding across your app.

## The Canvas App Design Experience

The Power Apps Studio is the primary environment for building and managing Canvas apps. It's a drag-and-drop interface that allows you to easily add controls and design the layout of your app. Key features include:

- **Canvas Design Area:** A blank area where you add and arrange controls, design the user interface, and set up app logic.
- **Properties Pane:** This allows you to modify the properties of controls (such as color, size, and behavior) and define their formulas.
- **Formula Bar:** Located at the top of the Power Apps Studio, this is where you write Power Fx formulas to define the logic of your app.
- **Preview Mode:** A feature that allows you to test and interact with the app during the development process to ensure everything works as intended.

## Topics

The following topics are handled separately in the best practices:

- [Component Library](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/componentlibrary.html)
- [Concurrent Function](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/ConcurrentFunction.html)
- [Delegation](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/Delegation.html)
- [Data sources](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/DataSources.html)
- [Naming Conventions](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/namingconventions.html)
- [Variable usage](https://docs.hso.com/PowerPlatform/Apps/CanvasApps/variableUsage.html)

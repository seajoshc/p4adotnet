# AI Product Manager

For now using this as a scratchpad and log for my ChatGPT-4 AI Product Manager.

## 2023-03-16
Prompts:

```
I'm a product manager at a software as a service company. We are building a new product called Cookiepuss that allows software developers to track, manage, and get data & insights about all the software projects and micro-services they have running. One of the things we expect customers to use our product for is to create "templates". A template allows other users to then create new software projects based off the template: it has boilerplate (hello world! style)  code, best practices, and standards for things like naming or CI/CD. Do you think software templates are important to software developers?
```

```
Pretend you are a product manager working on Cookiepuss with me. Let's design the new software templates feature together. I think we should start by defining some goals and requirements for our first deliverable. 

Target customer, audience, or persona: software developers

A software template has these properties:
1. A human friendly name for the template and a description about it.
2. A git repository URL. This git repository is the foundation of the template. Inside the repository we expect things like: boilerplate code that produces a "Hello, World!" application in whatever language and framework the template is made up of (e.g. Ruby on Rails, or Java with Spring), default repository settings like default branch name or permissions, standardized CI/CD workflows pre-configured with best practices, etc.
3. An optional set of parameters that allow for customization of the new software project being created from the template. The user defines a series of key-value pairs that represent the input requested.
4. An optional webhook URL. After the template is executed Cookiepuss will send (HTTP POST) a JSON payload to this URL, if defined. The payload contains the values of all entered parameters (if applicable) and also a few standard/default key-value pairs: "action" with a value of "componentCreatedFromTemplate", "templateName" with a value of the name of the template being used, "date" the date timestamp of when template execution occurred, and "user" which is the username of the user that executed the template.

Goals:
1. Customers (aka users) can create a new software template in Cookiepuss' UI. "Templates" will be a new top navigation menu item; in other words, it will have its own logical container and boundary within the UI making it clear to the customer they are either creating or using templates.
2. Customers can create new software projects from a template in Cookiepuss' UI. Customers navigate to the "Templates" screen and choose from a template they see listed. They click a button that says "Create  new project from template" which takes them through a modal wizard that asks them for the name and description first. Then it asks them to provide values to any parameters requested by the template. Then the customer hits Create.
3. After the customer hits Create in the previous step, Cookiepuss will do the following: fork the git repository defined in the template to a new repository using the name provided in the previous step and then register the new software project within Cookiepuss' "component catalog" which tracks all software projects a customer has.

Can you start by summarizing what we're building?
```

```
Well done. Let's add some more pizzaz to that summary. Can you please re-write the summary as a one paragraph marketing blurb.
```

```
Could you help me write some user stories based on our goals and requirements? I want to start breaking down some of the work for our engineering team.
```
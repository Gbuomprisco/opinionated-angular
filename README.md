## An internal style guide for writing code is an important decision that any development team should define and agree on at some point, ideally early on in the project.

*This article was originally published on [Bits and Pieces](https://blog.bitsrc.io) by [Giancarlo Buomprisco](https://medium.com/@.gc)*

If you’ve been writing code professionally, you know very well how important style is for many, many developers. Countless hours in my career have been spent arguing about style.

Why is it so important, though? **Programmers read code way more than they write**: it’s crucial that we simplify this task as much as possible for us but especially for our fellow teammates.

The consensus is to define a style guide before writing the first line of code, but this should not be fixed for the entire project lifecycle: **it is a continuous set of learnings deriving from experimentation and experience.**

It also doesn’t mean you should change your mind every day: it means you should evaluate, discuss and decide with your team as your project grows.

After writing Angular apps since the alpha days, I’ve developed my style, strongly influenced by people I’ve worked with, reading lots of people’s code, and simply experimenting with my projects.

In this article, I want to show how I style my Angular apps and the rationale behind my decisions. Hopefully, it will inspire you and your team to adopt some of it or to make your own.

Any suggestions would be extremely welcome on how to improve it!


**Notice**: this style guide is purely stylistic not based on technical details and best practices. This style guide is intended to **simply help with code aesthetics and readability**, not performance, design patterns or else.

## HTML Wrapping and Order

Angular templates have quite a few syntax additions on top of normal HTML, and sometimes they’re not very easy to read.

My first suggestion regards the wrapping. I normally **do not exceed 80 characters per column** for all files: it’s simply much easier to read vertically than horizontally.

This is an element written without any convention:

![](https://cdn-images-1.medium.com/max/4096/1*OSGrVXJZrUKE0kragUxzkA.png)

Messy, isn’t it? Almost every project I’ve worked on while consulting was written in a similar fashion.

We’re going to rewrite the snippet above using a set of simple rules to make it way more readable.

### Defining Rules For Writing HTML Tags

* When an element has two or more attributes, I normally **only write one attribute per line**

* Attributes have to be written **in a specific order**

* Unless using a single attribute, the closing tag has to be written **on the next line**

![](https://cdn-images-1.medium.com/max/2000/1*fdCeXlyeroegvXDL4t6Q3A.png)

I suggest defining a specific order:

* 1. Structural directives

* 1. Animations

* . Static properties

* 4. Dynamic properties

* 5. Events

Let’s see an example of how I would personally write the previous example:

![](https://cdn-images-1.medium.com/max/2888/1*FW_aK1ASTl3GCtacJ0WdkQ.png)

Even better, I’d always use structural directives exclusively with ng-container:

![](https://cdn-images-1.medium.com/max/2888/1*uxEVM22JmR2JeR4RKyKRng.png)

While I think you can mix up the order of the attributes based on a subjective view, I feel quite strong about **displaying structural directives before anything else**.

A structural directive tells me (before I need to know anything else it does):

* Is this field being shown? And why?

* Is this field being repeated?

In my opinion, this can facilitate reading through and understanding the structure of your templates.

## Pipes

Pipes are very powerful: they can transform the values in templates and avoid duplication/logic in our components. They can be reused and mixed easily, and are easy to write.

But are they easy to read and to spot? Yes and No.

This is highly subjective and a minor point but I still think it can be valuable to share: whenever I see a pipe in my template, I tend to wrap them within parenthesis. The feeling of division provided by the parenthesis gives me a clue that the value is being transformed and generally is easier on the eye:

![](https://cdn-images-1.medium.com/max/2000/1*Bi5BVprv9s_wCnBAq0ublA.png)

When using multiple pipes, it may even be more important:

![](https://cdn-images-1.medium.com/max/2000/1*WJsTeLNok5M7flf7dMHE2w.png)

## Lifecycle Hooks

### Interfaces

Adding lifecycle hooks interfaces is not mandatory but a suggested practice, which I strongly recommend to follow.

### Order

When I look for lifecycle hooks, I usually head to the constructor and expect them to see all of them together and not mixed up with other class methods. Ideally, they should be defined in the same order they execute.

What I recommend is:

* always add interfaces

* add public and private properties above the constructor

* add methods right below the constructor and above the component’s methods

* add them all close to each other

* add them in the order they execute. Admittedly though, this is a bit harder to follow consistently, so I guess it’s the least important one

![](https://cdn-images-1.medium.com/max/2492/1*TgTzECqW7WzMBvpm0x0mIA.png)

### Logic

I normally avoid directly writing any logic within the lifecycle hooks: my suggestion is to encapsulate logic within private methods and call them within the lifecycle hooks:

![](https://cdn-images-1.medium.com/max/2000/1*v06_dzz_36OiXdddKufhvw.png)

## Component Properties and Methods

Angular uses decorators for the component’s methods and properties in order to augment its functionality.

There are so many of them that defining a specific order to follow would be overwhelming, but the important thing I try to follow is to locate the properties and methods with the same decorator close to each other.

The following is what I would consider a bad example:

![](https://cdn-images-1.medium.com/max/2316/1*1O2RFk_3068CwfotLAmFSA.png)

And below is how I would write it; also, notice there’s an empty line in between groups of properties with the same decorator — I think it helps with readability:

![](https://cdn-images-1.medium.com/max/2316/1*uWXSp-qhoZY5n_Cdk6bQ-w.png)

I don’t have a strong opinion on this, but try to locate private and public component properties not marked with any decorator separately from the decorated properties.

In my experience, mixing them up only leads to confusion and a feeling of chaos.

## Naming

Oh, naming things is hard, I know.

When it comes to naming, I always have to think it twice or more to come up with a name that is understandable, unambiguous and easy-to-search:

* *understandable*: what does this do, at a glance?

* *unambiguous*: for example, if we have multiple click events on a single component, which one does this event refer to? So yeah, naming it *onClick* is **not** the way to go

* *easy-to-search*: I see naming code a bit like SEO: how will my users (teammates or me) search for this particular thing — and how can I write it to make sure they can search for it more easily?

### File Names

I like to use hyphen-case for all file names. I would think by now it’s a standard for Typescript projects, but I’ve seen quite a few variations, even in Angular projects, so I feel I have to mention this.

Examples:

* sign-up.component.ts

* profile-form.component.html

### Route Components

I tend to name route components with a suffix page.

For example, the authentication page would normally be called auth-page.component.ts — which tells me it’s a routed component, and I normally use it to wrap and display other components via router-outlet.

### Components

Some rules I tend to follow when naming components are:

* Try to use **no more than 3 words** (excluding the prefix). No specific reason why — they just don’t look very pretty. Of course, sometimes it’s simply not very easy to respect this rule

* Try to **avoid repeating words or contexts already used** with other components, as it would slow down while using the search function of my IDE, and also lead to mistakenly open other files, which is ultimately a waste of time and source of frustration

* At the same time, **also try not to be too generic**. For example: if we call a component settings — settings of what!? Help out a little here and give some further context (example: application-settings, profile-settings, organization-settings, etc.). 
No biggie for small applications, but at scale, it does make a difference

### Events Names

It seems simple and yet it’s not, especially with larger components with many events.

Here are a set of rules I try to follow:

* Do not prefix events/outputs names with on. The handler, instead, could be written with such prefix

* Don’t make me think: always specify the entity whose action referred, not only the action itself. 
If we are describing an event on a component whose value changed, the event change could be valueChange.
In my opinion, this is unambiguous and tells me what changed without having me question if this was the value, the status, or anything else

* Use past-sense or not (valueChange vs valueChanged)? This is controversial and I heard valid reasons on opposite sides, so it may be up for discussion for your and your team. 
As long as you agree on one single-way, I don’t think it’s *that* important. What do you think?

## ES Imports

Keeping your file imports ordered and neat is challenging, especially when using an IDE to auto-add them as you type. As your files grow, they tend to get quite messy.

This how I order my imports:

* 1. Angular imports always go at the top

* 2. Rx imports

* 3. Third parties (non-core)

* 4. Local/Project imports at the end

It’s also a good practice to leave a comment above each group:

![](https://cdn-images-1.medium.com/max/2596/1*cXI7qASNN8LUNUr4RUlcOg.png)
